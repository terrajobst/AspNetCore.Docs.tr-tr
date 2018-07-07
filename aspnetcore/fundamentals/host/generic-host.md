---
title: .NET genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu .NET genel ana bilgisayar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: ce2a540cc7a63f61075c9c01759f67531171e1e1
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889161"
---
# <a name="net-generic-host"></a><span data-ttu-id="3344e-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="3344e-103">.NET Generic Host</span></span>

<span data-ttu-id="3344e-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3344e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3344e-105">.NET uygulamaları ve başlatma yapılandırma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="3344e-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="3344e-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="3344e-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3344e-107">Bu konu, ASP.NET Core genel ana bilgisayar kapsar ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), HTTP isteklerini mıdl'ye işleme uygulamaları barındırmak için kullanışlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="3344e-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="3344e-108">İçin Web ana bilgisayarı kapsamını ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), bkz: [Web ana bilgisayarı](xref:fundamentals/host/web-host) konu.</span><span class="sxs-lookup"><span data-stu-id="3344e-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="3344e-109">Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır.</span><span class="sxs-lookup"><span data-stu-id="3344e-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="3344e-110">Mesajlaşma, arka plan görevleri ve diğer yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri genel ana bilgisayar avantajından temel HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="3344e-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="3344e-111">Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="3344e-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="3344e-112">Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="3344e-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="3344e-113">Gelecek sürümlerden birinde Web ana bilgisayarı değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API olarak davranmak için geliştirme aşamasındaki genel ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="3344e-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="3344e-114">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3344e-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3344e-115">Örnek uygulamayı çalıştırırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*.</span><span class="sxs-lookup"><span data-stu-id="3344e-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="3344e-116">Örneği çalıştırma bir `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="3344e-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="3344e-117">Visual Studio Code'da konsol ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="3344e-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="3344e-118">Açık *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="3344e-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="3344e-119">İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi.</span><span class="sxs-lookup"><span data-stu-id="3344e-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="3344e-120">Değer aşağıdaki seçeneklerden birine ayarlayın `externalTerminal` veya `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="3344e-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="3344e-121">Giriş</span><span class="sxs-lookup"><span data-stu-id="3344e-121">Introduction</span></span>

<span data-ttu-id="3344e-122">Genel Host kitaplığı kullanılabilir [Microsoft.Extensions.Hosting ad alanı](/dotnet/api/microsoft.extensions.hosting) ve tarafından sağlanan [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket.</span><span class="sxs-lookup"><span data-stu-id="3344e-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="3344e-123">`Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="3344e-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="3344e-124">[Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) kod yürütme için giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="3344e-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="3344e-125">Her `IHostedService` uygulama sırasına göre yürütülür [hizmet Createservicereplicalisteners() kaydında](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="3344e-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="3344e-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) her çağrılır `IHostedService` konak başlatıldığında ve [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) kayıt ters sırada konağın düzgün biçimde kapatıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3344e-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="3344e-127">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="3344e-127">Set up a host</span></span>

<span data-ttu-id="3344e-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) kitaplıkları ve uygulamaları, başlatmak, derleme ve konak çalıştırmak için kullandığınız ana bileşeni:</span><span class="sxs-lookup"><span data-stu-id="3344e-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="3344e-129">Ana Bilgisayar Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3344e-129">Host configuration</span></span>

<span data-ttu-id="3344e-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="3344e-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="3344e-131">Yapılandırma oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="3344e-131">Configuration builder</span></span>
* <span data-ttu-id="3344e-132">Uzantı yöntemi yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3344e-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="3344e-133">Yapılandırma oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="3344e-133">Configuration builder</span></span>

<span data-ttu-id="3344e-134">Ana bilgisayar Oluşturucu yapılandırması çağırılarak oluşturulur [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="3344e-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="3344e-135">`ConfigureHostConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) konak için.</span><span class="sxs-lookup"><span data-stu-id="3344e-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="3344e-136">Yapılandırma oluşturucuyu başlatır [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) kullanılmak üzere uygulamanın derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="3344e-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="3344e-137">Ortam değişkeni yapılandırma varsayılan olarak eklenmez.</span><span class="sxs-lookup"><span data-stu-id="3344e-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="3344e-138">Çağrı [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) ortam değişkenlerini konaktan yapılandırmak için konak oluşturucu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="3344e-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="3344e-139">`AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder.</span><span class="sxs-lookup"><span data-stu-id="3344e-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="3344e-140">Bir önek örnek uygulamanın kullandığı `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="3344e-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="3344e-141">Ön ek ortam değişkenlerini okunduğunda kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="3344e-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="3344e-142">Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="3344e-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="3344e-143">Kullanırken, geliştirme sırasında [Visual Studio](https://www.visualstudio.com/) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="3344e-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="3344e-144">İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="3344e-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="3344e-145">Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3344e-145">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="3344e-146">`ConfigureHostConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="3344e-147">Konak, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="3344e-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="3344e-148">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="3344e-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="3344e-149">Örnek `HostBuilder` yapılandırmayla `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="3344e-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="3344e-150">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="3344e-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3344e-151">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="3344e-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="3344e-152">`AddConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `HostBuilder` anahtarları (örneğin, `environment`).</span><span class="sxs-lookup"><span data-stu-id="3344e-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="3344e-153">Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="3344e-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="3344e-154">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="3344e-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3344e-155">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="3344e-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="3344e-156">Uzantı yöntemi yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3344e-156">Extension method configuration</span></span>

<span data-ttu-id="3344e-157">Uzantı yöntemleri üzerinde çağrıldığında `IHostBuilder` içerik kök ve ortamı yapılandırmak için uygulama.</span><span class="sxs-lookup"><span data-stu-id="3344e-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="3344e-158">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="3344e-158">Content Root</span></span>

<span data-ttu-id="3344e-159">Bu ayar, içerik dosyalarını aramaya konak başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="3344e-159">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="3344e-160">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="3344e-160">**Key**: contentRoot</span></span>  
<span data-ttu-id="3344e-161">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="3344e-161">**Type**: *string*</span></span>  
<span data-ttu-id="3344e-162">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="3344e-162">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="3344e-163">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="3344e-163">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="3344e-164">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="3344e-164">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="3344e-165">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="3344e-165">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="3344e-166">Ortam</span><span class="sxs-lookup"><span data-stu-id="3344e-166">Environment</span></span>

<span data-ttu-id="3344e-167">Uygulamanın ayarlar [ortam](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3344e-167">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="3344e-168">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="3344e-168">**Key**: environment</span></span>  
<span data-ttu-id="3344e-169">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="3344e-169">**Type**: *string*</span></span>  
<span data-ttu-id="3344e-170">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="3344e-170">**Default**: Production</span></span>  
<span data-ttu-id="3344e-171">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="3344e-171">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="3344e-172">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="3344e-172">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="3344e-173">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-173">The environment can be set to any value.</span></span> <span data-ttu-id="3344e-174">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="3344e-174">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="3344e-175">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="3344e-175">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="3344e-176">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="3344e-176">ConfigureAppConfiguration</span></span>

<span data-ttu-id="3344e-177">Uygulama Oluşturucu yapılandırması çağırılarak oluşturulur [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="3344e-177">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="3344e-178">`ConfigureAppConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) uygulama için.</span><span class="sxs-lookup"><span data-stu-id="3344e-178">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="3344e-179">`ConfigureAppConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-179">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="3344e-180">Uygulama, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="3344e-180">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="3344e-181">Tarafından oluşturulan yapılandırmayı `ConfigureAppConfiguration` kullanılabilir [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) sonraki işlemleri ve buna [Hizmetleri](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="3344e-181">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="3344e-182">Örnek uygulama yapılandırmasını kullanma `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="3344e-182">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="3344e-183">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="3344e-183">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="3344e-184">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="3344e-184">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="3344e-185">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="3344e-185">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="3344e-186">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="3344e-186">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3344e-187">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="3344e-187">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="3344e-188">`AddConfiguration` Yöntemi bekliyor yapılandırma anahtarları için tam bir eşleşme (örneğin, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="3344e-188">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="3344e-189">Bölüm adı anahtarlar varlığını, uygulama yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="3344e-189">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="3344e-190">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="3344e-190">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3344e-191">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="3344e-191">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="3344e-192">Ayarları dosyalar çıkış dizinine taşımak için ayarları dosyaları olarak belirtin. [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="3344e-192">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="3344e-193">Örnek uygulamasını kendi JSON uygulama ayarları dosyaları taşır ve *hostsettings.json* aşağıdaki **&lt;içerik:&gt;** öğesi:</span><span class="sxs-lookup"><span data-stu-id="3344e-193">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="3344e-194">Createservicereplicalisteners()</span><span class="sxs-lookup"><span data-stu-id="3344e-194">ConfigureServices</span></span>

<span data-ttu-id="3344e-195">[Createservicereplicalisteners()](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) Hizmetleri uygulamanın ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="3344e-195">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3344e-196">`ConfigureServices` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-196">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="3344e-197">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır [Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="3344e-197">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="3344e-198">Daha fazla bilgi için [arka plan görevleri barındırılan hizmetler ile](xref:fundamentals/host/hosted-services) konu.</span><span class="sxs-lookup"><span data-stu-id="3344e-198">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="3344e-199">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="3344e-199">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="3344e-200">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="3344e-200">ConfigureLogging</span></span>

<span data-ttu-id="3344e-201">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) sağlanan yapılandırmak için bir temsilci ekler [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="3344e-201">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="3344e-202">`ConfigureLogging` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-202">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="3344e-203">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="3344e-203">UseConsoleLifetime</span></span>

<span data-ttu-id="3344e-204">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) dinler `Ctrl+C`/SIGINT veya SIGTERM ve çağrıları [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) kapatma işlemi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="3344e-204">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="3344e-205">`UseConsoleLifetime` Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="3344e-205">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="3344e-206">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) varsayılan yaşam süresi uygulaması önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-206">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="3344e-207">Kaydedilen son ömrü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3344e-207">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="3344e-208">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3344e-208">Container configuration</span></span>

<span data-ttu-id="3344e-209">Diğer kapsayıcılarda takma desteklemek için konak alabilen bir [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="3344e-209">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="3344e-210">Fabrika sağlama DI kapsayıcı kaydı bir parçası değildir, ancak bunun yerine somut DI kapsayıcıyı oluşturmak için kullanılan bir iç yöneticisidir.</span><span class="sxs-lookup"><span data-stu-id="3344e-210">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="3344e-211">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="3344e-211">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="3344e-212">Özel kapsayıcı yapılandırması tarafından yönetilir [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3344e-212">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="3344e-213">`ConfigureContainer` kapsayıcısının üstünde API altta yatan ana bilgisayar yapılandırma türü kesin belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="3344e-213">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="3344e-214">`ConfigureContainer` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-214">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="3344e-215">App service kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="3344e-215">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="3344e-216">Bir hizmet kapsayıcı üreteci sağlar:</span><span class="sxs-lookup"><span data-stu-id="3344e-216">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="3344e-217">Fabrika kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="3344e-217">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="3344e-218">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="3344e-218">Extensibility</span></span>

<span data-ttu-id="3344e-219">Konak genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3344e-219">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="3344e-220">Aşağıdaki örnek, nasıl bir genişletme yönteminin genişlettiği gösterir. bir `IHostBuilder` uygulamasıyla [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="3344e-220">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="3344e-221">Genişletme yöntemi (uygulamada başka bir yerde) bir RabbitMQ kaydeder `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="3344e-221">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="3344e-222">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="3344e-222">Manage the host</span></span>

<span data-ttu-id="3344e-223">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) uygulamasıdır ve durdurmaktan sorumludur `IHostedService` service kapsayıcısında kayıtlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="3344e-223">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="3344e-224">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="3344e-224">Run</span></span>

<span data-ttu-id="3344e-225">[Çalıştırma](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="3344e-225">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="3344e-226">RunAsync</span><span class="sxs-lookup"><span data-stu-id="3344e-226">RunAsync</span></span>

<span data-ttu-id="3344e-227">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uygulama çalışır ve döndüren bir `Task` iptal belirteci veya kapatma tetiklendiğinde tamamlar:</span><span class="sxs-lookup"><span data-stu-id="3344e-227">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="3344e-228">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="3344e-228">RunConsoleAsync</span></span>

<span data-ttu-id="3344e-229">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve bekler `Ctrl+C`/SIGINT veya SIGTERM kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="3344e-229">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="3344e-230">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="3344e-230">Start and StopAsync</span></span>

<span data-ttu-id="3344e-231">[Başlangıç](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) konak zaman uyumlu olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="3344e-231">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="3344e-232">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) sağlanan zaman aşımı süresi içinde konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="3344e-232">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="3344e-233">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="3344e-233">StartAsync and StopAsync</span></span>

<span data-ttu-id="3344e-234">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="3344e-234">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="3344e-235">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) uygulamayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="3344e-235">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="3344e-236">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="3344e-236">WaitForShutdown</span></span>

<span data-ttu-id="3344e-237">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) aracılığıyla tetiklenen [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), gibi [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (dinler `Ctrl+C`/SIGINT veya SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="3344e-237">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="3344e-238">`WaitForShutdown` çağrıları [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="3344e-238">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="3344e-239">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="3344e-239">WaitForShutdownAsync</span></span>

<span data-ttu-id="3344e-240">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) döndürür bir `Task` kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="3344e-240">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="3344e-241">Dış denetimi</span><span class="sxs-lookup"><span data-stu-id="3344e-241">External control</span></span>

<span data-ttu-id="3344e-242">Dış denetim konağının harici olarak çağrılabilir yöntemleri kullanılarak elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="3344e-242">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="3344e-243">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) başlangıcında çağırılır [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), devam etmeden önce işlemi tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="3344e-243">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="3344e-244">Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3344e-244">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="3344e-245">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="3344e-245">IHostingEnvironment interface</span></span>

<span data-ttu-id="3344e-246">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) barındırma ortamı uygulama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="3344e-246">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="3344e-247">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="3344e-247">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="3344e-248">Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3344e-248">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="3344e-249">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="3344e-249">IApplicationLifetime interface</span></span>

<span data-ttu-id="3344e-250">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) kapatılmasını istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="3344e-250">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="3344e-251">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="3344e-251">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="3344e-252">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="3344e-252">Cancellation Token</span></span> | <span data-ttu-id="3344e-253">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="3344e-253">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="3344e-254">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="3344e-254">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="3344e-255">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="3344e-255">The host has fully started.</span></span> |
| [<span data-ttu-id="3344e-256">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="3344e-256">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="3344e-257">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="3344e-257">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="3344e-258">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="3344e-258">All requests should be processed.</span></span> <span data-ttu-id="3344e-259">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="3344e-259">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="3344e-260">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="3344e-260">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="3344e-261">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="3344e-261">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="3344e-262">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="3344e-262">Requests may still be processing.</span></span> <span data-ttu-id="3344e-263">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="3344e-263">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="3344e-264">Oluşturucu Ekle `IApplicationLifetime` herhangi bir sınıf içinde hizmet.</span><span class="sxs-lookup"><span data-stu-id="3344e-264">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="3344e-265">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir `IHostedService` uygulama) olaylarını kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="3344e-265">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="3344e-266">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="3344e-266">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="3344e-267">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister.</span><span class="sxs-lookup"><span data-stu-id="3344e-267">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="3344e-268">Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="3344e-268">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3344e-269">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3344e-269">Additional resources</span></span>

* [<span data-ttu-id="3344e-270">Barındırılan hizmetler ile arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="3344e-270">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="3344e-271">GitHub örnekleri deposu barındırma</span><span class="sxs-lookup"><span data-stu-id="3344e-271">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
