---
title: ASP.NET Core Web barındırma
author: guardrex
description: Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET Core web ana bilgisayar hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ced2a766359894b9b83164c12a3ab69aa13c93a0
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="4ab37-103">ASP.NET Core Web barındırma</span><span class="sxs-lookup"><span data-stu-id="4ab37-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="4ab37-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4ab37-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4ab37-105">ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="4ab37-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="4ab37-106">Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="4ab37-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4ab37-107">En az bir sunucu ve bir istek işleme ardışık düzenine konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="4ab37-108">Bu konu, ASP.NET çekirdek Web Host içerir ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), web uygulamaları barındırmak için yararlı olan.</span><span class="sxs-lookup"><span data-stu-id="4ab37-108">This topic covers the ASP.NET Core Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="4ab37-109">Kapsamı .NET genel ana bilgisayar için ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), bkz: [genel ana bilgisayar](xref:fundamentals/host/generic-host) konu.</span><span class="sxs-lookup"><span data-stu-id="4ab37-109">For coverage of the .NET Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="4ab37-110">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="4ab37-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="4ab37-112">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4ab37-112">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4ab37-113">Bu genellikle uygulamanın giriş noktası gerçekleştirilen `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4ab37-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="4ab37-114">Proje şablonları içinde `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="4ab37-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="4ab37-115">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="4ab37-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4ab37-116">`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="4ab37-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="4ab37-117">Yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak.</span><span class="sxs-lookup"><span data-stu-id="4ab37-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server.</span></span> <span data-ttu-id="4ab37-118">Kestrel varsayılan seçenekleri için bkz [Kestrel seçenekleri Kestrel web server ASP.NET Core uygulamasında bölümünü](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="4ab37-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="4ab37-119">İçerik kök tarafından döndürülen yola ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="4ab37-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="4ab37-120">Gelen isteğe bağlı yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="4ab37-120">Loads optional configuration from:</span></span>
  * <span data-ttu-id="4ab37-121">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="4ab37-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="4ab37-122">*appSettings. {Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="4ab37-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="4ab37-123">[Kullanıcı parolaları](xref:security/app-secrets) uygulama çalıştırıldığında `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="4ab37-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="4ab37-124">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="4ab37-124">Environment variables.</span></span>
  * <span data-ttu-id="4ab37-125">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="4ab37-125">Command-line arguments.</span></span>
* <span data-ttu-id="4ab37-126">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktısı için.</span><span class="sxs-lookup"><span data-stu-id="4ab37-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="4ab37-127">Günlük kaydı içerir [günlüğü filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="4ab37-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="4ab37-128">IIS çalıştırırken etkinleştirir [IIS tümleştirme](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="4ab37-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="4ab37-129">Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4ab37-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="4ab37-130">Modül IIS ve Kestrel arasında ters bir proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4ab37-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="4ab37-131">Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="4ab37-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="4ab37-132">IIS, varsayılan seçenekleri için bkz: [IIS seçenekleri konak ASP.NET Core IIS ile Windows bölümünü](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="4ab37-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="4ab37-133">Ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="4ab37-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="4ab37-134">Daha fazla bilgi için bkz: [kapsam doğrulama](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="4ab37-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="4ab37-135">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4ab37-136">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="4ab37-137">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4ab37-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4ab37-138">Uygulama yapılandırması hakkında daha fazla bilgi için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4ab37-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="4ab37-139">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` yöntemi, bir ana bilgisayardan oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core ile desteklenen bir yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="4ab37-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="4ab37-140">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="4ab37-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="4ab37-142">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4ab37-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4ab37-143">Bir ana bilgisayar oluşturma uygulamanın giriş noktası genellikle gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4ab37-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="4ab37-144">Proje şablonları içinde `Main` bulunan *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4ab37-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="4ab37-145">`WebHostBuilder` gerektiren bir [IServer uygulayan sunucu](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="4ab37-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="4ab37-146">Yerleşik sunucularıdır [Kestrel](xref:fundamentals/servers/kestrel) ve [HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET Core 2.0 sürümünden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="4ab37-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="4ab37-147">Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="4ab37-148">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4ab37-149">İçin varsayılan içerik kök elde `UseContentRoot` tarafından [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="4ab37-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="4ab37-150">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="4ab37-151">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4ab37-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4ab37-152">IIS ters proxy kullanmak için arama [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="4ab37-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="4ab37-153">`UseIISIntegration` yapılandırmaz bir *server*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="4ab37-154">`UseIISIntegration` Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters Ara sunucu oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="4ab37-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="4ab37-155">IIS ASP.NET Core ile kullanmak için `UseKestrel` ve `UseIISIntegration` belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="4ab37-156">`UseIISIntegration` yalnızca IIS veya IIS Express çalıştırırken etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="4ab37-157">Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4ab37-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="4ab37-158">Bir ana bilgisayar (ve ASP.NET Core uygulama) yapılandırır en az bir uygulama sunucusu ve yapılandırma uygulamanın istek ardışık belirtme içerir:</span><span class="sxs-lookup"><span data-stu-id="4ab37-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="4ab37-159">Bir konak ayarlıyor zaman [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="4ab37-160">Varsa bir `Startup` sınıfı belirtilmediyse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4ab37-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="4ab37-161">Daha fazla bilgi için bkz: [ASP.NET Core uygulama başlangıç](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4ab37-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="4ab37-162">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4ab37-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="4ab37-163">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarlarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4ab37-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="4ab37-164">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="4ab37-164">Host configuration values</span></span>

<span data-ttu-id="4ab37-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="4ab37-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="4ab37-166">Ortam değişkenleri biçiminde içeren konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="4ab37-167">Örneğin, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="4ab37-168">Açık yöntemleri gibi [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span><span class="sxs-lookup"><span data-stu-id="4ab37-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="4ab37-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="4ab37-170">Bir değerle ayarlarken `UseSetting`, değer, dize türünden bağımsız olarak olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="4ab37-171">Ana bilgisayar son hangi seçeneği bir değer ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="4ab37-172">Daha fazla bilgi için bkz: [geçersiz kılma yapılandırmasını](#override-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="4ab37-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="4ab37-173">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="4ab37-173">Capture Startup Errors</span></span>

<span data-ttu-id="4ab37-174">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="4ab37-175">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="4ab37-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="4ab37-176">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="4ab37-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4ab37-177">**Varsayılan**: varsayılan olarak `false` Kestrel IIS, varsayılan olduğu arkasında ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="4ab37-178">**Kullanılarak ayarlanan**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="4ab37-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="4ab37-179">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="4ab37-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="4ab37-180">Zaman `false`, çıkma konak başlangıç sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="4ab37-181">Zaman `true`, ana bilgisayar başlatma sırasında özel durumları yakalar ve sunucu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="4ab37-184">Kök içerik</span><span class="sxs-lookup"><span data-stu-id="4ab37-184">Content Root</span></span>

<span data-ttu-id="4ab37-185">Bu ayar, ASP.NET Core MVC görünümler gibi içerik dosyaları aranıyor başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="4ab37-186">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="4ab37-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="4ab37-187">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="4ab37-187">**Type**: *string*</span></span>  
<span data-ttu-id="4ab37-188">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.</span><span class="sxs-lookup"><span data-stu-id="4ab37-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="4ab37-189">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="4ab37-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="4ab37-190">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="4ab37-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="4ab37-191">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="4ab37-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="4ab37-192">Yol yoksa, konağı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4ab37-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-193">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="4ab37-195">Ayrıntılı hataları</span><span class="sxs-lookup"><span data-stu-id="4ab37-195">Detailed Errors</span></span>

<span data-ttu-id="4ab37-196">Ayrıntılı hataları yakalanan belirler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="4ab37-197">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="4ab37-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="4ab37-198">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="4ab37-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4ab37-199">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="4ab37-199">**Default**: false</span></span>  
<span data-ttu-id="4ab37-200">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4ab37-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4ab37-201">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="4ab37-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="4ab37-202">Etkin olduğunda (veya ne zaman <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="4ab37-205">Ortam</span><span class="sxs-lookup"><span data-stu-id="4ab37-205">Environment</span></span>

<span data-ttu-id="4ab37-206">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-206">Sets the app's environment.</span></span>

<span data-ttu-id="4ab37-207">**Anahtar**: ortamı</span><span class="sxs-lookup"><span data-stu-id="4ab37-207">**Key**: environment</span></span>  
<span data-ttu-id="4ab37-208">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="4ab37-208">**Type**: *string*</span></span>  
<span data-ttu-id="4ab37-209">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="4ab37-209">**Default**: Production</span></span>  
<span data-ttu-id="4ab37-210">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="4ab37-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="4ab37-211">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="4ab37-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="4ab37-212">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-212">The environment can be set to any value.</span></span> <span data-ttu-id="4ab37-213">Framework tanımlı değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="4ab37-214">Değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-214">Values aren't case sensitive.</span></span> <span data-ttu-id="4ab37-215">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4ab37-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="4ab37-216">Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri kümesinde *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="4ab37-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="4ab37-217">Daha fazla bilgi için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="4ab37-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="4ab37-220">Barındırma başlangıç derlemeler</span><span class="sxs-lookup"><span data-stu-id="4ab37-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="4ab37-221">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="4ab37-222">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="4ab37-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="4ab37-223">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="4ab37-223">**Type**: *string*</span></span>  
<span data-ttu-id="4ab37-224">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="4ab37-224">**Default**: Empty string</span></span>  
<span data-ttu-id="4ab37-225">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4ab37-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4ab37-226">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="4ab37-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="4ab37-227">Başlangıçta yüklemek için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış dize.</span><span class="sxs-lookup"><span data-stu-id="4ab37-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="4ab37-228">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="4ab37-229">Yapılandırma değeri için boş bir dize Varsayılanları rağmen barındırma başlangıç derlemeler her zaman uygulamanın derleme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4ab37-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="4ab37-230">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlatma sırasında ortak hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-231">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4ab37-233">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="4ab37-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="4ab37-234">URL'leri barındırma tercih</span><span class="sxs-lookup"><span data-stu-id="4ab37-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="4ab37-235">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4ab37-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="4ab37-236">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="4ab37-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="4ab37-237">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="4ab37-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4ab37-238">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="4ab37-238">**Default**: true</span></span>  
<span data-ttu-id="4ab37-239">**Kullanılarak ayarlanan**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="4ab37-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="4ab37-240">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="4ab37-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="4ab37-241">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-242">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4ab37-244">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="4ab37-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="4ab37-245">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="4ab37-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="4ab37-246">Uygulamanın derlemesi tarafından yapılandırılan başlangıç derlemeleri barındırma dahil olmak üzere başlangıç derlemeleri barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="4ab37-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="4ab37-247">Bkz: [IHostingStartup bir dış derleme uygulama geliştirmek](xref:fundamentals/configuration/platform-specific-configuration) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4ab37-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="4ab37-248">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="4ab37-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="4ab37-249">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="4ab37-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4ab37-250">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="4ab37-250">**Default**: false</span></span>  
<span data-ttu-id="4ab37-251">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4ab37-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4ab37-252">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="4ab37-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="4ab37-253">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4ab37-256">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="4ab37-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="4ab37-257">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="4ab37-257">Server URLs</span></span>

<span data-ttu-id="4ab37-258">IP adresi veya ana bilgisayar adresleriyle bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="4ab37-259">**Anahtar**: URL'leri</span><span class="sxs-lookup"><span data-stu-id="4ab37-259">**Key**: urls</span></span>  
<span data-ttu-id="4ab37-260">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="4ab37-260">**Type**: *string*</span></span>  
<span data-ttu-id="4ab37-261">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="4ab37-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="4ab37-262">**Kullanılarak ayarlanan**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="4ab37-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="4ab37-263">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="4ab37-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="4ab37-264">Bir noktalı virgülle ayrılmış ayarlayın (;) URL listesi önekleri sunucu yanıt.</span><span class="sxs-lookup"><span data-stu-id="4ab37-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="4ab37-265">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="4ab37-266">Kullan "\*" sunucu IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokolü kullanarak isteklerini dinleme yapması gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="4ab37-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="4ab37-267">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="4ab37-268">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-269">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="4ab37-270">Kestrel kendi uç nokta yapılandırması API vardır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="4ab37-271">Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="4ab37-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="4ab37-273">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="4ab37-273">Shutdown Timeout</span></span>

<span data-ttu-id="4ab37-274">Web ana bilgisayarı kapatmak beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="4ab37-275">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="4ab37-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="4ab37-276">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="4ab37-276">**Type**: *int*</span></span>  
<span data-ttu-id="4ab37-277">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="4ab37-277">**Default**: 5</span></span>  
<span data-ttu-id="4ab37-278">**Kullanılarak ayarlanan**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="4ab37-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="4ab37-279">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="4ab37-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="4ab37-280">Anahtar kabul rağmen bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi geçen bir [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="4ab37-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="4ab37-281">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="4ab37-282">Sırasında zaman aşımı süresi, barındırma:</span><span class="sxs-lookup"><span data-stu-id="4ab37-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="4ab37-283">Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="4ab37-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="4ab37-284">Barındırılan hizmetler, durdurmak için başarısız hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="4ab37-285">Tüm barındırılan Hizmetleri Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında kalan tüm etkin Hizmetleri durduruldu.</span><span class="sxs-lookup"><span data-stu-id="4ab37-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="4ab37-286">İşleme tamamlamadınız olsa bile hizmetlerini durdurun.</span><span class="sxs-lookup"><span data-stu-id="4ab37-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="4ab37-287">Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımı süresini artırın.</span><span class="sxs-lookup"><span data-stu-id="4ab37-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-288">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4ab37-290">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="4ab37-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="4ab37-291">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="4ab37-291">Startup Assembly</span></span>

<span data-ttu-id="4ab37-292">Aranacak derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4ab37-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="4ab37-293">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="4ab37-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="4ab37-294">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="4ab37-294">**Type**: *string*</span></span>  
<span data-ttu-id="4ab37-295">**Varsayılan**: uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="4ab37-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="4ab37-296">**Kullanılarak ayarlanan**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="4ab37-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="4ab37-297">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="4ab37-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="4ab37-298">Ada göre derleme (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="4ab37-299">Birden çok `UseStartup` yöntemleri çağrılmadan, son önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="4ab37-302">Kök web</span><span class="sxs-lookup"><span data-stu-id="4ab37-302">Web Root</span></span>

<span data-ttu-id="4ab37-303">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="4ab37-304">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="4ab37-304">**Key**: webroot</span></span>  
<span data-ttu-id="4ab37-305">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="4ab37-305">**Type**: *string*</span></span>  
<span data-ttu-id="4ab37-306">**Varsayılan**: belirtilmezse, varsayılan "(Content Root)/wwwroot olur", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="4ab37-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="4ab37-307">Yol yoksa, yok dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="4ab37-308">**Kullanılarak ayarlanan**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="4ab37-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="4ab37-309">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="4ab37-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="4ab37-312">Yapılandırma geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="4ab37-312">Override configuration</span></span>

<span data-ttu-id="4ab37-313">Kullanım [yapılandırma](xref:fundamentals/configuration/index) ana bilgisayarı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="4ab37-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="4ab37-314">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hosting.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="4ab37-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="4ab37-315">Herhangi bir yapılandırma aşağıdaki konumdan yüklendi *hosting.json* dosyası tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="4ab37-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="4ab37-316">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-317">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4ab37-318">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="4ab37-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4ab37-319">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="4ab37-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4ab37-321">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="4ab37-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4ab37-322">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="4ab37-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="4ab37-323">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="4ab37-324">`GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="4ab37-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="4ab37-325">`UseConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="4ab37-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="4ab37-326">Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="4ab37-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="4ab37-327">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="4ab37-328">Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="4ab37-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="4ab37-329">Ana bilgisayar üzerinde belirli bir URL çalıştırmak belirtmek için istenen değeri bir komut isteminden yürütülürken geçirilebilir [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="4ab37-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="4ab37-330">Komut satırı bağımsız değişkeni geçersiz kılmaları `urls` değeri *hosting.json* dosya ve sunucunun dinlediği bağlantı noktası 8080 üzerinde:</span><span class="sxs-lookup"><span data-stu-id="4ab37-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="4ab37-331">Ana bilgisayar yönetmek</span><span class="sxs-lookup"><span data-stu-id="4ab37-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4ab37-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4ab37-333">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="4ab37-333">**Run**</span></span>

<span data-ttu-id="4ab37-334">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="4ab37-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4ab37-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="4ab37-335">**Start**</span></span>

<span data-ttu-id="4ab37-336">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4ab37-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4ab37-337">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="4ab37-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="4ab37-338">Uygulamayı başlatın ve önceden yapılandırılmış Varsayılanları birini kullanarak yeni bir ana bilgisayar Başlat `CreateDefaultBuilder` statik kolaylık yöntemini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="4ab37-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="4ab37-339">Bu yöntemler konsol çıktısı olmadan ve sunucuyu Başlat [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için bir sonu bekleyin:</span><span class="sxs-lookup"><span data-stu-id="4ab37-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="4ab37-340">**Başlangıç (RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="4ab37-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="4ab37-341">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4ab37-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4ab37-342">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="4ab37-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4ab37-343">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="4ab37-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4ab37-344">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4ab37-345">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="4ab37-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="4ab37-346">Bir URL ile başlatın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4ab37-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4ab37-347">Aynı sonucu verir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="4ab37-348">**Başlangıç (Eylem&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4ab37-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="4ab37-349">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="4ab37-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4ab37-350">Aşağıdaki tarayıcı isteklerini örnekle kullanın:</span><span class="sxs-lookup"><span data-stu-id="4ab37-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="4ab37-351">İstek</span><span class="sxs-lookup"><span data-stu-id="4ab37-351">Request</span></span>                                    | <span data-ttu-id="4ab37-352">Yanıt</span><span class="sxs-lookup"><span data-stu-id="4ab37-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="4ab37-353">Merhaba, Martin!</span><span class="sxs-lookup"><span data-stu-id="4ab37-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="4ab37-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="4ab37-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="4ab37-355">"Ooops!" dizesini içeren bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="4ab37-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="4ab37-356">Aykırı dizesiyle "Uh!!"</span><span class="sxs-lookup"><span data-stu-id="4ab37-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="4ab37-357">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="4ab37-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="4ab37-358">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="4ab37-358">Hello World!</span></span>                             |

<span data-ttu-id="4ab37-359">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="4ab37-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4ab37-360">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4ab37-361">**Başlangıç (url, eylem dize&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4ab37-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="4ab37-362">Bir örneği ile bir URL kullanın `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4ab37-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4ab37-363">Aynı sonucu verir **Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**, uygulama dışında yanıt adresindeki `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="4ab37-364">**StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="4ab37-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="4ab37-365">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4ab37-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4ab37-366">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="4ab37-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4ab37-367">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="4ab37-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4ab37-368">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="4ab37-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4ab37-369">**StartWith (url, eylem dize&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="4ab37-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="4ab37-370">Bir URL ve yapılandırmak için bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4ab37-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4ab37-371">Aynı sonucu verir **StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4ab37-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4ab37-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4ab37-373">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="4ab37-373">**Run**</span></span>

<span data-ttu-id="4ab37-374">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="4ab37-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4ab37-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="4ab37-375">**Start**</span></span>

<span data-ttu-id="4ab37-376">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4ab37-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4ab37-377">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="4ab37-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="4ab37-378">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="4ab37-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="4ab37-379">[IHostingEnvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="4ab37-380">Kullanmak [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="4ab37-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="4ab37-381">A [kurala dayalı yaklaşım](xref:fundamentals/environments#startup-conventions) ortamına bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="4ab37-382">Alternatif olarak, Ekle `IHostingEnvironment` içine `Startup` kullanılmak üzere Oluşturucusu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4ab37-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="4ab37-383">Ek olarak `IsDevelopment` genişletme yöntemi, `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="4ab37-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="4ab37-384">Bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="4ab37-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="4ab37-385">`IHostingEnvironment` Hizmet aynı zamanda eklenen doğrudan `Configure` yöntemi işleme ardışık ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="4ab37-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="4ab37-386">`IHostingEnvironment` içine eklenen `Invoke` özel oluştururken yöntemi [Ara](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="4ab37-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="4ab37-387">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="4ab37-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="4ab37-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ab37-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="4ab37-389">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="4ab37-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="4ab37-390">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="4ab37-390">Cancellation Token</span></span>    | <span data-ttu-id="4ab37-391">Ne zaman tetiklendi&#8230;</span><span class="sxs-lookup"><span data-stu-id="4ab37-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="4ab37-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="4ab37-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="4ab37-393">Ana bilgisayar tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="4ab37-393">The host has fully started.</span></span> |
| [<span data-ttu-id="4ab37-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="4ab37-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="4ab37-395">Konak bir kapama üzeredir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="4ab37-396">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="4ab37-396">All requests should be processed.</span></span> <span data-ttu-id="4ab37-397">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="4ab37-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="4ab37-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="4ab37-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="4ab37-399">Konak bir kapama gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="4ab37-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="4ab37-400">İstekleri hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-400">Requests may still be processing.</span></span> <span data-ttu-id="4ab37-401">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="4ab37-401">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="4ab37-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) uygulama sonlandırılması ister.</span><span class="sxs-lookup"><span data-stu-id="4ab37-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="4ab37-403">Aşağıdaki sınıf kullanır `StopApplication` düzgün biçimde bir uygulamasını kapatmak için zaman sınıfının `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="4ab37-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="4ab37-404">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="4ab37-404">Scope validation</span></span>

<span data-ttu-id="4ab37-405">ASP.NET Core 2.0 veya sonraki sürümlerde, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="4ab37-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="4ab37-406">Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı doğrulamak üzere denetler:</span><span class="sxs-lookup"><span data-stu-id="4ab37-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="4ab37-407">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök servis sağlayıcısı'ndan çözülmüş değil.</span><span class="sxs-lookup"><span data-stu-id="4ab37-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="4ab37-408">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değil.</span><span class="sxs-lookup"><span data-stu-id="4ab37-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="4ab37-409">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="4ab37-410">Kök hizmet sağlayıcısının ömrü zaman sağlayıcı uygulamayla başlatır ve uygulamayı kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4ab37-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="4ab37-411">Kapsamlı Hizmetleri oluşturuldukları kapsayıcı tarafından elden.</span><span class="sxs-lookup"><span data-stu-id="4ab37-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="4ab37-412">Kapsamlı bir hizmet kök kapsayıcısında oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından atıldı çünkü hizmetin ömrü tekliye etkili bir şekilde yükseltildi.</span><span class="sxs-lookup"><span data-stu-id="4ab37-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="4ab37-413">Hizmet kapsamları doğrulama yakalar bu durumlarda, `BuildServiceProvider` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4ab37-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="4ab37-414">Her zaman üretim ortamında da dahil olmak üzere kapsamları doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="4ab37-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="4ab37-415">Sorun giderme System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="4ab37-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="4ab37-416">**ASP.NET çekirdeği 2.0 yalnızca uygular**</span><span class="sxs-lookup"><span data-stu-id="4ab37-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="4ab37-417">Bir konak ekleyerek yerleşik `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`:</span><span class="sxs-lookup"><span data-stu-id="4ab37-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="4ab37-418">Konak bu şekilde oluşturulduysa, aşağıdaki hata oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="4ab37-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="4ab37-419">Bu kaynaklanır [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (geçerli derleme) taramak için gerekli `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="4ab37-420">Uygulamayı el ile yerleştirir, `IStartup` bağımlılık ekleme kapsayıcısına aşağıdaki çağrısı ekleyin `WebHostBuilder` ile belirtilen derleme adı:</span><span class="sxs-lookup"><span data-stu-id="4ab37-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="4ab37-421">Alternatif olarak, bir kukla eklemek `Configure` için `WebHostBuilder`, hangi kümeleri `applicationName`(`ApplicationKey`) otomatik olarak:</span><span class="sxs-lookup"><span data-stu-id="4ab37-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="4ab37-422">**Not**: uygulama çağırdığınızda değil yalnızca gerekli ASP.NET Core 2.0 sürümüyle ve yalnızca budur `UseStartup` veya `Configure`.</span><span class="sxs-lookup"><span data-stu-id="4ab37-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="4ab37-423">Daha fazla bilgi için bkz: [duyuruları: Microsoft.Extensions.PlatformAbstractions süredir (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="4ab37-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ab37-424">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4ab37-424">Additional resources</span></span>

* [<span data-ttu-id="4ab37-425">IIS ile Windows’da barındırma</span><span class="sxs-lookup"><span data-stu-id="4ab37-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="4ab37-426">Nginx ile Linux’ta barındırma</span><span class="sxs-lookup"><span data-stu-id="4ab37-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="4ab37-427">Apache ile Linux’ta barındırma</span><span class="sxs-lookup"><span data-stu-id="4ab37-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="4ab37-428">Bir Windows hizmeti ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="4ab37-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
