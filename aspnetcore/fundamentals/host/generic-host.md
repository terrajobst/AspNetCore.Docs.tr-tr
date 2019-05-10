---
title: .NET genel ana bilgisayar
author: guardrex
description: ASP.NET Core'nın genel ana bilgisayar hakkında uygulama başlatma ve ömür yönetimi için sorumlu olduğu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/25/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: d823e2189d21e0566656b7eb8c9164d02e43d0ea
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901643"
---
# <a name="net-generic-host"></a><span data-ttu-id="ab5de-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="ab5de-103">.NET Generic Host</span></span>

<span data-ttu-id="ab5de-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ab5de-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ab5de-105">ASP.NET Core uygulamaları yapılandırın ve bir konak başlatın.</span><span class="sxs-lookup"><span data-stu-id="ab5de-105">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="ab5de-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ab5de-106">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="ab5de-107">Bu makalede, .NET Core genel ana bilgisayar yer almaktadır (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span><span class="sxs-lookup"><span data-stu-id="ab5de-107">This article covers the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span></span>

<span data-ttu-id="ab5de-108">Genel konak Web Konaktan farklıdır HTTP ardışık düzen tarafından ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API ayırır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-108">Generic Host differs from Web Host in that it decouples the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="ab5de-109">Mesajlaşma, arka plan görevleri ve diğer HTTP olmayan iş yükleri, genel ana bilgisayar kullanın ve yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri yararlanın.</span><span class="sxs-lookup"><span data-stu-id="ab5de-109">Messaging, background tasks, and other non-HTTP workloads can use Generic Host and benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="ab5de-110">ASP.NET Core 3. 0'dan başlayarak, genel ana bilgisayar hem HTTP hem de HTTP olmayan iş yükleri için önerilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-110">Starting in ASP.NET Core 3.0, Generic Host is recommended for both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="ab5de-111">Bir HTTP sunucusu uygulamasını eklenirse, uygulaması çalışan <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-111">An HTTP server implementation, if included, runs as an implementation of <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="ab5de-112"><xref:Microsoft.Extensions.Hosting.IHostedService> diğer iş yükleri için de kullanılabilir bir arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-112"><xref:Microsoft.Extensions.Hosting.IHostedService> is an interface that can be used for other workloads as well.</span></span>

<span data-ttu-id="ab5de-113">Web ana bilgisayarı artık web apps için önerilir, ancak geriye dönük uyumluluk için kullanılabilir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-113">Web Host is no longer recommended for web apps but remains available for backward compatibility.</span></span>

> [!NOTE]
> <span data-ttu-id="ab5de-114">Bu makalenin kalan kısmı için 3.0 güncelleştirilmedi.</span><span class="sxs-lookup"><span data-stu-id="ab5de-114">This remainder of this article hasn't been updated for 3.0.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ab5de-115">ASP.NET Core uygulamaları yapılandırın ve bir konak başlatın.</span><span class="sxs-lookup"><span data-stu-id="ab5de-115">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="ab5de-116">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ab5de-116">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="ab5de-117">Bu makalede, ASP.NET Core genel ana bilgisayar yer almaktadır (<xref:Microsoft.Extensions.Hosting.HostBuilder>), HTTP isteklerini mıdl'ye işleme uygulamaları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-117">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="ab5de-118">Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-118">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="ab5de-119">Mesajlaşma, arka plan görevleri ve diğer genel ana bilgisayar yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri avantajından temel HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="ab5de-119">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="ab5de-120">Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-120">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="ab5de-121">Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="ab5de-121">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="ab5de-122">Genel konak Web ana bilgisayarı gelecekteki bir sürümde değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="ab5de-122">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

::: moniker-end

<span data-ttu-id="ab5de-123">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab5de-123">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ab5de-124">Örnek uygulamayı çalıştırırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*.</span><span class="sxs-lookup"><span data-stu-id="ab5de-124">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="ab5de-125">Örneği çalıştırma bir `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="ab5de-125">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="ab5de-126">Visual Studio Code'da konsol ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="ab5de-126">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="ab5de-127">Açık *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="ab5de-127">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="ab5de-128">İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi.</span><span class="sxs-lookup"><span data-stu-id="ab5de-128">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="ab5de-129">Değer aşağıdaki seçeneklerden birine ayarlayın `externalTerminal` veya `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="ab5de-129">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="ab5de-130">Giriş</span><span class="sxs-lookup"><span data-stu-id="ab5de-130">Introduction</span></span>

<span data-ttu-id="ab5de-131">Genel Host kitaplığı kullanılabilir <xref:Microsoft.Extensions.Hosting> ad alanı tarafından sağlanan ve [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket.</span><span class="sxs-lookup"><span data-stu-id="ab5de-131">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="ab5de-132">`Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="ab5de-132">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="ab5de-133"><xref:Microsoft.Extensions.Hosting.IHostedService> kod yürütme için giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-133"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="ab5de-134">Her <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama sırasına göre yürütülür [hizmet Createservicereplicalisteners() kaydında](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="ab5de-134">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="ab5de-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> Her çağrılır <xref:Microsoft.Extensions.Hosting.IHostedService> konak başlatıldığında ve <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> kayıt ters sırada konağın düzgün biçimde kapatıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="ab5de-136">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="ab5de-136">Set up a host</span></span>

<span data-ttu-id="ab5de-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> kitaplıkları ve uygulamaları, başlatmak, derleme ve konak çalıştırmak için kullandığınız ana bileşen şöyledir:</span><span class="sxs-lookup"><span data-stu-id="ab5de-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="ab5de-138">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ab5de-138">Options</span></span>

<span data-ttu-id="ab5de-139"><xref:Microsoft.Extensions.Hosting.HostOptions> seçeneklerini yapılandırmak <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-139"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="ab5de-140">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="ab5de-140">Shutdown timeout</span></span>

<span data-ttu-id="ab5de-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> zaman aşımı'için ayarlar <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="ab5de-142">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-142">The default value is five seconds.</span></span>

<span data-ttu-id="ab5de-143">Aşağıdaki seçeneği yapılandırmada `Program.Main` varsayılan beş ikinci kapatma zaman aşımı süresi 20 saniye artırır:</span><span class="sxs-lookup"><span data-stu-id="ab5de-143">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="ab5de-144">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="ab5de-144">Default services</span></span>

<span data-ttu-id="ab5de-145">Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="ab5de-145">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="ab5de-146">[Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="ab5de-146">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="ab5de-147">[Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="ab5de-147">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="ab5de-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="ab5de-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="ab5de-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="ab5de-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="ab5de-150">[Seçenekleri](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="ab5de-150">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="ab5de-151">[Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="ab5de-151">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="ab5de-152">Ana Bilgisayar Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ab5de-152">Host configuration</span></span>

<span data-ttu-id="ab5de-153">Ana Bilgisayar Yapılandırması tarafından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="ab5de-153">Host configuration is created by:</span></span>

* <span data-ttu-id="ab5de-154">Uzantı metotları çağırma <xref:Microsoft.Extensions.Hosting.IHostBuilder> ayarlanacak [içerik kök](#content-root) ve [ortam](#environment).</span><span class="sxs-lookup"><span data-stu-id="ab5de-154">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="ab5de-155">Yapılandırma sağlayıcıları okuma yapılandırmasından <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-155">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="ab5de-156">Genişletme yöntemleri</span><span class="sxs-lookup"><span data-stu-id="ab5de-156">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="ab5de-157">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="ab5de-157">Application key (name)</span></span>

<span data-ttu-id="ab5de-158">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-158">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="ab5de-159">Değeri açıkça ayarlamak için kullanın [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="ab5de-159">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="ab5de-160">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="ab5de-160">**Key**: applicationName</span></span>  
<span data-ttu-id="ab5de-161">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="ab5de-161">**Type**: *string*</span></span>  
<span data-ttu-id="ab5de-162">**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="ab5de-162">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="ab5de-163">**Kullanılarak ayarlanan**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="ab5de-163">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="ab5de-164">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="ab5de-164">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="ab5de-165">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="ab5de-165">Content root</span></span>

<span data-ttu-id="ab5de-166">Bu ayar, içerik dosyalarını aramaya konak başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ab5de-166">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="ab5de-167">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="ab5de-167">**Key**: contentRoot</span></span>  
<span data-ttu-id="ab5de-168">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="ab5de-168">**Type**: *string*</span></span>  
<span data-ttu-id="ab5de-169">**Varsayılan**: Uygulama derleme bulunduğu klasör varsayılan olur.</span><span class="sxs-lookup"><span data-stu-id="ab5de-169">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="ab5de-170">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="ab5de-170">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="ab5de-171">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="ab5de-171">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="ab5de-172">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="ab5de-172">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="ab5de-173">Ortam</span><span class="sxs-lookup"><span data-stu-id="ab5de-173">Environment</span></span>

<span data-ttu-id="ab5de-174">Uygulamanın ayarlar [ortam](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="ab5de-174">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="ab5de-175">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="ab5de-175">**Key**: environment</span></span>  
<span data-ttu-id="ab5de-176">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="ab5de-176">**Type**: *string*</span></span>  
<span data-ttu-id="ab5de-177">**Varsayılan**: Üretim</span><span class="sxs-lookup"><span data-stu-id="ab5de-177">**Default**: Production</span></span>  
<span data-ttu-id="ab5de-178">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="ab5de-178">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="ab5de-179">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="ab5de-179">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="ab5de-180">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-180">The environment can be set to any value.</span></span> <span data-ttu-id="ab5de-181">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="ab5de-181">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="ab5de-182">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-182">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="ab5de-183">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="ab5de-183">ConfigureHostConfiguration</span></span>

<span data-ttu-id="ab5de-184"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> konak için.</span><span class="sxs-lookup"><span data-stu-id="ab5de-184"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="ab5de-185">Konak yapılandırmasının başlatmak için kullanılan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> kullanılmak üzere uygulamanın derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="ab5de-185">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="ab5de-186"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-186"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="ab5de-187">Ana bilgisayarın hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-187">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="ab5de-188">Ana bilgisayar yapılandırması otomatik olarak uygulama yapılandırmasının akışları ([ConfigureAppConfiguration](#configureappconfiguration) ve uygulamayı geri kalanı).</span><span class="sxs-lookup"><span data-stu-id="ab5de-188">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="ab5de-189">Yok sağlayıcıları varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-189">No providers are included by default.</span></span> <span data-ttu-id="ab5de-190">Uygulama gerekiyor, herhangi bir yapılandırma sağlayıcısı açıkça belirtmeniz gerekir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>de dahil olmak üzere:</span><span class="sxs-lookup"><span data-stu-id="ab5de-190">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="ab5de-191">Dosya yapılandırması (örneğin, bir *hostsettings.json* dosyası).</span><span class="sxs-lookup"><span data-stu-id="ab5de-191">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="ab5de-192">Ortam değişkeni yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="ab5de-192">Environment variable configuration.</span></span>
* <span data-ttu-id="ab5de-193">Komut satırı bağımsız yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="ab5de-193">Command-line argument configuration.</span></span>
* <span data-ttu-id="ab5de-194">Herhangi diğer gerekli yapılandırma sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="ab5de-194">Any other required configuration providers.</span></span>

<span data-ttu-id="ab5de-195">Ana bilgisayar dosya yapılandırması ile uygulamanın taban yolu belirterek etkin `SetBasePath` birine bir çağrı tarafından izlenen [yapılandırma sağlayıcıları dosya](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ab5de-195">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="ab5de-196">Bir JSON dosyası örnek uygulamanın kullandığı *hostsettings.json*ve aramalar <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> dosyanın ana bilgisayar yapılandırma ayarlarını kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="ab5de-196">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="ab5de-197">Eklenecek [ortam değişkeni yapılandırma](xref:fundamentals/configuration/index#environment-variables-configuration-provider) ana bilgisayarının çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> konak oluşturucu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="ab5de-197">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="ab5de-198">`AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ab5de-198">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="ab5de-199">Bir önek örnek uygulamanın kullandığı `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="ab5de-199">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="ab5de-200">Ön ek ortam değişkenlerini okunduğunda kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-200">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="ab5de-201">Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="ab5de-201">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="ab5de-202">Kullanırken, geliştirme sırasında [Visual Studio](https://visualstudio.microsoft.com) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="ab5de-202">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="ab5de-203">İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="ab5de-203">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="ab5de-204">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-204">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="ab5de-205">[Komut satırı yapılandırma](xref:fundamentals/configuration/index#command-line-configuration-provider) çağrılarak eklenir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-205">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="ab5de-206">Komut satırı yapılandırma önceki yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek için son eklenir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-206">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="ab5de-207">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="ab5de-207">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="ab5de-208">Ek yapılandırma ile sağlanabilir [applicationName](#application-key-name) ve [contentRoot](#content-root) anahtarları.</span><span class="sxs-lookup"><span data-stu-id="ab5de-208">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="ab5de-209">Örnek `HostBuilder` yapılandırmayla <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="ab5de-209">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="ab5de-210">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="ab5de-210">ConfigureAppConfiguration</span></span>

<span data-ttu-id="ab5de-211">Uygulama yapılandırması çağırılarak oluşturulur <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> üzerinde <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulaması.</span><span class="sxs-lookup"><span data-stu-id="ab5de-211">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="ab5de-212"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> uygulama için.</span><span class="sxs-lookup"><span data-stu-id="ab5de-212"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="ab5de-213"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-213"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="ab5de-214">Uygulama, hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-214">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="ab5de-215">Tarafından oluşturulan yapılandırmayı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) sonraki işlemleri ve buna <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-215">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="ab5de-216">Uygulama Yapılandırması tarafından sağlanan ana bilgisayar yapılandırması otomatik olarak aldığı [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="ab5de-216">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="ab5de-217">Örnek uygulama yapılandırmasını kullanma <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="ab5de-217">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="ab5de-218">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ab5de-218">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="ab5de-219">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="ab5de-219">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="ab5de-220">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="ab5de-220">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="ab5de-221">Ayarları dosyalar çıkış dizinine taşımak için ayarları dosyaları olarak belirtin. [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="ab5de-221">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="ab5de-222">Örnek uygulamasını kendi JSON uygulama ayarları dosyaları taşır ve *hostsettings.json* aşağıdaki `<Content>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="ab5de-222">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="ab5de-223">Yapılandırma genişletme yöntemleri gibi <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> ve <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> ek NuGet paketleri gibi gereken [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) ve [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="ab5de-223">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="ab5de-224">Uygulamanın kullandığı sürece [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), bu paketleri çekirdek ek olarak projeye eklenmelidir [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) paket.</span><span class="sxs-lookup"><span data-stu-id="ab5de-224">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="ab5de-225">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-225">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="ab5de-226">Createservicereplicalisteners()</span><span class="sxs-lookup"><span data-stu-id="ab5de-226">ConfigureServices</span></span>

<span data-ttu-id="ab5de-227"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> uygulamanın Hizmetleri ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="ab5de-227"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="ab5de-228"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-228"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="ab5de-229">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="ab5de-229">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="ab5de-230">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-230">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="ab5de-231">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="ab5de-231">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="ab5de-232">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="ab5de-232">ConfigureLogging</span></span>

<span data-ttu-id="ab5de-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> sağlanan yapılandırmak için bir temsilci ekler <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="ab5de-234"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-234"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="ab5de-235">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="ab5de-235">UseConsoleLifetime</span></span>

<span data-ttu-id="ab5de-236"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> CTRL + C/SIGINT veya SIGTERM ve aramalar için dinler <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kapatma işlemi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="ab5de-236"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="ab5de-237"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="ab5de-237"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="ab5de-238"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Varsayılan yaşam süresi uygulaması önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-238"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="ab5de-239">Kaydedilen son ömrü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-239">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="ab5de-240">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ab5de-240">Container configuration</span></span>

<span data-ttu-id="ab5de-241">Diğer kapsayıcılarda takma desteklemek için konak alabilen bir <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-241">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="ab5de-242">Fabrika sağlama DI kapsayıcı kaydı bir parçası değildir, ancak bunun yerine somut DI kapsayıcıyı oluşturmak için kullanılan bir iç yöneticisidir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-242">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="ab5de-243">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ab5de-243">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="ab5de-244">Özel kapsayıcı yapılandırması tarafından yönetilir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ab5de-244">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="ab5de-245"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> kapsayıcısının üstünde API altta yatan ana bilgisayar yapılandırma türü kesin belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab5de-245"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="ab5de-246"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-246"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="ab5de-247">App service kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ab5de-247">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="ab5de-248">Bir hizmet kapsayıcı üreteci sağlar:</span><span class="sxs-lookup"><span data-stu-id="ab5de-248">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="ab5de-249">Fabrika kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ab5de-249">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="ab5de-250">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="ab5de-250">Extensibility</span></span>

<span data-ttu-id="ab5de-251">Konak genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-251">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="ab5de-252">Aşağıdaki örnek, nasıl bir genişletme yönteminin genişlettiği gösterir. bir <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasıyla [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) gösterilen örnek içinde <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-252">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="ab5de-253">Bir uygulama oluşturur `UseHostedService` genişletme yöntemi, barındırılan hizmeti kaydetmek için geçirilen `T`:</span><span class="sxs-lookup"><span data-stu-id="ab5de-253">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="ab5de-254">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="ab5de-254">Manage the host</span></span>

<span data-ttu-id="ab5de-255"><xref:Microsoft.Extensions.Hosting.IHost> Uygulamasıdır ve durdurmaktan sorumludur <xref:Microsoft.Extensions.Hosting.IHostedService> service kapsayıcısında kayıtlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="ab5de-255">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="ab5de-256">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="ab5de-256">Run</span></span>

<span data-ttu-id="ab5de-257"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="ab5de-257"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="ab5de-258">RunAsync</span><span class="sxs-lookup"><span data-stu-id="ab5de-258">RunAsync</span></span>

<span data-ttu-id="ab5de-259"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uygulama çalışır ve döndüren bir <xref:System.Threading.Tasks.Task> iptal belirteci veya kapatma tetiklendiğinde tamamlar:</span><span class="sxs-lookup"><span data-stu-id="ab5de-259"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="ab5de-260">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="ab5de-260">RunConsoleAsync</span></span>

<span data-ttu-id="ab5de-261"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve kapatmak Ctrl + C/SIGINT veya SIGTERM bekler.</span><span class="sxs-lookup"><span data-stu-id="ab5de-261"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="ab5de-262">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="ab5de-262">Start and StopAsync</span></span>

<span data-ttu-id="ab5de-263"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> ana bilgisayar zaman uyumlu olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="ab5de-263"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="ab5de-264"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> Belirtilen zaman aşımı süresi içinde konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-264"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="ab5de-265">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="ab5de-265">StartAsync and StopAsync</span></span>

<span data-ttu-id="ab5de-266"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="ab5de-266"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="ab5de-267"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> uygulamayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="ab5de-267"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="ab5de-268">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="ab5de-268">WaitForShutdown</span></span>

<span data-ttu-id="ab5de-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> aracılığıyla tetiklenen <xref:Microsoft.Extensions.Hosting.IHostLifetime>, gibi <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (Ctrl + C/SIGINT veya SIGTERM dinler).</span><span class="sxs-lookup"><span data-stu-id="ab5de-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="ab5de-270"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> çağrıları <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-270"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="ab5de-271">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="ab5de-271">WaitForShutdownAsync</span></span>

<span data-ttu-id="ab5de-272"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> döndürür bir <xref:System.Threading.Tasks.Task> kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-272"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="ab5de-273">Dış denetimi</span><span class="sxs-lookup"><span data-stu-id="ab5de-273">External control</span></span>

<span data-ttu-id="ab5de-274">Dış denetim konağının harici olarak çağrılabilir yöntemleri kullanılarak elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="ab5de-274">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="ab5de-275"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> başlangıcında çağırılır <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, devam etmeden önce işlemi tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="ab5de-275"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="ab5de-276">Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ab5de-276">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="ab5de-277">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="ab5de-277">IHostingEnvironment interface</span></span>

<span data-ttu-id="ab5de-278"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> uygulama hakkındaki bilgileri barındırma ortamı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab5de-278"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="ab5de-279">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="ab5de-279">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="ab5de-280">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ab5de-280">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="ab5de-281">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="ab5de-281">IApplicationLifetime interface</span></span>

<span data-ttu-id="ab5de-282"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> normal şekilde kapatılmasını istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab5de-282"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="ab5de-283">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini <xref:System.Action> başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="ab5de-283">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="ab5de-284">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="ab5de-284">Cancellation Token</span></span> | <span data-ttu-id="ab5de-285">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="ab5de-285">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="ab5de-286">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="ab5de-286">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="ab5de-287">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="ab5de-287">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="ab5de-288">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="ab5de-288">All requests should be processed.</span></span> <span data-ttu-id="ab5de-289">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="ab5de-289">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="ab5de-290">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="ab5de-290">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="ab5de-291">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="ab5de-291">Requests may still be processing.</span></span> <span data-ttu-id="ab5de-292">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="ab5de-292">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="ab5de-293">Oluşturucu Ekle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> herhangi bir sınıf içinde hizmet.</span><span class="sxs-lookup"><span data-stu-id="ab5de-293">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="ab5de-294">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama) olaylarını kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="ab5de-294">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="ab5de-295">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="ab5de-295">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="ab5de-296"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> uygulamanın sonlandırma ister.</span><span class="sxs-lookup"><span data-stu-id="ab5de-296"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="ab5de-297">Aşağıdaki sınıf kullandığı <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="ab5de-297">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ab5de-298">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ab5de-298">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
