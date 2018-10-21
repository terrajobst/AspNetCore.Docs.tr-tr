---
title: .NET genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu .NET genel ana bilgisayar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0924e2764958911dc1711d5427f6dd58e8873739
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477611"
---
# <a name="net-generic-host"></a><span data-ttu-id="f0f31-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="f0f31-103">.NET Generic Host</span></span>

<span data-ttu-id="f0f31-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f0f31-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f0f31-105">.NET core uygulamaları yapılandırmak ve başlatmak bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="f0f31-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="f0f31-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="f0f31-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f0f31-107">Bu konu, ASP.NET Core genel ana bilgisayar kapsar ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), HTTP isteklerini mıdl'ye işleme uygulamaları barındırmak için kullanışlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="f0f31-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="f0f31-108">İçin Web ana bilgisayarı kapsamını ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), bkz: <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="f0f31-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="f0f31-109">Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="f0f31-110">Mesajlaşma, arka plan görevleri ve diğer yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri genel ana bilgisayar avantajından temel HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="f0f31-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="f0f31-111">Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="f0f31-112">Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="f0f31-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="f0f31-113">Gelecek sürümlerden birinde Web ana bilgisayarı değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API olarak davranmak için geliştirme aşamasındaki genel ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="f0f31-114">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0f31-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f0f31-115">Örnek uygulamayı çalıştırırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*.</span><span class="sxs-lookup"><span data-stu-id="f0f31-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="f0f31-116">Örneği çalıştırma bir `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="f0f31-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="f0f31-117">Visual Studio Code'da konsol ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="f0f31-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="f0f31-118">Açık *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="f0f31-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="f0f31-119">İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi.</span><span class="sxs-lookup"><span data-stu-id="f0f31-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="f0f31-120">Değer aşağıdaki seçeneklerden birine ayarlayın `externalTerminal` veya `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="f0f31-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="f0f31-121">Giriş</span><span class="sxs-lookup"><span data-stu-id="f0f31-121">Introduction</span></span>

<span data-ttu-id="f0f31-122">Genel Host kitaplığı kullanılabilir [Microsoft.Extensions.Hosting ad alanı](/dotnet/api/microsoft.extensions.hosting) ve tarafından sağlanan [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket.</span><span class="sxs-lookup"><span data-stu-id="f0f31-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="f0f31-123">`Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="f0f31-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="f0f31-124">[Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) kod yürütme için giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="f0f31-125">Her `IHostedService` uygulama sırasına göre yürütülür [hizmet Createservicereplicalisteners() kaydında](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="f0f31-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="f0f31-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) her çağrılır `IHostedService` konak başlatıldığında ve [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) kayıt ters sırada konağın düzgün biçimde kapatıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="f0f31-127">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="f0f31-127">Set up a host</span></span>

<span data-ttu-id="f0f31-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) kitaplıkları ve uygulamaları, başlatmak, derleme ve konak çalıştırmak için kullandığınız ana bileşeni:</span><span class="sxs-lookup"><span data-stu-id="f0f31-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="f0f31-129">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="f0f31-129">Default services</span></span>

<span data-ttu-id="f0f31-130">Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="f0f31-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="f0f31-131">[Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="f0f31-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="f0f31-132">[Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="f0f31-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="f0f31-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="f0f31-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="f0f31-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="f0f31-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="f0f31-135">[Seçenekleri](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="f0f31-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="f0f31-136">[Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="f0f31-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="f0f31-137">Ana Bilgisayar Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="f0f31-137">Host configuration</span></span>

<span data-ttu-id="f0f31-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="f0f31-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="f0f31-139">Yapılandırma oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="f0f31-139">Configuration builder</span></span>
* <span data-ttu-id="f0f31-140">Uzantı yöntemi yapılandırması</span><span class="sxs-lookup"><span data-stu-id="f0f31-140">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="f0f31-141">Yapılandırma oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="f0f31-141">Configuration builder</span></span>

<span data-ttu-id="f0f31-142">Ana bilgisayar Oluşturucu yapılandırması çağırılarak oluşturulur [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="f0f31-142">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="f0f31-143">`ConfigureHostConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) konak için.</span><span class="sxs-lookup"><span data-stu-id="f0f31-143">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="f0f31-144">Yapılandırma oluşturucuyu başlatır [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) kullanılmak üzere uygulamanın derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="f0f31-144">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="f0f31-145">Ortam değişkeni yapılandırma varsayılan olarak eklenmez.</span><span class="sxs-lookup"><span data-stu-id="f0f31-145">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="f0f31-146">Çağrı [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) ortam değişkenlerini konaktan yapılandırmak için konak oluşturucu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="f0f31-146">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="f0f31-147">`AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder.</span><span class="sxs-lookup"><span data-stu-id="f0f31-147">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="f0f31-148">Bir önek örnek uygulamanın kullandığı `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="f0f31-148">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="f0f31-149">Ön ek ortam değişkenlerini okunduğunda kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-149">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="f0f31-150">Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="f0f31-150">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="f0f31-151">Kullanırken, geliştirme sırasında [Visual Studio](https://www.visualstudio.com/) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="f0f31-151">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="f0f31-152">İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="f0f31-152">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="f0f31-153">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="f0f31-153">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="f0f31-154">`ConfigureHostConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-154">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="f0f31-155">Konak, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-155">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="f0f31-156">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="f0f31-156">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="f0f31-157">Örnek `HostBuilder` yapılandırmayla `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="f0f31-157">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="f0f31-158">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="f0f31-158">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="f0f31-159">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="f0f31-159">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="f0f31-160">`AddConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `HostBuilder` anahtarları (örneğin, `environment`).</span><span class="sxs-lookup"><span data-stu-id="f0f31-160">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="f0f31-161">Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="f0f31-161">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="f0f31-162">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-162">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="f0f31-163">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="f0f31-163">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="f0f31-164">Uzantı yöntemi yapılandırması</span><span class="sxs-lookup"><span data-stu-id="f0f31-164">Extension method configuration</span></span>

<span data-ttu-id="f0f31-165">Uzantı yöntemleri üzerinde çağrıldığında `IHostBuilder` içerik kök ve ortamı yapılandırmak için uygulama.</span><span class="sxs-lookup"><span data-stu-id="f0f31-165">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="f0f31-166">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="f0f31-166">Application Key (Name)</span></span>

<span data-ttu-id="f0f31-167">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-167">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="f0f31-168">Değeri açıkça ayarlamak için kullanın [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="f0f31-168">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="f0f31-169">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="f0f31-169">**Key**: applicationName</span></span>  
<span data-ttu-id="f0f31-170">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="f0f31-170">**Type**: *string*</span></span>  
<span data-ttu-id="f0f31-171">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="f0f31-171">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="f0f31-172">**Kullanılarak ayarlanan**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="f0f31-172">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="f0f31-173">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="f0f31-173">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="f0f31-174">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="f0f31-174">Content Root</span></span>

<span data-ttu-id="f0f31-175">Bu ayar, içerik dosyalarını aramaya konak başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="f0f31-175">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="f0f31-176">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="f0f31-176">**Key**: contentRoot</span></span>  
<span data-ttu-id="f0f31-177">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="f0f31-177">**Type**: *string*</span></span>  
<span data-ttu-id="f0f31-178">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="f0f31-178">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="f0f31-179">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="f0f31-179">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="f0f31-180">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="f0f31-180">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="f0f31-181">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="f0f31-181">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="f0f31-182">Ortam</span><span class="sxs-lookup"><span data-stu-id="f0f31-182">Environment</span></span>

<span data-ttu-id="f0f31-183">Uygulamanın ayarlar [ortam](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f0f31-183">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="f0f31-184">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="f0f31-184">**Key**: environment</span></span>  
<span data-ttu-id="f0f31-185">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="f0f31-185">**Type**: *string*</span></span>  
<span data-ttu-id="f0f31-186">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="f0f31-186">**Default**: Production</span></span>  
<span data-ttu-id="f0f31-187">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="f0f31-187">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="f0f31-188">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="f0f31-188">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="f0f31-189">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-189">The environment can be set to any value.</span></span> <span data-ttu-id="f0f31-190">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="f0f31-190">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="f0f31-191">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-191">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="f0f31-192">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="f0f31-192">ConfigureAppConfiguration</span></span>

<span data-ttu-id="f0f31-193">Uygulama Oluşturucu yapılandırması çağırılarak oluşturulur [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="f0f31-193">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="f0f31-194">`ConfigureAppConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) uygulama için.</span><span class="sxs-lookup"><span data-stu-id="f0f31-194">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="f0f31-195">`ConfigureAppConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-195">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="f0f31-196">Uygulama, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-196">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="f0f31-197">Tarafından oluşturulan yapılandırmayı `ConfigureAppConfiguration` kullanılabilir [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) sonraki işlemleri ve buna [Hizmetleri](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="f0f31-197">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="f0f31-198">Örnek uygulama yapılandırmasını kullanma `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="f0f31-198">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="f0f31-199">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="f0f31-199">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="f0f31-200">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="f0f31-200">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="f0f31-201">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="f0f31-201">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="f0f31-202">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="f0f31-202">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="f0f31-203">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="f0f31-203">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="f0f31-204">`AddConfiguration` Yöntemi bekliyor yapılandırma anahtarları için tam bir eşleşme (örneğin, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="f0f31-204">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="f0f31-205">Bölüm adı anahtarlar varlığını, uygulama yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="f0f31-205">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="f0f31-206">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-206">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="f0f31-207">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="f0f31-207">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="f0f31-208">Ayarları dosyalar çıkış dizinine taşımak için ayarları dosyaları olarak belirtin. [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="f0f31-208">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="f0f31-209">Örnek uygulamasını kendi JSON uygulama ayarları dosyaları taşır ve *hostsettings.json* aşağıdaki **&lt;içerik:&gt;** öğesi:</span><span class="sxs-lookup"><span data-stu-id="f0f31-209">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="f0f31-210">Createservicereplicalisteners()</span><span class="sxs-lookup"><span data-stu-id="f0f31-210">ConfigureServices</span></span>

<span data-ttu-id="f0f31-211">[Createservicereplicalisteners()](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) Hizmetleri uygulamanın ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="f0f31-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f0f31-212">`ConfigureServices` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-212">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="f0f31-213">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır [Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="f0f31-213">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="f0f31-214">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="f0f31-214">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="f0f31-215">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="f0f31-215">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="f0f31-216">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="f0f31-216">ConfigureLogging</span></span>

<span data-ttu-id="f0f31-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) sağlanan yapılandırmak için bir temsilci ekler [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="f0f31-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="f0f31-218">`ConfigureLogging` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-218">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="f0f31-219">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="f0f31-219">UseConsoleLifetime</span></span>

<span data-ttu-id="f0f31-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) dinler `Ctrl+C`/SIGINT veya SIGTERM ve çağrıları [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) kapatma işlemi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="f0f31-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="f0f31-221">`UseConsoleLifetime` Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="f0f31-221">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="f0f31-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) varsayılan yaşam süresi uygulaması önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="f0f31-223">Kaydedilen son ömrü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-223">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="f0f31-224">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="f0f31-224">Container configuration</span></span>

<span data-ttu-id="f0f31-225">Diğer kapsayıcılarda takma desteklemek için konak alabilen bir [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="f0f31-225">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="f0f31-226">Fabrika sağlama DI kapsayıcı kaydı bir parçası değildir, ancak bunun yerine somut DI kapsayıcıyı oluşturmak için kullanılan bir iç yöneticisidir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-226">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="f0f31-227">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="f0f31-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="f0f31-228">Özel kapsayıcı yapılandırması tarafından yönetilir [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f0f31-228">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="f0f31-229">`ConfigureContainer` kapsayıcısının üstünde API altta yatan ana bilgisayar yapılandırma türü kesin belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="f0f31-229">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="f0f31-230">`ConfigureContainer` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-230">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="f0f31-231">App service kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f0f31-231">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="f0f31-232">Bir hizmet kapsayıcı üreteci sağlar:</span><span class="sxs-lookup"><span data-stu-id="f0f31-232">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="f0f31-233">Fabrika kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="f0f31-233">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="f0f31-234">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="f0f31-234">Extensibility</span></span>

<span data-ttu-id="f0f31-235">Konak genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0f31-235">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="f0f31-236">Aşağıdaki örnek, nasıl bir genişletme yönteminin genişlettiği gösterir. bir `IHostBuilder` uygulamasıyla [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) gösterilen örnek içinde <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="f0f31-236">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="f0f31-237">Bir uygulama oluşturur `UseHostedService` genişletme yöntemi, barındırılan hizmeti kaydetmek için geçirilen `T`:</span><span class="sxs-lookup"><span data-stu-id="f0f31-237">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="f0f31-238">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="f0f31-238">Manage the host</span></span>

<span data-ttu-id="f0f31-239">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) uygulamasıdır ve durdurmaktan sorumludur `IHostedService` service kapsayıcısında kayıtlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="f0f31-239">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="f0f31-240">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="f0f31-240">Run</span></span>

<span data-ttu-id="f0f31-241">[Çalıştırma](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="f0f31-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="f0f31-242">RunAsync</span><span class="sxs-lookup"><span data-stu-id="f0f31-242">RunAsync</span></span>

<span data-ttu-id="f0f31-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uygulama çalışır ve döndüren bir `Task` iptal belirteci veya kapatma tetiklendiğinde tamamlar:</span><span class="sxs-lookup"><span data-stu-id="f0f31-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="f0f31-244">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="f0f31-244">RunConsoleAsync</span></span>

<span data-ttu-id="f0f31-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve bekler `Ctrl+C`/SIGINT veya SIGTERM kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="f0f31-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="f0f31-246">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="f0f31-246">Start and StopAsync</span></span>

<span data-ttu-id="f0f31-247">[Başlangıç](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) konak zaman uyumlu olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="f0f31-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="f0f31-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) sağlanan zaman aşımı süresi içinde konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="f0f31-249">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="f0f31-249">StartAsync and StopAsync</span></span>

<span data-ttu-id="f0f31-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="f0f31-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="f0f31-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) uygulamayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="f0f31-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="f0f31-252">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="f0f31-252">WaitForShutdown</span></span>

<span data-ttu-id="f0f31-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) aracılığıyla tetiklenen [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), gibi [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (dinler `Ctrl+C`/SIGINT veya SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="f0f31-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="f0f31-254">`WaitForShutdown` çağrıları [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="f0f31-254">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="f0f31-255">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="f0f31-255">WaitForShutdownAsync</span></span>

<span data-ttu-id="f0f31-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) döndürür bir `Task` kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="f0f31-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="f0f31-257">Dış denetimi</span><span class="sxs-lookup"><span data-stu-id="f0f31-257">External control</span></span>

<span data-ttu-id="f0f31-258">Dış denetim konağının harici olarak çağrılabilir yöntemleri kullanılarak elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="f0f31-258">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="f0f31-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) başlangıcında çağırılır [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), devam etmeden önce işlemi tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="f0f31-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="f0f31-260">Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0f31-260">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="f0f31-261">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="f0f31-261">IHostingEnvironment interface</span></span>

<span data-ttu-id="f0f31-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) barındırma ortamı uygulama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="f0f31-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="f0f31-263">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="f0f31-263">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="f0f31-264">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="f0f31-264">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="f0f31-265">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="f0f31-265">IApplicationLifetime interface</span></span>

<span data-ttu-id="f0f31-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) kapatılmasını istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="f0f31-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="f0f31-267">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="f0f31-267">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="f0f31-268">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="f0f31-268">Cancellation Token</span></span> | <span data-ttu-id="f0f31-269">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="f0f31-269">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="f0f31-270">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="f0f31-270">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="f0f31-271">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="f0f31-271">The host has fully started.</span></span> |
| [<span data-ttu-id="f0f31-272">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="f0f31-272">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="f0f31-273">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="f0f31-273">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="f0f31-274">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="f0f31-274">All requests should be processed.</span></span> <span data-ttu-id="f0f31-275">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="f0f31-275">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="f0f31-276">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="f0f31-276">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="f0f31-277">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="f0f31-277">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="f0f31-278">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="f0f31-278">Requests may still be processing.</span></span> <span data-ttu-id="f0f31-279">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="f0f31-279">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="f0f31-280">Oluşturucu Ekle `IApplicationLifetime` herhangi bir sınıf içinde hizmet.</span><span class="sxs-lookup"><span data-stu-id="f0f31-280">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="f0f31-281">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir `IHostedService` uygulama) olaylarını kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="f0f31-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="f0f31-282">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0f31-282">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="f0f31-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister.</span><span class="sxs-lookup"><span data-stu-id="f0f31-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="f0f31-284">Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="f0f31-284">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="f0f31-285">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f0f31-285">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="f0f31-286">GitHub örnekleri deposu barındırma</span><span class="sxs-lookup"><span data-stu-id="f0f31-286">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
