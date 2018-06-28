---
title: .NET genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür boyu yönetimi için sorumlu olduğu .NET genel ana bilgisayar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 40d297257895a4defeb89cef9c5ec6deea64a985
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033361"
---
# <a name="net-generic-host"></a><span data-ttu-id="d6c5f-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="d6c5f-103">.NET Generic Host</span></span>

<span data-ttu-id="d6c5f-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d6c5f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d6c5f-105">.NET uygulamaları yapılandırmak ve başlatma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="d6c5f-106">Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d6c5f-107">Bu konu, ASP.NET Core genel ana bilgisayar içerir ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), HTTP isteklerini işleyen olmayan uygulamaları barındırmak için yararlı olan.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="d6c5f-108">Web barındırma kapsamını için ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), bkz: [Web ana bilgisayarı](xref:fundamentals/host/web-host) konu.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="d6c5f-109">Genel ana bilgisayar daha geniş bir dizi ana senaryoları etkinleştirmek için Web ana bilgisayar API HTTP ardışık düzen aynı şekilde hedefidir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="d6c5f-110">Mesajlaşma, arka plan görevleri ve yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özelliklerden genel ana avantajı göre diğer HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="d6c5f-111">Genel ana bilgisayar ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryolarında için uygun değil.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="d6c5f-112">Barındırma senaryolarında Web'deki kullanan [Web ana bilgisayarı](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="d6c5f-113">Web ana bilgisayarı gelecek bir sürümde değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil ana makinesi API olarak davranmak geliştirilme genel bir ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="d6c5f-114">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6c5f-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d6c5f-115">Örnek uygulama çalışırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="d6c5f-116">Örneği çalıştırma bir `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="d6c5f-117">Visual Studio kodda konsol ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="d6c5f-118">Açık *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="d6c5f-119">İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="d6c5f-120">Ya da değeri `externalTerminal` veya `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="d6c5f-121">Giriş</span><span class="sxs-lookup"><span data-stu-id="d6c5f-121">Introduction</span></span>

<span data-ttu-id="d6c5f-122">Genel ana bilgisayar kitaplığı kullanılabilir [Microsoft.Extensions.Hosting ad alanı](/dotnet/api/microsoft.extensions.hosting) ve tarafından sağlanan [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="d6c5f-123">`Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya sonrası).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="d6c5f-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) kod yürütülmesine giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="d6c5f-125">Her `IHostedService` uygulama sırasına göre yürütüldüğünde [hizmet ConfigureServices kaydında](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="d6c5f-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) her adlı `IHostedService` ana bilgisayar başlatıldığında ve [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) ters kayıt sırayla konak düzgün biçimde kapatıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="d6c5f-127">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="d6c5f-127">Set up a host</span></span>

<span data-ttu-id="d6c5f-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) kitaplıkları ve uygulamaları başlatmak, yapı ve ana bilgisayar çalıştırmak için kullandığınız ana bileşeni:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="d6c5f-129">Ana Bilgisayar Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d6c5f-129">Host configuration</span></span>

<span data-ttu-id="d6c5f-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d6c5f-131">Yapılandırma oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="d6c5f-131">Configuration builder</span></span>
* <span data-ttu-id="d6c5f-132">Uzantı yöntemi yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d6c5f-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="d6c5f-133">Yapılandırma oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="d6c5f-133">Configuration builder</span></span>

<span data-ttu-id="d6c5f-134">Ana bilgisayar Oluşturucu yapılandırması çağırarak oluşturulur [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="d6c5f-135">`ConfigureHostConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) ana bilgisayar için.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="d6c5f-136">Yapılandırma Oluşturucu başlatır [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) uygulamanın derleme işleminde kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="d6c5f-137">Ortam değişkeni yapılandırma, varsayılan olarak eklenmez.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="d6c5f-138">Çağrı [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) ortam değişkenleri konaktan yapılandırmak için konak oluşturucu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="d6c5f-139">`AddEnvironmentVariables` İsteğe bağlı kullanıcı tanımlı önek kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="d6c5f-140">Bir önek örnek uygulamanın kullandığı `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="d6c5f-141">Ortam değişkenleri okurken öneki kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="d6c5f-142">Örnek uygulamanın ana zaman yapılandırılmışsa, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="d6c5f-143">Kullanırken geliştirme sırasında [Visual Studio](https://www.visualstudio.com/) veya bir uygulamayla çalıştıran `dotnet run`, ortam değişkenleri kümesinde *Properties/launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="d6c5f-144">İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri kümesinde *.vscode/launch.json* geliştirme sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="d6c5f-145">Daha fazla bilgi için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-145">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="d6c5f-146">`ConfigureHostConfiguration` toplama sonuçları ile birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="d6c5f-147">Ana bilgisayar son hangi seçeneği bir değer ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="d6c5f-148">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="d6c5f-149">Örnek `HostBuilder` Yapılandırması'nı kullanarak `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="d6c5f-150">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d6c5f-151">`GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="d6c5f-152">`AddConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `HostBuilder` anahtarları (örneğin, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="d6c5f-153">Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d6c5f-154">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d6c5f-155">Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="d6c5f-156">Uzantı yöntemi yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d6c5f-156">Extension method configuration</span></span>

<span data-ttu-id="d6c5f-157">Uzantı yöntemleri çağrılmadan üzerinde `IHostBuilder` içerik kök ve ortamı yapılandırmak için uygulama.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="d6c5f-158">Kök içerik</span><span class="sxs-lookup"><span data-stu-id="d6c5f-158">Content Root</span></span>

<span data-ttu-id="d6c5f-159">Bu ayar, ana bilgisayar için içerik dosyaları arama başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-159">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="d6c5f-160">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d6c5f-160">**Key**: contentRoot</span></span>  
<span data-ttu-id="d6c5f-161">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d6c5f-161">**Type**: *string*</span></span>  
<span data-ttu-id="d6c5f-162">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-162">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d6c5f-163">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d6c5f-163">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d6c5f-164">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olan [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="d6c5f-164">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="d6c5f-165">Yol yoksa, konağı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-165">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="d6c5f-166">Ortam</span><span class="sxs-lookup"><span data-stu-id="d6c5f-166">Environment</span></span>

<span data-ttu-id="d6c5f-167">Uygulamanın ayarlar [ortam](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-167">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="d6c5f-168">**Anahtar**: ortamı</span><span class="sxs-lookup"><span data-stu-id="d6c5f-168">**Key**: environment</span></span>  
<span data-ttu-id="d6c5f-169">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d6c5f-169">**Type**: *string*</span></span>  
<span data-ttu-id="d6c5f-170">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="d6c5f-170">**Default**: Production</span></span>  
<span data-ttu-id="d6c5f-171">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d6c5f-171">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d6c5f-172">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olan [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="d6c5f-172">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="d6c5f-173">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-173">The environment can be set to any value.</span></span> <span data-ttu-id="d6c5f-174">Framework tanımlı değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-174">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d6c5f-175">Değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-175">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="d6c5f-176">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="d6c5f-176">ConfigureAppConfiguration</span></span>

<span data-ttu-id="d6c5f-177">Uygulama Oluşturucu yapılandırması çağırarak oluşturulur [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-177">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="d6c5f-178">`ConfigureAppConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-178">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="d6c5f-179">`ConfigureAppConfiguration` toplama sonuçları ile birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-179">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="d6c5f-180">Uygulama, en son hangi seçeneği bir değer ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-180">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="d6c5f-181">Tarafından oluşturulan yapılandırma `ConfigureAppConfiguration` şu adresten edinilebilir [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) sonraki işlemleri hem de [Hizmetleri](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-181">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="d6c5f-182">Örnek Uygulama Yapılandırması kullanılarak `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-182">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="d6c5f-183">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-183">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="d6c5f-184">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-184">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="d6c5f-185">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-185">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="d6c5f-186">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-186">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d6c5f-187">`GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-187">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="d6c5f-188">`AddConfiguration` Yöntemi bekliyor yapılandırma anahtarları için tam bir eşleşme (örneğin, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-188">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="d6c5f-189">Bölüm adı anahtarlar varlığını uygulama yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-189">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="d6c5f-190">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-190">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d6c5f-191">Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-191">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="d6c5f-192">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="d6c5f-192">ConfigureServices</span></span>

<span data-ttu-id="d6c5f-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) Hizmetleri uygulamanın ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d6c5f-194">`ConfigureServices` toplama sonuçları ile birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-194">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="d6c5f-195">Barındırılan hizmet uygulayan arka plan görevi mantığı ile bir sınıftır [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-195">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="d6c5f-196">Daha fazla bilgi için bkz: [arka plan görevleri barındırılan hizmetleri ile](xref:fundamentals/host/hosted-services) konu.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-196">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="d6c5f-197">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanan `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zaman aşımına arka plan görevi `TimedHostedService`, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-197">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="d6c5f-198">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="d6c5f-198">ConfigureLogging</span></span>

<span data-ttu-id="d6c5f-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) sağlanan yapılandırmak için bir temsilci ekler [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="d6c5f-200">`ConfigureLogging` toplama sonuçları ile birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-200">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="d6c5f-201">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="d6c5f-201">UseConsoleLifetime</span></span>

<span data-ttu-id="d6c5f-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) dinler `Ctrl+C`/SIGINT veya SIGTERM ve çağrıları [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) kapatma işlemini başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="d6c5f-203">`UseConsoleLifetime` Uzantıları gibi engelini kaldırır [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-203">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="d6c5f-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) olarak varsayılan yaşam süresi uygulaması önceden kayıtlı değil.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="d6c5f-205">Kayıtlı son ömrü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-205">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="d6c5f-206">Kapsayıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6c5f-206">Container configuration</span></span>

<span data-ttu-id="d6c5f-207">Diğer kapsayıcılarında takma desteklemek için konak kabul edebilen bir [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-207">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="d6c5f-208">Bir Fabrika sağlama dı kapsayıcı kayıt parçası değildir, ancak bunun yerine somut dı kapsayıcı oluşturmak için kullanılan bir iç yöneticisidir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-208">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="d6c5f-209">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-209">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="d6c5f-210">Özel kapsayıcısı yapılandırmasını yönetilir [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-210">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="d6c5f-211">`ConfigureContainer` altta yatan ana bilgisayar API üstünde kapsayıcı yapılandırmak için kesin türü belirtilmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-211">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="d6c5f-212">`ConfigureContainer` toplama sonuçları ile birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-212">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="d6c5f-213">Uygulama için bir hizmet kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-213">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="d6c5f-214">Bir hizmet kapsayıcı üreteci sağlar:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-214">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="d6c5f-215">Fabrika kullanın ve uygulama için özel hizmet kapsayıcı yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-215">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="d6c5f-216">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="d6c5f-216">Extensibility</span></span>

<span data-ttu-id="d6c5f-217">Ana bilgisayar genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-217">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="d6c5f-218">Aşağıdaki örnek, bir genişletme yöntemi nasıl genişlettiğini gösterir bir `IHostBuilder` uygulamasıyla [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-218">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="d6c5f-219">Bir RabbitMQ genişletme yöntemi (başka bir uygulamadaki) kaydeder `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-219">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="d6c5f-220">Ana bilgisayar yönetmek</span><span class="sxs-lookup"><span data-stu-id="d6c5f-220">Manage the host</span></span>

<span data-ttu-id="d6c5f-221">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) uygulamasıdır başlatma ve durdurma sorumlu `IHostedService` hizmet kapsayıcısında kayıtlı uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-221">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="d6c5f-222">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="d6c5f-222">Run</span></span>

<span data-ttu-id="d6c5f-223">[Çalıştırma](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uygulama çalıştırır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-223">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="d6c5f-224">RunAsync</span><span class="sxs-lookup"><span data-stu-id="d6c5f-224">RunAsync</span></span>

<span data-ttu-id="d6c5f-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uygulama çalıştırır ve döndürür bir `Task` kapatma ya da iptal belirteci tetiklendiğinde tamamlar:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="d6c5f-226">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="d6c5f-226">RunConsoleAsync</span></span>

<span data-ttu-id="d6c5f-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) konsol desteğini etkinleştirir, oluşturur ve ana bilgisayarı başlatır ve bekler `Ctrl+C`/SIGINT veya SIGTERM kapatılamadı.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="d6c5f-228">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="d6c5f-228">Start and StopAsync</span></span>

<span data-ttu-id="d6c5f-229">[Başlat](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) konak zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-229">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="d6c5f-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) sağlanan zaman aşımı süresi içinde ana bilgisayar durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="d6c5f-231">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="d6c5f-231">StartAsync and StopAsync</span></span>

<span data-ttu-id="d6c5f-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="d6c5f-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) uygulama durdurur.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="d6c5f-234">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="d6c5f-234">WaitForShutdown</span></span>

<span data-ttu-id="d6c5f-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) aracılığıyla tetiklenen [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), gibi [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (dinler `Ctrl+C`/SIGINT veya SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="d6c5f-236">`WaitForShutdown` çağrıları [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-236">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="d6c5f-237">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="d6c5f-237">WaitForShutdownAsync</span></span>

<span data-ttu-id="d6c5f-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) döndüren bir `Task` kapatma verilen belirteç ve çağrıları aracılığıyla tetiklendiğinde tamamlanan [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="d6c5f-239">Dış denetimi</span><span class="sxs-lookup"><span data-stu-id="d6c5f-239">External control</span></span>

<span data-ttu-id="d6c5f-240">Ana bilgisayarın dış denetim dışarıdan çağrılabilir yöntemleri kullanılarak elde edilir:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-240">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="d6c5f-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) başlangıcında adlı [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), devam etmeden önce işlemi tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="d6c5f-242">Bu, bir dış olay işaret kadar başlatma gecikmesi için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-242">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d6c5f-243">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="d6c5f-243">IHostingEnvironment interface</span></span>

<span data-ttu-id="d6c5f-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) barındırma ortamı uygulama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="d6c5f-245">Kullanmak [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-245">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d6c5f-246">Daha fazla bilgi için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d6c5f-246">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d6c5f-247">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="d6c5f-247">IApplicationLifetime interface</span></span>

<span data-ttu-id="d6c5f-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) kapama istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="d6c5f-249">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-249">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d6c5f-250">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="d6c5f-250">Cancellation Token</span></span> | <span data-ttu-id="d6c5f-251">Ne zaman tetiklendi&#8230;</span><span class="sxs-lookup"><span data-stu-id="d6c5f-251">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="d6c5f-252">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d6c5f-252">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d6c5f-253">Ana bilgisayar tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-253">The host has fully started.</span></span> |
| [<span data-ttu-id="d6c5f-254">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="d6c5f-254">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d6c5f-255">Konak bir kapama üzeredir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-255">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d6c5f-256">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-256">All requests should be processed.</span></span> <span data-ttu-id="d6c5f-257">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-257">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d6c5f-258">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="d6c5f-258">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d6c5f-259">Konak bir kapama gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-259">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d6c5f-260">İstekleri hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-260">Requests may still be processing.</span></span> <span data-ttu-id="d6c5f-261">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-261">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="d6c5f-262">Oluşturucu Ekle `IApplicationLifetime` herhangi bir sınıfın içine hizmet.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-262">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="d6c5f-263">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) içine Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir `IHostedService` uygulaması) olayları kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-263">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="d6c5f-264">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-264">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="d6c5f-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) uygulama sonlandırılması ister.</span><span class="sxs-lookup"><span data-stu-id="d6c5f-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d6c5f-266">Aşağıdaki sınıf kullanır `StopApplication` düzgün biçimde bir uygulamasını kapatmak için zaman sınıfının `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d6c5f-266">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="d6c5f-267">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d6c5f-267">Additional resources</span></span>

* [<span data-ttu-id="d6c5f-268">Barındırılan hizmetler ile arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="d6c5f-268">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="d6c5f-269">GitHub depo örnekleri barındırma</span><span class="sxs-lookup"><span data-stu-id="d6c5f-269">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
