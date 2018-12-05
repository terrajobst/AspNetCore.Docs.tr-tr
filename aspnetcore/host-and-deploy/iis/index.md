---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1680b1377351fbfbfc38249868da389012dd5fb6
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862193"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="1bde5-103">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="1bde5-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="1bde5-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1bde5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="1bde5-105">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="1bde5-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

> [!NOTE]
> <span data-ttu-id="1bde5-106">ASP.NET Core içindekiler tablosuna yönelik önerilmiş olan yeni bir yapının kullanılabilirliğini test ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="1bde5-106">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="1bde5-107">Geçerli veya önerilen içindekiler tablosunda 7 farklı konuyu bulmaya ilişkin alıştırmayı denemek için vaktiniz varsa lütfen [çalışmaya katılmak için buraya tıklayın](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="1bde5-107">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="1bde5-108">Desteklenen işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="1bde5-108">Supported operating systems</span></span>

<span data-ttu-id="1bde5-109">Aşağıdaki işletim sistemleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-109">The following operating systems are supported:</span></span>

* <span data-ttu-id="1bde5-110">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1bde5-110">Windows 7 or later</span></span>
* <span data-ttu-id="1bde5-111">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1bde5-111">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1bde5-112">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)) IIS ile bir ters proxy yapılandırması çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="1bde5-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="1bde5-113">Kullanım [Kestrel sunucu](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1bde5-113">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="1bde5-114">Azure'da barındırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="1bde5-114">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="1bde5-115">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="1bde5-115">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="1bde5-116">IISIntegration bileşenlerini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="1bde5-116">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1bde5-117">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="1bde5-117">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1bde5-118">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="1bde5-118">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1bde5-119">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="1bde5-119">**In-process hosting model**</span></span>

<span data-ttu-id="1bde5-120">`CreateDefaultBuilder` çağrıları `UseIIS` önyükleme yöntemi [CoreCLR](/dotnet/standard/glossary#coreclr) ve IIS çalışan işlemi uygulama barındırın (*w3wp.exe* veya *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="1bde5-120">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="1bde5-121">Performans testleri belirten bir .NET Core uygulaması işlem içi barındırma için uygulama işlem dışı ve proxy isteklerini barındırma kıyasla önemli ölçüde daha yüksek istek üretilen işini teslim [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-121">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="1bde5-122">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="1bde5-122">**Out-of-process hosting model**</span></span>

<span data-ttu-id="1bde5-123">IIS ile işlem dışı barındırmak için `CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1bde5-123">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="1bde5-124">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-124">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1bde5-125">`CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-125">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="1bde5-126">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="1bde5-126">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="1bde5-127">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-127">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="1bde5-128">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-128">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="1bde5-129">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="1bde5-129">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="1bde5-130">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1bde5-130">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1bde5-131">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-131">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="1bde5-132">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirttiğiniz bağlantı noktalarındaki Kestrel dinlediği.</span><span class="sxs-lookup"><span data-stu-id="1bde5-132">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="1bde5-133">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1bde5-133">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="1bde5-134">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1bde5-134">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="1bde5-135">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-135">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1bde5-136">`CreateDefaultBuilder` çağrıları [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-136">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="1bde5-137">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="1bde5-137">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="1bde5-138">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-138">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="1bde5-139">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-139">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="1bde5-140">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="1bde5-140">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="1bde5-141">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1bde5-141">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1bde5-142">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-142">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="1bde5-143">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1bde5-143">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1bde5-144">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1bde5-144">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="1bde5-145">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-145">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1bde5-146">`CreateDefaultBuilder` çağrıları [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-146">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="1bde5-147">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="1bde5-147">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="1bde5-148">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-148">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="1bde5-149">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-149">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="1bde5-150">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="1bde5-150">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="1bde5-151">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1bde5-151">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1bde5-152">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-152">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="1bde5-153">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1bde5-153">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1bde5-154">Bir bağımlılık dahil [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıklarını bir pakette.</span><span class="sxs-lookup"><span data-stu-id="1bde5-154">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="1bde5-155">Ekleyerek IIS tümleştirme Ara yazılımları kullanmayı [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) genişletme yöntemi için [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span><span class="sxs-lookup"><span data-stu-id="1bde5-155">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="1bde5-156">Her ikisi de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) ve [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-156">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="1bde5-157">Kod arama `UseIISIntegration` kod taşınabilirliği etkilemez.</span><span class="sxs-lookup"><span data-stu-id="1bde5-157">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="1bde5-158">Uygulamanın IIS çalıştırırsanız değildir (örneğin, uygulamayı doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="1bde5-158">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="1bde5-159">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-159">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1bde5-160">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="1bde5-160">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="1bde5-161">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-161">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="1bde5-162">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-162">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="1bde5-163">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1bde5-163">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1bde5-164">Bir çağrı `UseUrls` Modülü'nü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-164">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="1bde5-165">Varsa `UseUrls` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1bde5-165">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="1bde5-166">`UseUrls` Olan bir ASP.NET Core 1.0 uygulamada çağrılır, çağrı **önce** çağırma `UseIISIntegration` böylece modül yapılandırılan bağlantı noktası üzerine değil.</span><span class="sxs-lookup"><span data-stu-id="1bde5-166">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="1bde5-167">Ayarı modülü geçersiz kıldığından bu arama sırası ASP.NET Core 1.1 ile gerekli değildir `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-167">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="1bde5-168">Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core ana](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="1bde5-168">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="1bde5-169">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="1bde5-169">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1bde5-170">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="1bde5-170">**In-process hosting model**</span></span>

<span data-ttu-id="1bde5-171">IIS sunucusu seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) içinde [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="1bde5-171">To configure IIS Server options, include a service configuration for [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="1bde5-172">Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="1bde5-172">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="1bde5-173">Seçenek</span><span class="sxs-lookup"><span data-stu-id="1bde5-173">Option</span></span>                         | <span data-ttu-id="1bde5-174">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="1bde5-174">Default</span></span> | <span data-ttu-id="1bde5-175">Ayar</span><span class="sxs-lookup"><span data-stu-id="1bde5-175">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="1bde5-176">Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1bde5-176">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="1bde5-177">Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-177">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="1bde5-178">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-178">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="1bde5-179">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1bde5-179">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="1bde5-180">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1bde5-180">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="1bde5-181">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="1bde5-181">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="1bde5-182">IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) içinde [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="1bde5-182">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="1bde5-183">Aşağıdaki örnek uygulamayı doldurmasını önler `HttpContext.Connection.ClientCertificate`:</span><span class="sxs-lookup"><span data-stu-id="1bde5-183">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="1bde5-184">Seçenek</span><span class="sxs-lookup"><span data-stu-id="1bde5-184">Option</span></span>                         | <span data-ttu-id="1bde5-185">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="1bde5-185">Default</span></span> | <span data-ttu-id="1bde5-186">Ayar</span><span class="sxs-lookup"><span data-stu-id="1bde5-186">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="1bde5-187">Varsa `true`, IIS tümleştirme ara yazılımı ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1bde5-187">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="1bde5-188">Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-188">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="1bde5-189">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-189">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="1bde5-190">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-190">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="1bde5-191">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1bde5-191">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="1bde5-192">Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-192">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1bde5-193">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="1bde5-193">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1bde5-194">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-194">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="1bde5-195">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-195">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="1bde5-196">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="1bde5-196">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="1bde5-197">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="1bde5-197">web.config file</span></span>

<span data-ttu-id="1bde5-198">*Web.config* dosya yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1bde5-198">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1bde5-199">Oluşturma, dönüştürme ve yayımlama *web.config* dosya bir MSBuild hedefi tarafından gerçekleştirilir (`_TransformWebConfig`) projeyi ne zaman yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-199">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="1bde5-200">Bu Web SDK'sı hedeflerin mevcut hedefidir (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="1bde5-200">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="1bde5-201">SDK'sı, proje dosyasının en üstüne ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="1bde5-201">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="1bde5-202">Varsa bir *web.config* dosyası projedeki mevcut değil, dosyanın doğru oluşturulması *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modül](xref:fundamentals/servers/aspnet-core-module) ve taşınabilir [yayımlanmış çıktısı](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="1bde5-202">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="1bde5-203">Varsa bir *web.config* dosyası projesinde, dosyanın doğru dönüştürülür *processPath* ve *bağımsız değişkenleri* ASP.NET Core modülü yapılandırmak için ve azure'a taşındı yayımlanmış çıktısı.</span><span class="sxs-lookup"><span data-stu-id="1bde5-203">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="1bde5-204">Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="1bde5-204">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="1bde5-205">*Web.config* etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları dosyası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-205">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="1bde5-206">ASP.NET Core uygulamaları istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-206">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="1bde5-207">Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın  **\<IsTransformWebConfigDisabled >** özelliği proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="1bde5-207">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="1bde5-208">Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-208">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="1bde5-209">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1bde5-209">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="1bde5-210">Web.config dosyası konumu</span><span class="sxs-lookup"><span data-stu-id="1bde5-210">web.config file location</span></span>

<span data-ttu-id="1bde5-211">Ayarlamak için [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) doğru *web.config* dosya dağıtılan uygulamayı içerik kök yolda (genellikle uygulama temel yolu) mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-211">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="1bde5-212">IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-212">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="1bde5-213">*Web.config* dosya, Web dağıtımı kullanarak birden fazla uygulama yayımlamayı etkinleştirmek için uygulamanın kök dizininde gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-213">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="1bde5-214">Mevcut uygulamanın fiziksel yola gibi hassas dosyalar  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="1bde5-214">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="1bde5-215">Zaman *web.config* dosya varsa ve ve site normalde başlatır, bunların istenmesi halinde, IIS bu hassas dosyalar hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="1bde5-215">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="1bde5-216">Varsa *web.config* dosyası eksik, yanlış adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet verebilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-216">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="1bde5-217">***Web.config* doğru adlı, dağıtımdaki her zaman mevcut ve yukarı site normal başlangıç için yapılandırmak için dosya olmalıdır. Hiçbir zaman Kaldır *web.config* dosyasından bir üretim dağıtımı.**</span><span class="sxs-lookup"><span data-stu-id="1bde5-217">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="1bde5-218">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="1bde5-218">IIS configuration</span></span>

<span data-ttu-id="1bde5-219">**Windows Server işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="1bde5-219">**Windows Server operating systems**</span></span>

<span data-ttu-id="1bde5-220">Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-220">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="1bde5-221">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-221">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="1bde5-222">Üzerinde **sunucu rolleri** kutusunu işaretleyin, adım **Web sunucusu (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-222">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="1bde5-224">Sonra **özellikleri** adım **rol hizmetlerini** adım, Web sunucusu (IIS) yükler.</span><span class="sxs-lookup"><span data-stu-id="1bde5-224">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="1bde5-225">İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-225">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="1bde5-227">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="1bde5-227">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="1bde5-228">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-228">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="1bde5-229">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="1bde5-229">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="1bde5-230">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1bde5-230">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="1bde5-231">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="1bde5-231">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="1bde5-232">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-232">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="1bde5-233">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-233">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="1bde5-234">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="1bde5-234">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="1bde5-235">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="1bde5-235">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="1bde5-236">Aracılığıyla devam **onay** Hizmetleri ve web sunucusu rolü yüklemek için adım.</span><span class="sxs-lookup"><span data-stu-id="1bde5-236">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="1bde5-237">Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değilse **Web sunucusu (IIS)** rol.</span><span class="sxs-lookup"><span data-stu-id="1bde5-237">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="1bde5-238">**Windows masaüstü işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="1bde5-238">**Windows desktop operating systems**</span></span>

<span data-ttu-id="1bde5-239">Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-239">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="1bde5-240">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="1bde5-240">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="1bde5-241">Açık **Internet Information Services** düğümü.</span><span class="sxs-lookup"><span data-stu-id="1bde5-241">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="1bde5-242">Açık **Web yönetimi araçları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="1bde5-242">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="1bde5-243">İçin kutuyu **IIS Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-243">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="1bde5-244">İçin kutuyu **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-244">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="1bde5-245">Varsayılan özelliklerini kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-245">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="1bde5-246">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="1bde5-246">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="1bde5-247">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-247">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="1bde5-248">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="1bde5-248">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="1bde5-249">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1bde5-249">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="1bde5-250">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="1bde5-250">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="1bde5-251">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-251">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="1bde5-252">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-252">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="1bde5-253">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="1bde5-253">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="1bde5-254">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="1bde5-254">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="1bde5-255">IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-255">If the IIS installation requires a restart, restart the system.</span></span>

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="1bde5-257">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="1bde5-257">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="1bde5-258">Yükleme *.NET Core barındırma paket* barındıran sistemde.</span><span class="sxs-lookup"><span data-stu-id="1bde5-258">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="1bde5-259">.NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1bde5-259">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1bde5-260">Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="1bde5-260">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="1bde5-261">Sistem, Internet bağlantısı yoksa, alma ve yükleme [Microsoft Visual C++ 2015 yeniden dağıtılabilir](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core barındırma paketini yüklemeden önce.</span><span class="sxs-lookup"><span data-stu-id="1bde5-261">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1bde5-262">Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-262">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="1bde5-263">IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-263">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="1bde5-264">Doğrudan indirme (geçerli sürüm)</span><span class="sxs-lookup"><span data-stu-id="1bde5-264">Direct download (current version)</span></span>

<span data-ttu-id="1bde5-265">Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:</span><span class="sxs-lookup"><span data-stu-id="1bde5-265">Download the installer using the following link:</span></span>

[<span data-ttu-id="1bde5-266">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="1bde5-266">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="1bde5-267">Yükleyici önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="1bde5-267">Earlier versions of the installer</span></span>

<span data-ttu-id="1bde5-268">Yükleyici önceki bir sürümünü almak için:</span><span class="sxs-lookup"><span data-stu-id="1bde5-268">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="1bde5-269">Gidin [.NET indirme arşivleri](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="1bde5-269">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="1bde5-270">Altında **.NET Core**, .NET Core sürümünüzü seçin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-270">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="1bde5-271">İçinde **uygulamaları - çalışma zamanı çalıştırma** sütun, istenen .NET Core çalışma zamanı sürümü satırını bulur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-271">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="1bde5-272">Kullanarak yükleyiciyi indirin **çalışma zamanı & paketi barındırma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="1bde5-272">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="1bde5-273">Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-273">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="1bde5-274">Daha fazla bilgi için [Destek İlkesi](https://www.microsoft.com/net/download/dotnet-core/2.0).</span><span class="sxs-lookup"><span data-stu-id="1bde5-274">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="1bde5-275">Barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="1bde5-275">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="1bde5-276">Sunucuda yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-276">Run the installer on the server.</span></span> <span data-ttu-id="1bde5-277">Yükleyiciyi bir yönetici komut isteminden çalıştırırken aşağıdaki anahtarlar kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-277">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="1bde5-278">`OPT_NO_ANCM=1` &ndash; ASP.NET Core modülü yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="1bde5-278">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="1bde5-279">`OPT_NO_RUNTIME=1` &ndash; .NET Core çalışma zamanı yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="1bde5-279">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="1bde5-280">`OPT_NO_SHAREDFX=1` &ndash; ASP.NET paylaşılan Framework (ASP.NET çalışma zamanı) yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="1bde5-280">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="1bde5-281">`OPT_NO_X86=1` &ndash; X86 yükleme atlanıyor çalışma zamanları.</span><span class="sxs-lookup"><span data-stu-id="1bde5-281">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="1bde5-282">32-bit uygulamaları barındırma gerekmez, bildiğiniz durumlarda bu anahtarı kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-282">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="1bde5-283">Hem 32 bit hem de 64-bit uygulamaları gelecekte barındıracak ihtimali varsa, yoksa bu anahtarı kullanın ve her iki çalışma zamanları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-283">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="1bde5-284">Sistemi yeniden başlatın veya yürütme **net stop olan /y** ardından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="1bde5-284">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="1bde5-285">Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.</span><span class="sxs-lookup"><span data-stu-id="1bde5-285">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="1bde5-286">Windows barındırma Paket Yükleyici IIS yüklemesini tamamlamak için bir sıfırlama gerektiren algılarsa, yükleyici IIS sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="1bde5-286">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="1bde5-287">Yükleyici IIS sıfırlama tetiklenirse, tüm Web siteleri ve IIS uygulama havuzları yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-287">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="1bde5-288">IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="1bde5-288">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="1bde5-289">Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin</span><span class="sxs-lookup"><span data-stu-id="1bde5-289">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="1bde5-290">Uygulamaları olan sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-290">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="1bde5-291">Web Dağıtımı'nı yüklemek için kullanın [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyicisini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="1bde5-291">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="1bde5-292">Tercih edilen yöntem, Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-292">The preferred method is to use WebPI.</span></span> <span data-ttu-id="1bde5-293">Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="1bde5-293">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="1bde5-294">IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="1bde5-294">Create the IIS site</span></span>

1. <span data-ttu-id="1bde5-295">Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-295">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="1bde5-296">Bir uygulamanın dağıtım Düzen açıklanan [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-296">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="1bde5-297">Yeni bir klasör içinde oluşturun bir *günlükleri* stdout günlüğe kaydetme etkinleştirilmişse ASP.NET Core modülü stdout günlükleri tutmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="1bde5-297">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="1bde5-298">Uygulama ile dağıtılırsa bir *günlükleri* yükü klasöründe bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-298">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="1bde5-299">Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak projeyi yerel olarak yapılandırıldığında klasörü görmek [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-299">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="1bde5-300">Yalnızca uygulama başlatma hataları gidermek için stdout günlük kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-300">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="1bde5-301">Hiçbir stdout günlük kaydı için rutin uygulama günlüğü kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-301">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="1bde5-302">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-302">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="1bde5-303">Uygulama havuzu günlüklerin yazılacağı konumuna yazma erişimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-303">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="1bde5-304">Tüm Günlük konumu yolu klasörlerde mevcut olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-304">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="1bde5-305">Stdout günlüğü hakkında daha fazla bilgi için bkz. [günlük oluşturma ve yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="1bde5-305">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="1bde5-306">ASP.NET Core uygulaması, günlüğe kaydetme hakkında daha fazla bilgi için bkz: [günlüğü](xref:fundamentals/logging/index) konu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-306">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="1bde5-307">İçinde **IIS Yöneticisi'ni**, sunucunun düğümünde açın **bağlantıları** paneli.</span><span class="sxs-lookup"><span data-stu-id="1bde5-307">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="1bde5-308">Sağ **siteleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="1bde5-308">Right-click the **Sites** folder.</span></span> <span data-ttu-id="1bde5-309">Seçin **Web sitesi Ekle** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="1bde5-309">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="1bde5-310">Sağlayan bir **Site adı** ayarlayıp **fiziksel yolu** uygulamanın dağıtım klasörüne.</span><span class="sxs-lookup"><span data-stu-id="1bde5-310">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="1bde5-311">Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="1bde5-311">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="1bde5-313">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-313">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="1bde5-314">Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1bde5-314">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="1bde5-315">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-315">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="1bde5-316">Joker karakterler yerine açık bir ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-316">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="1bde5-317">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="1bde5-317">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1bde5-318">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1bde5-318">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="1bde5-319">Sunucu düğümü altında seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-319">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="1bde5-320">Sitenin uygulama havuzu sağ tıklayıp **temel ayarları** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="1bde5-320">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="1bde5-321">İçinde **uygulama havuzunu Düzenle** penceresinde **.NET CLR sürümü** için **yönetilen kod yok**:</span><span class="sxs-lookup"><span data-stu-id="1bde5-321">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="1bde5-323">ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-323">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="1bde5-324">ASP.NET Core, Masaüstü CLR yüklemede de içermez.</span><span class="sxs-lookup"><span data-stu-id="1bde5-324">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="1bde5-325">Ayarı **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-325">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="1bde5-326">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-326">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="1bde5-327">Varsayılan uygulama havuzu kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimlik için doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve diğer gerekli kaynakları erişmek için gerekli izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="1bde5-327">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="1bde5-328">Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-328">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="1bde5-329">**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="1bde5-329">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="1bde5-330">Daha fazla bilgi için [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1bde5-330">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="1bde5-331">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="1bde5-331">Deploy the app</span></span>

<span data-ttu-id="1bde5-332">Uygulamayı barındıran sistemde oluşturduğunuz klasöre dağıtın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-332">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="1bde5-333">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-333">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="1bde5-334">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="1bde5-334">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="1bde5-335">Bkz: [uygulama dağıtımı ASP.NET Core için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu için Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-335">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="1bde5-336">Barındırma sağlayıcısı oluşturmak için bir yayımlama profili veya destek sağlar, bunların profilini indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="1bde5-336">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="1bde5-338">Web dağıtımı Visual Studio'nun dışında</span><span class="sxs-lookup"><span data-stu-id="1bde5-338">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="1bde5-339">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio'nun dışında komut satırından da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-339">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="1bde5-340">Daha fazla bilgi için [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="1bde5-340">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="1bde5-341">Alternatif Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="1bde5-341">Alternatives to Web Deploy</span></span>

<span data-ttu-id="1bde5-342">Uygulama barındırma sistemin el ile kopyalama, Xcopy, Robocopy ve PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-342">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="1bde5-343">IIS'ye ASP.NET Core dağıtımı hakkında daha fazla bilgi için bkz. [IIS Yöneticiler için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1bde5-343">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="1bde5-344">Web sitesine Gözat</span><span class="sxs-lookup"><span data-stu-id="1bde5-344">Browse the website</span></span>

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="1bde5-346">Kilitli dağıtım dosyaları</span><span class="sxs-lookup"><span data-stu-id="1bde5-346">Locked deployment files</span></span>

<span data-ttu-id="1bde5-347">Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı.</span><span class="sxs-lookup"><span data-stu-id="1bde5-347">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="1bde5-348">Dağıtım sırasında kilitli dosyalar üzerine yazılamaz.</span><span class="sxs-lookup"><span data-stu-id="1bde5-348">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="1bde5-349">Uygulama havuzunu kullanan bir dağıtımda kilitli dosyalar serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="1bde5-349">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="1bde5-350">Web dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="1bde5-350">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="1bde5-351">Bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-351">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="1bde5-352">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="1bde5-352">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="1bde5-353">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="1bde5-353">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="1bde5-354">El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-354">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="1bde5-355">Silmek için PowerShell kullanma *app_offline.html* (PowerShell 5 veya üzeri gerekir):</span><span class="sxs-lookup"><span data-stu-id="1bde5-355">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="1bde5-356">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="1bde5-356">Data protection</span></span>

<span data-ttu-id="1bde5-357">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="1bde5-357">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="1bde5-358">Veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma dağıtım betiği ile veya bir kalıcı oluşturmak için kullanıcı kodunda yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="1bde5-358">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="1bde5-359">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="1bde5-359">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="1bde5-360">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="1bde5-360">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="1bde5-361">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-361">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="1bde5-362">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-362">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="1bde5-363">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-363">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="1bde5-364">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="1bde5-364">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="1bde5-365">Veri koruma anahtarı halka kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="1bde5-365">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="1bde5-366">**Veri koruma kayıt defteri anahtarları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="1bde5-366">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="1bde5-367">ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-367">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="1bde5-368">Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-368">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="1bde5-369">Tek başına, webfarm olmayan IIS yüklemeleri [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) ile ASP.NET Core uygulaması kullanılan her bir uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-369">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="1bde5-370">Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-370">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="1bde5-371">Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-371">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="1bde5-372">Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-372">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="1bde5-373">Varsayılan olarak, veri koruma anahtarları şifreli değildir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-373">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="1bde5-374">Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-374">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="1bde5-375">X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-375">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="1bde5-376">Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-376">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="1bde5-377">Bkz: [ASP.NET Core veri koruma yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="1bde5-377">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="1bde5-378">**Kullanıcı profili yüklemek için IIS uygulama havuzu yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="1bde5-378">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="1bde5-379">Bu ayar **işlem modeli** bölümüne **Gelişmiş ayarlar** uygulama havuzu için.</span><span class="sxs-lookup"><span data-stu-id="1bde5-379">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="1bde5-380">Ayarlanan kullanıcı profilini Yükle `True`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-380">Set Load User Profile to `True`.</span></span> <span data-ttu-id="1bde5-381">Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI ile uygulama havuzu tarafından kullanılan kullanıcı hesabı için belirli bir anahtar kullanarak.</span><span class="sxs-lookup"><span data-stu-id="1bde5-381">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="1bde5-382">**Dosya sistemi anahtarı halkası deposu olarak kullanın**</span><span class="sxs-lookup"><span data-stu-id="1bde5-382">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="1bde5-383">Uygulama kodu ayarlamak [dosya sistemi bir anahtar halkası deposu olarak kullanırsınız](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="1bde5-383">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="1bde5-384">Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-384">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="1bde5-385">Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-385">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="1bde5-386">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="1bde5-386">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="1bde5-387">Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bde5-387">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="1bde5-388">X X509 dağıtma her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="1bde5-388">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="1bde5-389">Yapılandırma [kod veri koruması](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="1bde5-389">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="1bde5-390">**Veri koruma için makine genelinde bir ilke ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="1bde5-390">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="1bde5-391">Veri koruma sisteminde bir varsayılan ayarı desteği sınırlıdır [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'lerini kullanan tüm uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="1bde5-391">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="1bde5-392">Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="1bde5-392">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="1bde5-393">Sanal dizinler</span><span class="sxs-lookup"><span data-stu-id="1bde5-393">Virtual Directories</span></span>

<span data-ttu-id="1bde5-394">[IIS sanal dizinlerinin](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ile ASP.NET Core uygulamaları desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1bde5-394">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="1bde5-395">Bir uygulama olarak barındırılan bir [alt uygulama](#sub-applications).</span><span class="sxs-lookup"><span data-stu-id="1bde5-395">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="1bde5-396">Alt uygulamalar</span><span class="sxs-lookup"><span data-stu-id="1bde5-396">Sub-applications</span></span>

<span data-ttu-id="1bde5-397">ASP.NET Core uygulaması olarak barındırılan bir [IIS alt uygulama (uygulama içi sub)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span><span class="sxs-lookup"><span data-stu-id="1bde5-397">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="1bde5-398">Sub uygulamanın yolu, uygulama kök URL'SİNİN bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-398">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1bde5-399">Bir alt uygulama ASP.NET Core modülü bir işleyici içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-399">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="1bde5-400">Modül bir alt uygulamasının işleyici olarak eklenip eklenmediğini *web.config* dosyası bir *iç sunucu hatası 500.19* hatalı yapılandırma dosyasına başvuran alındığında alt uygulama göz atmak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="1bde5-400">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="1bde5-401">Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosyası için bir ASP.NET Core alt uygulama:</span><span class="sxs-lookup"><span data-stu-id="1bde5-401">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="1bde5-402">ASP.NET Core uygulaması altında ASP.NET Core sub-uygulama barındırma, devralınan işleyici sub-uygulamanın açıkça Kaldır *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1bde5-402">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

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

<span data-ttu-id="1bde5-403">Alt uygulama içindeki statik varlık bağlantılar tilde eğik çizgi kullanılmalıdır (`~/`) gösterimi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-403">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="1bde5-404">Tilde eğik çizgi gösterimi Tetikleyiciler bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro) işlenmiş göreli bağlantısını için alt-uygulamanın pathbase önüne eklediğinizden.</span><span class="sxs-lookup"><span data-stu-id="1bde5-404">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="1bde5-405">Alt uygulama için `/subapp_path`, bir görüntü ile bağlantılı `src="~/image.png"` olarak işlenen `src="/subapp_path/image.png"`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-405">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="1bde5-406">Kök uygulamanın statik dosya ara yazılımlarını statik dosya istek işlemiyor.</span><span class="sxs-lookup"><span data-stu-id="1bde5-406">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="1bde5-407">İstek, alt uygulamanın statik dosya ara yazılımı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-407">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="1bde5-408">Statik bir varlık, ın `src` özniteliği için mutlak bir yol ayarlayın (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase işlenir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-408">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="1bde5-409">Kök uygulamanın statik dosya ara yazılımlarını kök uygulamanın varlığından hizmet dener [webroot](xref:fundamentals/index#web-root-webroot), hangi sonuçlanıyor bir *404 - Bulunamadı* yanıt statik varlık kök uygulama kullanılabilir değilse.</span><span class="sxs-lookup"><span data-stu-id="1bde5-409">The root app's Static File Middleware attempts to serve the asset from the root app's [webroot](xref:fundamentals/index#web-root-webroot), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="1bde5-410">ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:</span><span class="sxs-lookup"><span data-stu-id="1bde5-410">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="1bde5-411">Alt uygulama için bir uygulama havuzu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-411">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="1bde5-412">Ayarlama **.NET CLR sürümü** için **yönetilen kod yok**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-412">Set the **.NET CLR Version** to **No Managed Code**.</span></span>

1. <span data-ttu-id="1bde5-413">IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-413">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="1bde5-414">IIS Yöneticisi'nde alt uygulama klasörüne sağ tıklayıp **uygulamasına dönüştürün**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-414">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="1bde5-415">İçinde **uygulama Ekle** iletişim kutusunda, kullanmak **seçin** için düğme **uygulama havuzu** alt uygulama için oluşturduğunuz uygulama havuzuna atanamıyor.</span><span class="sxs-lookup"><span data-stu-id="1bde5-415">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="1bde5-416">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-416">Select **OK**.</span></span>

<span data-ttu-id="1bde5-417">Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-417">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="1bde5-418">Barındırma modeli ve ASP.NET Core modülü yapılandırma işlem hakkında daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module> ve <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="1bde5-418">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="1bde5-419">Web.config ile IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1bde5-419">Configuration of IIS with web.config</span></span>

<span data-ttu-id="1bde5-420">IIS yapılandırması tarafından etkilenir `<system.webServer>` bölümünü *web.config* işlevsel ASP.NET Core modülü ile ASP.NET Core uygulamaları için IIS senaryolar için.</span><span class="sxs-lookup"><span data-stu-id="1bde5-420">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="1bde5-421">Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-421">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="1bde5-422">IIS dinamik sıkıştırması kullanmak için sunucu düzeyinde yapılandırılmışsa `<urlCompression>` uygulamanın öğesinde *web.config* dosya devre dışı bırakabilir, ASP.NET Core uygulaması için.</span><span class="sxs-lookup"><span data-stu-id="1bde5-422">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="1bde5-423">Daha fazla bilgi için [yapılandırma başvurusu için \<system.webServer >](/iis/configuration/system.webServer/), [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module), ve [ASP.NET içeren IIS modülleri Çekirdek](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="1bde5-423">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="1bde5-424">(IIS 10.0 veya üzeri desteklenir) yalıtılmış uygulama havuzlarında çalışmakta olan tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgeleri.</span><span class="sxs-lookup"><span data-stu-id="1bde5-424">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="1bde5-425">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="1bde5-425">Configuration sections of web.config</span></span>

<span data-ttu-id="1bde5-426">ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma için ASP.NET Core uygulamaları tarafından kullanılmaz:</span><span class="sxs-lookup"><span data-stu-id="1bde5-426">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="1bde5-427">ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-427">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="1bde5-428">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1bde5-428">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="1bde5-429">Uygulama havuzları</span><span class="sxs-lookup"><span data-stu-id="1bde5-429">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1bde5-430">Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-430">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="1bde5-431">İşlemdeki barındırma &ndash; uygulamalarını ayrı uygulama havuzlarında çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-431">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="1bde5-432">Barındırma işlemi çıkış &ndash; her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="1bde5-432">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="1bde5-433">IIS **Web sitesi Ekle** iletişim varsayılan olarak bir uygulama başına tek bir uygulama havuzu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-433">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="1bde5-434">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-434">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="1bde5-435">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-435">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1bde5-436">Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="1bde5-436">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="1bde5-437">IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="1bde5-437">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="1bde5-438">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-438">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="1bde5-439">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-439">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="1bde5-440">Uygulama havuzu kimliği</span><span class="sxs-lookup"><span data-stu-id="1bde5-440">Application Pool Identity</span></span>

<span data-ttu-id="1bde5-441">Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="1bde5-441">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="1bde5-442">IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-442">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="1bde5-443">IIS Yönetim konsolunda altında **Gelişmiş ayarlar** emin olmak için uygulama havuzu, **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="1bde5-443">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="1bde5-445">IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bde5-445">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="1bde5-446">Bu kimlik kullanarak kaynakları güvenliği sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-446">Resources can be secured using this identity.</span></span> <span data-ttu-id="1bde5-447">Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.</span><span class="sxs-lookup"><span data-stu-id="1bde5-447">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="1bde5-448">IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1bde5-448">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="1bde5-449">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-449">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="1bde5-450">Sağ tıklatın ve dizin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-450">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="1bde5-451">Altında **güvenlik** sekmesinde **Düzenle** düğmesine ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-451">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="1bde5-452">Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="1bde5-452">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="1bde5-453">ENTER **IIS uygulama havuzu\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alan.</span><span class="sxs-lookup"><span data-stu-id="1bde5-453">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="1bde5-454">Seçin **Adları Denetle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-454">Select the **Check Names** button.</span></span> <span data-ttu-id="1bde5-455">İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-455">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="1bde5-456">Zaman **Adları Denetle** düğmesi seçili değerini **DefaultAppPool** nesne adları alanında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-456">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="1bde5-457">Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-457">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="1bde5-458">Kullanım **IIS uygulama havuzu\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.</span><span class="sxs-lookup"><span data-stu-id="1bde5-458">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="1bde5-460">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1bde5-460">Select **OK**.</span></span>

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "Adları denetle"'i seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="1bde5-462">Okuma &amp; Yürütme izinleri varsayılan verilmesi.</span><span class="sxs-lookup"><span data-stu-id="1bde5-462">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="1bde5-463">Gerektiğinde ek izinler sağlar.</span><span class="sxs-lookup"><span data-stu-id="1bde5-463">Provide additional permissions as needed.</span></span>

<span data-ttu-id="1bde5-464">Erişim de verilebilir bir komut istemi kullanarak **ICACLS** aracı.</span><span class="sxs-lookup"><span data-stu-id="1bde5-464">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="1bde5-465">Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:</span><span class="sxs-lookup"><span data-stu-id="1bde5-465">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="1bde5-466">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls) konu.</span><span class="sxs-lookup"><span data-stu-id="1bde5-466">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="1bde5-467">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="1bde5-467">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1bde5-468">[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki IIS dağıtım senaryolarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-468">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="1bde5-469">İşlem içi</span><span class="sxs-lookup"><span data-stu-id="1bde5-469">In-process</span></span>
  * <span data-ttu-id="1bde5-470">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1bde5-470">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="1bde5-471">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="1bde5-471">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="1bde5-472">İşlem dışı</span><span class="sxs-lookup"><span data-stu-id="1bde5-472">Out-of-process</span></span>
  * <span data-ttu-id="1bde5-473">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1bde5-473">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="1bde5-474">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-474">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="1bde5-475">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="1bde5-475">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1bde5-476">Bir HTTP/2 bağlantı kurulduğunda, işlem içi dağıtımı için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-476">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="1bde5-477">Bir HTTP/2 bağlantı kurulduğunda, bir işlem dışı dağıtım için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-477">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="1bde5-478">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module> konu ve <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="1bde5-478">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1bde5-479">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki temel gereksinimlere işlem dışı dağıtımları için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="1bde5-479">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="1bde5-480">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1bde5-480">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="1bde5-481">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="1bde5-481">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="1bde5-482">Hedef çerçeve: uygulanamaz işlem dışı dağıtımlar için bu yana HTTP/2 bağlantı tamamen IIS tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-482">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="1bde5-483">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="1bde5-483">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1bde5-484">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="1bde5-484">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="1bde5-485">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1bde5-485">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="1bde5-486">Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner.</span><span class="sxs-lookup"><span data-stu-id="1bde5-486">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="1bde5-487">IIS dağıtımları olan HTTP/2 yapılandırma hakkında daha fazla bilgi için bkz. [HTTP/2 IIS'de](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="1bde5-487">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="1bde5-488">IIS Yöneticiler için dağıtım kaynakları</span><span class="sxs-lookup"><span data-stu-id="1bde5-488">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="1bde5-489">IIS IIS belgelerinde kapsamlı hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-489">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="1bde5-490">IIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="1bde5-490">IIS documentation</span></span>](/iis)

<span data-ttu-id="1bde5-491">.NET Core uygulaması dağıtım modelleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-491">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="1bde5-492">.NET core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="1bde5-492">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="1bde5-493">ASP.NET Core modülü Kestrel web sunucusu, bir ters proxy sunucusu olarak IIS veya IIS Express kullanacak şekilde nasıl olanak tanıdığını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-493">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="1bde5-494">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="1bde5-494">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="1bde5-495">ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-495">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="1bde5-496">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="1bde5-496">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="1bde5-497">Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-497">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="1bde5-498">Dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="1bde5-498">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="1bde5-499">ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri keşfedin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-499">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="1bde5-500">IIS modülleri</span><span class="sxs-lookup"><span data-stu-id="1bde5-500">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="1bde5-501">ASP.NET Core uygulamaları IIS dağıtımları sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="1bde5-501">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="1bde5-502">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="1bde5-502">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="1bde5-503">Sık karşılaşılan barındırırken IIS üzerinde ASP.NET Core uygulamaları ayırmak.</span><span class="sxs-lookup"><span data-stu-id="1bde5-503">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="1bde5-504">Azure App Service ve IIS için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="1bde5-504">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="1bde5-505">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1bde5-505">Additional resources</span></span>

* <xref:test/troubleshoot>
* [<span data-ttu-id="1bde5-506">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="1bde5-506">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="1bde5-507">Resmi Microsoft IIS sitesi</span><span class="sxs-lookup"><span data-stu-id="1bde5-507">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="1bde5-508">Windows Server Teknik İçerik Kitaplığı</span><span class="sxs-lookup"><span data-stu-id="1bde5-508">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="1bde5-509">IIS HTTP/2</span><span class="sxs-lookup"><span data-stu-id="1bde5-509">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
