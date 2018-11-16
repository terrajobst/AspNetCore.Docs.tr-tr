---
title: .NET genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu .NET genel ana bilgisayar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: cac5ccdea7838d26b7468f9bf1ab8d317b444b46
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708523"
---
# <a name="net-generic-host"></a><span data-ttu-id="660fe-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="660fe-103">.NET Generic Host</span></span>

<span data-ttu-id="660fe-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="660fe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="660fe-105">.NET core uygulamaları yapılandırmak ve başlatmak bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="660fe-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="660fe-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="660fe-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="660fe-107">Bu konu, ASP.NET Core genel ana bilgisayar kapsar (<xref:Microsoft.Extensions.Hosting.HostBuilder>), HTTP isteklerini mıdl'ye işleme uygulamaları barındırmak için kullanışlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="660fe-107">This topic covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="660fe-108">İçin Web ana bilgisayarı kapsamını (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), bkz: <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="660fe-108">For coverage of the Web Host (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="660fe-109">Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır.</span><span class="sxs-lookup"><span data-stu-id="660fe-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="660fe-110">Mesajlaşma, arka plan görevleri ve diğer yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri genel ana bilgisayar avantajından temel HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="660fe-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="660fe-111">Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="660fe-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="660fe-112">Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="660fe-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="660fe-113">Gelecek sürümlerden birinde Web ana bilgisayarı değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API olarak davranmak için geliştirme aşamasındaki genel ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="660fe-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="660fe-114">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="660fe-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="660fe-115">Örnek uygulamayı çalıştırırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*.</span><span class="sxs-lookup"><span data-stu-id="660fe-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="660fe-116">Örneği çalıştırma bir `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="660fe-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="660fe-117">Visual Studio Code'da konsol ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="660fe-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="660fe-118">Açık *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="660fe-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="660fe-119">İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi.</span><span class="sxs-lookup"><span data-stu-id="660fe-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="660fe-120">Değer aşağıdaki seçeneklerden birine ayarlayın `externalTerminal` veya `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="660fe-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="660fe-121">Giriş</span><span class="sxs-lookup"><span data-stu-id="660fe-121">Introduction</span></span>

<span data-ttu-id="660fe-122">Genel Host kitaplığı kullanılabilir <xref:Microsoft.Extensions.Hosting> ad alanı tarafından sağlanan ve [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket.</span><span class="sxs-lookup"><span data-stu-id="660fe-122">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="660fe-123">`Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="660fe-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="660fe-124"><xref:Microsoft.Extensions.Hosting.IHostedService> kod yürütme için giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="660fe-124"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="660fe-125">Her `IHostedService` uygulama sırasına göre yürütülür [hizmet Createservicereplicalisteners() kaydında](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="660fe-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="660fe-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> Her çağrılır `IHostedService` konak başlatıldığında ve <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> kayıt ters sırada konağın düzgün biçimde kapatıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="660fe-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each `IHostedService` when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="660fe-127">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="660fe-127">Set up a host</span></span>

<span data-ttu-id="660fe-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> kitaplıkları ve uygulamaları, başlatmak, derleme ve konak çalıştırmak için kullandığınız ana bileşen şöyledir:</span><span class="sxs-lookup"><span data-stu-id="660fe-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="660fe-129">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="660fe-129">Default services</span></span>

<span data-ttu-id="660fe-130">Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="660fe-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="660fe-131">[Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="660fe-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="660fe-132">[Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="660fe-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="660fe-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="660fe-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="660fe-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="660fe-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="660fe-135">[Seçenekleri](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="660fe-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="660fe-136">[Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="660fe-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="660fe-137">Ana Bilgisayar Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="660fe-137">Host configuration</span></span>

<span data-ttu-id="660fe-138">Ana Bilgisayar Yapılandırması tarafından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="660fe-138">Host configuration is created by:</span></span>

* <span data-ttu-id="660fe-139">Uzantı metotları çağırma <xref:Microsoft.Extensions.Hosting.IHostBuilder> ayarlanacak [içerik kök](#content-root) ve [ortam](#environment).</span><span class="sxs-lookup"><span data-stu-id="660fe-139">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="660fe-140">Yapılandırma sağlayıcıları okuma yapılandırmasından <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="660fe-140">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="660fe-141">Genişletme yöntemleri</span><span class="sxs-lookup"><span data-stu-id="660fe-141">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="660fe-142">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="660fe-142">Application key (name)</span></span>

<span data-ttu-id="660fe-143">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="660fe-143">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="660fe-144">Değeri açıkça ayarlamak için kullanın [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="660fe-144">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="660fe-145">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="660fe-145">**Key**: applicationName</span></span>  
<span data-ttu-id="660fe-146">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="660fe-146">**Type**: *string*</span></span>  
<span data-ttu-id="660fe-147">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="660fe-147">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="660fe-148">**Kullanılarak ayarlanan**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="660fe-148">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="660fe-149">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="660fe-149">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="660fe-150">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="660fe-150">Content root</span></span>

<span data-ttu-id="660fe-151">Bu ayar, içerik dosyalarını aramaya konak başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="660fe-151">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="660fe-152">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="660fe-152">**Key**: contentRoot</span></span>  
<span data-ttu-id="660fe-153">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="660fe-153">**Type**: *string*</span></span>  
<span data-ttu-id="660fe-154">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="660fe-154">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="660fe-155">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="660fe-155">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="660fe-156">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="660fe-156">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="660fe-157">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="660fe-157">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="660fe-158">Ortam</span><span class="sxs-lookup"><span data-stu-id="660fe-158">Environment</span></span>

<span data-ttu-id="660fe-159">Uygulamanın ayarlar [ortam](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="660fe-159">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="660fe-160">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="660fe-160">**Key**: environment</span></span>  
<span data-ttu-id="660fe-161">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="660fe-161">**Type**: *string*</span></span>  
<span data-ttu-id="660fe-162">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="660fe-162">**Default**: Production</span></span>  
<span data-ttu-id="660fe-163">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="660fe-163">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="660fe-164">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="660fe-164">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="660fe-165">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-165">The environment can be set to any value.</span></span> <span data-ttu-id="660fe-166">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="660fe-166">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="660fe-167">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="660fe-167">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="660fe-168">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="660fe-168">ConfigureHostConfiguration</span></span>

<span data-ttu-id="660fe-169">`ConfigureHostConfiguration` kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> konak için.</span><span class="sxs-lookup"><span data-stu-id="660fe-169">`ConfigureHostConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="660fe-170">Konak yapılandırmasının başlatmak için kullanılan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> kullanılmak üzere uygulamanın derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="660fe-170">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="660fe-171">`ConfigureHostConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-171">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="660fe-172">Ana bilgisayarın hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="660fe-172">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="660fe-173">Ana bilgisayar yapılandırması otomatik olarak uygulama yapılandırmasının akışları ([ConfigureAppConfiguration](#configureappconfiguration) ve uygulamayı geri kalanı).</span><span class="sxs-lookup"><span data-stu-id="660fe-173">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="660fe-174">Yok sağlayıcıları varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-174">No providers are included by default.</span></span> <span data-ttu-id="660fe-175">Uygulama gerekiyor, herhangi bir yapılandırma sağlayıcısı açıkça belirtmeniz gerekir `ConfigureHostConfiguration`de dahil olmak üzere:</span><span class="sxs-lookup"><span data-stu-id="660fe-175">You must explicitly specify whatever configuration providers the app requires in `ConfigureHostConfiguration`, including:</span></span>

* <span data-ttu-id="660fe-176">Dosya yapılandırması (örneğin, bir *hostsettings.json* dosyası).</span><span class="sxs-lookup"><span data-stu-id="660fe-176">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="660fe-177">Ortam değişkeni yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="660fe-177">Environment variable configuration.</span></span>
* <span data-ttu-id="660fe-178">Komut satırı bağımsız yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="660fe-178">Command-line argument configuration.</span></span>
* <span data-ttu-id="660fe-179">Herhangi diğer gerekli yapılandırma sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="660fe-179">Any other required configuration providers.</span></span>

<span data-ttu-id="660fe-180">Ana bilgisayar dosya yapılandırması ile uygulamanın taban yolu belirterek etkin `SetBasePath` birine bir çağrı tarafından izlenen [yapılandırma sağlayıcıları dosya](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="660fe-180">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="660fe-181">Bir JSON dosyası örnek uygulamanın kullandığı *hostsettings.json*ve aramalar <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> dosyanın ana bilgisayar yapılandırma ayarlarını kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="660fe-181">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="660fe-182">Eklenecek [ortam değişkeni yapılandırma](xref:fundamentals/configuration/index#environment-variables-configuration-provider) ana bilgisayarının çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> konak oluşturucu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="660fe-182">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="660fe-183">`AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder.</span><span class="sxs-lookup"><span data-stu-id="660fe-183">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="660fe-184">Bir önek örnek uygulamanın kullandığı `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="660fe-184">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="660fe-185">Ön ek ortam değişkenlerini okunduğunda kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="660fe-185">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="660fe-186">Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="660fe-186">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="660fe-187">Kullanırken, geliştirme sırasında [Visual Studio](https://www.visualstudio.com/) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="660fe-187">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="660fe-188">İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="660fe-188">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="660fe-189">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="660fe-189">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="660fe-190">[Komut satırı yapılandırma](xref:fundamentals/configuration/index#command-line-configuration-provider) çağrılarak eklenir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="660fe-190">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="660fe-191">Komut satırı yapılandırma önceki yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek için son eklenir.</span><span class="sxs-lookup"><span data-stu-id="660fe-191">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="660fe-192">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="660fe-192">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="660fe-193">Ek yapılandırma ile sağlanabilir [applicationName](#application-key-name) ve [contentRoot](#content-root) anahtarları.</span><span class="sxs-lookup"><span data-stu-id="660fe-193">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="660fe-194">Örnek `HostBuilder` yapılandırmayla `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="660fe-194">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="660fe-195">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="660fe-195">ConfigureAppConfiguration</span></span>

<span data-ttu-id="660fe-196">Uygulama yapılandırması çağırılarak oluşturulur <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> üzerinde <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulaması.</span><span class="sxs-lookup"><span data-stu-id="660fe-196">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="660fe-197">`ConfigureAppConfiguration` kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> uygulama için.</span><span class="sxs-lookup"><span data-stu-id="660fe-197">`ConfigureAppConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="660fe-198">`ConfigureAppConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-198">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="660fe-199">Uygulama, hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="660fe-199">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="660fe-200">Tarafından oluşturulan yapılandırmayı `ConfigureAppConfiguration` kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) sonraki işlemleri ve buna <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="660fe-200">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="660fe-201">Uygulama Yapılandırması tarafından sağlanan ana bilgisayar yapılandırması otomatik olarak aldığı [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="660fe-201">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="660fe-202">Örnek uygulama yapılandırmasını kullanma `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="660fe-202">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="660fe-203">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="660fe-203">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="660fe-204">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="660fe-204">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="660fe-205">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="660fe-205">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="660fe-206">Ayarları dosyalar çıkış dizinine taşımak için ayarları dosyaları olarak belirtin. [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="660fe-206">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="660fe-207">Örnek uygulamasını kendi JSON uygulama ayarları dosyaları taşır ve *hostsettings.json* aşağıdaki `<Content>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="660fe-207">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="660fe-208">Createservicereplicalisteners()</span><span class="sxs-lookup"><span data-stu-id="660fe-208">ConfigureServices</span></span>

<span data-ttu-id="660fe-209"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> uygulamanın Hizmetleri ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="660fe-209"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="660fe-210">`ConfigureServices` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-210">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="660fe-211">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="660fe-211">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="660fe-212">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="660fe-212">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="660fe-213">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="660fe-213">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="660fe-214">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="660fe-214">ConfigureLogging</span></span>

<span data-ttu-id="660fe-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> sağlanan yapılandırmak için bir temsilci ekler <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="660fe-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="660fe-216">`ConfigureLogging` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-216">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="660fe-217">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="660fe-217">UseConsoleLifetime</span></span>

<span data-ttu-id="660fe-218"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> dinler `Ctrl+C`/SIGINT veya SIGTERM ve çağrıları <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kapatma işlemi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="660fe-218"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for `Ctrl+C`/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="660fe-219">`UseConsoleLifetime` Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="660fe-219">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="660fe-220"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Varsayılan yaşam süresi uygulaması önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-220"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="660fe-221">Kaydedilen son ömrü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="660fe-221">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="660fe-222">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="660fe-222">Container configuration</span></span>

<span data-ttu-id="660fe-223">Diğer kapsayıcılarda takma desteklemek için konak alabilen bir <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span><span class="sxs-lookup"><span data-stu-id="660fe-223">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span></span> <span data-ttu-id="660fe-224">Fabrika sağlama DI kapsayıcı kaydı bir parçası değildir, ancak bunun yerine somut DI kapsayıcıyı oluşturmak için kullanılan bir iç yöneticisidir.</span><span class="sxs-lookup"><span data-stu-id="660fe-224">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="660fe-225">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="660fe-225">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="660fe-226">Özel kapsayıcı yapılandırması tarafından yönetilir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="660fe-226">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="660fe-227">`ConfigureContainer` kapsayıcısının üstünde API altta yatan ana bilgisayar yapılandırma türü kesin belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="660fe-227">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="660fe-228">`ConfigureContainer` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-228">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="660fe-229">App service kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="660fe-229">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="660fe-230">Bir hizmet kapsayıcı üreteci sağlar:</span><span class="sxs-lookup"><span data-stu-id="660fe-230">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="660fe-231">Fabrika kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="660fe-231">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="660fe-232">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="660fe-232">Extensibility</span></span>

<span data-ttu-id="660fe-233">Konak genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="660fe-233">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="660fe-234">Aşağıdaki örnek, nasıl bir genişletme yönteminin genişlettiği gösterir. bir `IHostBuilder` uygulamasıyla [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) gösterilen örnek içinde <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="660fe-234">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="660fe-235">Bir uygulama oluşturur `UseHostedService` genişletme yöntemi, barındırılan hizmeti kaydetmek için geçirilen `T`:</span><span class="sxs-lookup"><span data-stu-id="660fe-235">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="660fe-236">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="660fe-236">Manage the host</span></span>

<span data-ttu-id="660fe-237"><xref:Microsoft.Extensions.Hosting.IHost> Uygulamasıdır ve durdurmaktan sorumludur `IHostedService` service kapsayıcısında kayıtlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="660fe-237">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="660fe-238">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="660fe-238">Run</span></span>

<span data-ttu-id="660fe-239"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="660fe-239"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="660fe-240">RunAsync</span><span class="sxs-lookup"><span data-stu-id="660fe-240">RunAsync</span></span>

<span data-ttu-id="660fe-241"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uygulama çalışır ve döndüren bir `Task` iptal belirteci veya kapatma tetiklendiğinde tamamlar:</span><span class="sxs-lookup"><span data-stu-id="660fe-241"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="660fe-242">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="660fe-242">RunConsoleAsync</span></span>

<span data-ttu-id="660fe-243"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve bekler `Ctrl+C`/SIGINT veya SIGTERM kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="660fe-243"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="660fe-244">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="660fe-244">Start and StopAsync</span></span>

<span data-ttu-id="660fe-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> ana bilgisayar zaman uyumlu olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="660fe-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="660fe-246"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> Belirtilen zaman aşımı süresi içinde konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="660fe-246"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="660fe-247">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="660fe-247">StartAsync and StopAsync</span></span>

<span data-ttu-id="660fe-248"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="660fe-248"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="660fe-249"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> uygulamayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="660fe-249"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="660fe-250">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="660fe-250">WaitForShutdown</span></span>

<span data-ttu-id="660fe-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> aracılığıyla tetiklenen <xref:Microsoft.Extensions.Hosting.IHostLifetime>, gibi <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (dinler `Ctrl+C`/SIGINT veya SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="660fe-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="660fe-252">`WaitForShutdown` çağrıları <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="660fe-252">`WaitForShutdown` calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="660fe-253">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="660fe-253">WaitForShutdownAsync</span></span>

<span data-ttu-id="660fe-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> döndürür bir `Task` kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="660fe-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a `Task` that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="660fe-255">Dış denetimi</span><span class="sxs-lookup"><span data-stu-id="660fe-255">External control</span></span>

<span data-ttu-id="660fe-256">Dış denetim konağının harici olarak çağrılabilir yöntemleri kullanılarak elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="660fe-256">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="660fe-257"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> başlangıcında çağırılır <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, devam etmeden önce işlemi tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="660fe-257"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="660fe-258">Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="660fe-258">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="660fe-259">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="660fe-259">IHostingEnvironment interface</span></span>

<span data-ttu-id="660fe-260"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> uygulama hakkındaki bilgileri barındırma ortamı sağlar.</span><span class="sxs-lookup"><span data-stu-id="660fe-260"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="660fe-261">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="660fe-261">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="660fe-262">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="660fe-262">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="660fe-263">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="660fe-263">IApplicationLifetime interface</span></span>

<span data-ttu-id="660fe-264"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> normal şekilde kapatılmasını istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="660fe-264"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="660fe-265">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="660fe-265">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="660fe-266">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="660fe-266">Cancellation Token</span></span> | <span data-ttu-id="660fe-267">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="660fe-267">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="660fe-268">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="660fe-268">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="660fe-269">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="660fe-269">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="660fe-270">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="660fe-270">All requests should be processed.</span></span> <span data-ttu-id="660fe-271">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="660fe-271">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="660fe-272">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="660fe-272">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="660fe-273">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="660fe-273">Requests may still be processing.</span></span> <span data-ttu-id="660fe-274">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="660fe-274">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="660fe-275">Oluşturucu Ekle `IApplicationLifetime` herhangi bir sınıf içinde hizmet.</span><span class="sxs-lookup"><span data-stu-id="660fe-275">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="660fe-276">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir `IHostedService` uygulama) olaylarını kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="660fe-276">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="660fe-277">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="660fe-277">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="660fe-278"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> uygulamanın sonlandırma ister.</span><span class="sxs-lookup"><span data-stu-id="660fe-278"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="660fe-279">Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="660fe-279">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="660fe-280">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="660fe-280">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="660fe-281">GitHub örnekleri deposu barındırma</span><span class="sxs-lookup"><span data-stu-id="660fe-281">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
