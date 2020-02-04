---
title: Windows hizmetinde konak ASP.NET Core
author: guardrex
description: ASP.NET Core uygulamasının bir Windows hizmetinde nasıl barındıralınacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: d4b540de50f4153f517f871f037521347fb5eb84
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2020
ms.locfileid: "76971998"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="2c3e2-103">Windows hizmetinde konak ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c3e2-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="2c3e2-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2c3e2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2c3e2-105">Bir ASP.NET Core uygulaması, IIS kullanmadan Windows [hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) olarak Windows üzerinde barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="2c3e2-106">Windows hizmeti olarak barındırıldığı zaman, uygulama otomatik olarak sunucu yeniden başlatıldıktan sonra başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="2c3e2-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2c3e2-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c3e2-108">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="2c3e2-108">Prerequisites</span></span>

* [<span data-ttu-id="2c3e2-109">ASP.NET Core SDK 2,1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="2c3e2-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="2c3e2-110">PowerShell 6,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="2c3e2-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="2c3e2-111">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="2c3e2-111">Worker Service template</span></span>

<span data-ttu-id="2c3e2-112">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="2c3e2-113">Şablonu bir Windows Hizmeti uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="2c3e2-114">.NET Core şablonundan bir çalışan hizmeti uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="2c3e2-115">Çalışan hizmeti uygulamasını bir Windows hizmeti olarak çalışacak şekilde güncelleştirmek için [uygulama yapılandırma](#app-configuration) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="2c3e2-116">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="2c3e2-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2c3e2-117">Uygulama, [Microsoft. Extensions. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices)için bir paket başvurusu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="2c3e2-118">`IHostBuilder.UseWindowsService` ana bilgisayar oluşturulurken çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="2c3e2-119">Uygulama bir Windows hizmeti olarak çalışıyorsa, yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="2c3e2-120">Ana bilgisayar ömrünü `WindowsServiceLifetime`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="2c3e2-121">[İçerik kökünü](xref:fundamentals/index#content-root) [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory)olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-121">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="2c3e2-122">Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-122">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="2c3e2-123">Varsayılan kaynak adı olarak uygulama adı ile olay günlüğüne günlük kaydını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-123">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="2c3e2-124">Günlük düzeyi appSettings 'teki `Logging:LogLevel:Default` anahtarı kullanılarak yapılandırılabilir *. Production. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-124">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="2c3e2-125">Yeni olay kaynakları yalnızca yöneticiler tarafından oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-125">Only administrators can create new event sources.</span></span> <span data-ttu-id="2c3e2-126">Uygulama adı kullanılarak bir olay kaynağı oluşturuoluşturumadığında, *uygulama* kaynağına bir uyarı kaydedilir ve olay günlükleri devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-126">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="2c3e2-127">*Program.cs*`CreateHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-127">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="2c3e2-128">Aşağıdaki örnek uygulamalar bu konuya eşlik eder:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-128">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="2c3e2-129">Arka plan çalışan hizmeti örneği, arka plan görevleri için [barındırılan Hizmetleri](xref:fundamentals/host/hosted-services) kullanan [çalışan hizmeti şablonunu](#worker-service-template) temel alan, Web olmayan bir uygulama örneğini &ndash;.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-129">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="2c3e2-130">Web App Service örneği, arka plan görevleri için [barındırılan hizmetlerle](xref:fundamentals/host/hosted-services) Windows hizmeti olarak çalışan bir Razor pages Web uygulaması örneği &ndash;.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-130">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="2c3e2-131">MVC Kılavuzu için <xref:mvc/overview> ve <xref:migration/22-to-30>altındaki makalelere bakın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-131">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2c3e2-132">Uygulama [Microsoft. AspNetCore. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)için paket başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-132">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="2c3e2-133">Bir hizmetin dışında çalışırken test ve hata ayıklamak için, uygulamanın bir hizmet olarak mı yoksa bir konsol uygulaması mi çalıştığını belirleme kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-133">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="2c3e2-134">Hata ayıklayıcının ekli olduğunu veya `--console` bir anahtarın mevcut olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-134">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="2c3e2-135">Her iki koşul de geçerliyse (uygulama hizmet olarak çalıştırılmadıysa) <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>çağırın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-135">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="2c3e2-136">Koşullar yanlışsa (uygulama bir hizmet olarak çalıştırılır):</span><span class="sxs-lookup"><span data-stu-id="2c3e2-136">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="2c3e2-137"><xref:System.IO.Directory.SetCurrentDirectory*> çağırın ve uygulamanın yayımlanan konumunun yolunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-137">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="2c3e2-138">Bir Windows hizmeti uygulaması, <xref:System.IO.Directory.GetCurrentDirectory*> çağrıldığında *C:\\windows\\system32* klasörünü döndürdüğünden yolu almak için <xref:System.IO.Directory.GetCurrentDirectory*> çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-138">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="2c3e2-139">Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-139">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="2c3e2-140">Bu adım uygulama `CreateWebHostBuilder`' de yapılandırılmadan önce gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-140">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="2c3e2-141">Uygulamayı bir hizmet olarak çalıştırmak için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-141">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="2c3e2-142">Komut satırı [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bağımsız değişkenleri almadan önce `--console` anahtarı bağımsız değişkenlerden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-142">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="2c3e2-143">Windows olay günlüğü 'ne yazmak için, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>EventLog sağlayıcısını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-143">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="2c3e2-144">Günlük kaydı düzeyini appSettings 'teki `Logging:LogLevel:Default` anahtarıyla ayarlayın *. Production. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-144">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="2c3e2-145">Örnek uygulamadan aşağıdaki örnekte, uygulama içindeki ömür olaylarını işlemek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> yerine `RunAsCustomService` çağırılır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-145">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="2c3e2-146">Daha fazla bilgi için [olayları başlatma ve durdurma olaylarını](#handle-starting-and-stopping-events) inceleyin bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-146">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="2c3e2-147">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="2c3e2-147">Deployment type</span></span>

<span data-ttu-id="2c3e2-148">Dağıtım senaryoları hakkında bilgi ve öneriler için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-148">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="2c3e2-149">SDK</span><span class="sxs-lookup"><span data-stu-id="2c3e2-149">SDK</span></span>

<span data-ttu-id="2c3e2-150">Razor Pages veya MVC çerçevelerini kullanan bir Web uygulaması tabanlı hizmet için, proje dosyasında Web SDK 'sını belirtin:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-150">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="2c3e2-151">Hizmet yalnızca arka plan görevlerini (örneğin, [barındırılan hizmetler](xref:fundamentals/host/hosted-services)) yürütülüyorsa, proje dosyasında çalışan SDK 'sını belirtin:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-151">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="2c3e2-152">Çerçeveye bağımlı dağıtım (FDD)</span><span class="sxs-lookup"><span data-stu-id="2c3e2-152">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="2c3e2-153">Çerçeveye bağımlı dağıtım (FDD), hedef sistemde .NET Core 'un paylaşılan sistem genelindeki bir sürümünün varlığına dayanır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-153">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="2c3e2-154">Bu makaledeki kılavuzdan sonra FDD senaryosu benimsendiği zaman SDK, *çerçeveye bağlı yürütülebilir dosya*olarak adlandırılan yürütülebilir bir dosya ( *. exe*) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-154">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2c3e2-155">[Web SDK](#sdk)kullanıyorsanız, normalde bir ASP.NET Core uygulaması yayımlarken üretilen bir *Web. config* dosyası Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-155">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="2c3e2-156">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliği `true`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-156">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="2c3e2-157">Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-157">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="2c3e2-158">Aşağıdaki örnekte, RID `win7-x64`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-158">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="2c3e2-159">`<SelfContained>` özelliği `false`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-159">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="2c3e2-160">Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-160">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="2c3e2-161">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="2c3e2-162">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliği `true`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="2c3e2-163">Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="2c3e2-164">Aşağıdaki örnekte, RID `win7-x64`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="2c3e2-165">`<SelfContained>` özelliği `false`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="2c3e2-166">Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="2c3e2-167">`<UseAppHost>` özelliği `true`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-167">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="2c3e2-168">Bu özellik, bir FDD için bir etkinleştirme yolu (yürütülebilir, *. exe*) ile hizmeti sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-168">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="2c3e2-169">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-169">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="2c3e2-170">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliği `true`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-170">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="2c3e2-171">Kendi içinde dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="2c3e2-171">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="2c3e2-172">Kendinden bağımsız dağıtım (SCD), ana bilgisayar sisteminde paylaşılan bir Framework varlığına güvenmez.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-172">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="2c3e2-173">Çalışma zamanı ve uygulamanın bağımlılıkları uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-173">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="2c3e2-174">Hedef Framework 'ü içeren `<PropertyGroup>` bir Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) bulunur:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-174">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="2c3e2-175">Birden çok RID için yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-175">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="2c3e2-176">RID 'leri, noktalı virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-176">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="2c3e2-177">[\<Runtimetanýmlayýcýtanımlayıcıları >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural) için özellik adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-177">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="2c3e2-178">Daha fazla bilgi için bkz. [.NET Core RID kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-178">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2c3e2-179">`<SelfContained>` Özellik `true`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-179">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="2c3e2-180">Hizmet Kullanıcı hesabı</span><span class="sxs-lookup"><span data-stu-id="2c3e2-180">Service user account</span></span>

<span data-ttu-id="2c3e2-181">Bir hizmet için Kullanıcı hesabı oluşturmak için, bir yönetim PowerShell 6 komut kabuğundan [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ini kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-181">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="2c3e2-182">Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/Build 10.0.17763) veya sonraki sürümler:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-182">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="2c3e2-183">Windows 10 Ekim 2018 (sürüm 1809/Build 10.0.17763) sürümünden önceki Windows IŞLETIM sistemlerinde:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-183">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="2c3e2-184">İstendiğinde [güçlü bir parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) sağlayın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-184">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="2c3e2-185">`-AccountExpires` parametresi [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ine bir süre sonu <xref:System.DateTime>sağlanmamışsa hesabın süresi dolmaz.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-185">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="2c3e2-186">Daha fazla bilgi için bkz. [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmet Kullanıcı hesapları](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-186">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="2c3e2-187">Active Directory kullanırken kullanıcıları yönetmeye yönelik alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-187">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="2c3e2-188">Daha fazla bilgi için bkz. [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-188">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="2c3e2-189">Hizmet hakları olarak oturum açma</span><span class="sxs-lookup"><span data-stu-id="2c3e2-189">Log on as a service rights</span></span>

<span data-ttu-id="2c3e2-190">Hizmet Kullanıcı hesabı için *hizmet hakları olarak oturum* açma oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-190">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="2c3e2-191">Yerel Güvenlik Ilkesi düzenleyicisini, *secpol. msc*' i çalıştırarak açın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-191">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="2c3e2-192">**Yerel ilkeler** düğümünü genişletin ve **Kullanıcı hakları ataması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-192">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="2c3e2-193">**Hizmet olarak oturum** açma ilkesi açın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-193">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="2c3e2-194">**Kullanıcı veya Grup Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-194">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="2c3e2-195">Aşağıdaki yaklaşımlardan birini kullanarak nesne adını (Kullanıcı hesabı) sağlayın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-195">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="2c3e2-196">Kullanıcı hesabını (`{DOMAIN OR COMPUTER NAME\USER}`) nesne adı alanına yazın ve kullanıcıyı ilkeye eklemek için **Tamam** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-196">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="2c3e2-197">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-197">Select **Advanced**.</span></span> <span data-ttu-id="2c3e2-198">**Şimdi bul**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-198">Select **Find Now**.</span></span> <span data-ttu-id="2c3e2-199">Listeden Kullanıcı hesabını seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-199">Select the user account from the list.</span></span> <span data-ttu-id="2c3e2-200">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-200">Select **OK**.</span></span> <span data-ttu-id="2c3e2-201">Kullanıcıyı ilkeye eklemek için yeniden **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-201">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="2c3e2-202">Değişiklikleri kabul etmek için **Tamam ' ı** veya **Uygula** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-202">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="2c3e2-203">Windows hizmetini oluşturma ve yönetme</span><span class="sxs-lookup"><span data-stu-id="2c3e2-203">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="2c3e2-204">Bir hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c3e2-204">Create a service</span></span>

<span data-ttu-id="2c3e2-205">Bir hizmeti kaydetmek için PowerShell komutlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-205">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="2c3e2-206">Bir yönetim PowerShell 6 komut kabuğundan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-206">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="2c3e2-207">Ana bilgisayardaki uygulamanın klasörünün yolunu &ndash; `{EXE PATH}` (örneğin, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-207">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="2c3e2-208">Uygulamanın yürütülebilir dosyasını yola eklemeyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-208">Don't include the app's executable in the path.</span></span> <span data-ttu-id="2c3e2-209">Sondaki eğik çizgi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-209">A trailing slash isn't required.</span></span>
* <span data-ttu-id="2c3e2-210">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; hizmet Kullanıcı hesabı (örneğin, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-210">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="2c3e2-211">`{SERVICE NAME}` &ndash; hizmet adı (örneğin, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-211">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="2c3e2-212">uygulamanın yürütülebilir yolunu &ndash; `{EXE FILE PATH}` (örneğin, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-212">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="2c3e2-213">Yürütülebilir dosyanın dosya adını uzantısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-213">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="2c3e2-214">`{DESCRIPTION}` &ndash; hizmet açıklaması (örneğin, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-214">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="2c3e2-215">`{DISPLAY NAME}` &ndash; hizmet görünen adı (örneğin, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-215">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="2c3e2-216">Hizmet başlatma</span><span class="sxs-lookup"><span data-stu-id="2c3e2-216">Start a service</span></span>

<span data-ttu-id="2c3e2-217">Aşağıdaki PowerShell 6 komutuyla bir hizmet başlatın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-217">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="2c3e2-218">Komutun başlatılması birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-218">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="2c3e2-219">Hizmetin durumunu belirleme</span><span class="sxs-lookup"><span data-stu-id="2c3e2-219">Determine a service's status</span></span>

<span data-ttu-id="2c3e2-220">Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-220">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="2c3e2-221">Durum, aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-221">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="2c3e2-222">Bir hizmeti durdur</span><span class="sxs-lookup"><span data-stu-id="2c3e2-222">Stop a service</span></span>

<span data-ttu-id="2c3e2-223">Aşağıdaki PowerShell 6 komutuyla bir hizmeti durdurun:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-223">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="2c3e2-224">Hizmeti Kaldır</span><span class="sxs-lookup"><span data-stu-id="2c3e2-224">Remove a service</span></span>

<span data-ttu-id="2c3e2-225">Bir hizmeti durdurmak için kısa bir gecikmeden sonra, aşağıdaki PowerShell 6 komutuyla bir hizmeti kaldırın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-225">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="2c3e2-226">Olayları başlatma ve durdurma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="2c3e2-226">Handle starting and stopping events</span></span>

<span data-ttu-id="2c3e2-227"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olaylarını işlemek için:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-227">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="2c3e2-228">`OnStarting`, `OnStarted`ve `OnStopping` yöntemlerle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> türetilen bir sınıf oluşturun:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-228">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="2c3e2-229"><xref:System.ServiceProcess.ServiceBase.Run*>`CustomWebHostService` ileten <xref:Microsoft.AspNetCore.Hosting.IWebHost> için bir genişletme yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-229">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="2c3e2-230">`Program.Main`' de, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>yerine `RunAsCustomService` uzantısı metodunu çağırın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-230">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="2c3e2-231">`Program.Main`<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> konumunu görmek için, [dağıtım türü](#deployment-type) bölümünde gösterilen kod örneğine bakın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-231">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="2c3e2-232">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="2c3e2-232">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="2c3e2-233">Internet 'ten veya şirket ağından gelen isteklerle etkileşime geçen ve bir ara sunucu veya yük dengeleyicinin arkasındaki Hizmetler ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-233">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="2c3e2-234">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-234">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="2c3e2-235">Uç noktaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c3e2-235">Configure endpoints</span></span>

<span data-ttu-id="2c3e2-236">Varsayılan olarak, ASP.NET Core `http://localhost:5000`bağlar.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-236">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="2c3e2-237">`ASPNETCORE_URLS` ortam değişkenini ayarlayarak URL 'YI ve bağlantı noktasını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-237">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="2c3e2-238">Ek URL ve bağlantı noktası yapılandırma yaklaşımları için ilgili sunucu makalesine bakın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-238">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="2c3e2-239">Yukarıdaki kılavuz, HTTPS uç noktaları için desteği içerir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-239">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="2c3e2-240">Örneğin, bir Windows hizmeti ile kimlik doğrulaması kullanıldığında HTTPS için uygulamayı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-240">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="2c3e2-241">Hizmet uç noktasının güvenliğini sağlamak için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-241">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="2c3e2-242">Geçerli dizin ve içerik kökü</span><span class="sxs-lookup"><span data-stu-id="2c3e2-242">Current directory and content root</span></span>

<span data-ttu-id="2c3e2-243">Bir Windows hizmeti için <xref:System.IO.Directory.GetCurrentDirectory*> çağırarak döndürülen geçerli çalışma dizini *C:\\windows\\system32* klasörüdür.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-243">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="2c3e2-244">*System32* klasörü, bir hizmetin dosyalarını (örneğin, ayarlar dosyaları) depolamak için uygun bir konum değildir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-244">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="2c3e2-245">Bir hizmetin varlık ve ayar dosyalarını sürdürmek ve erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-245">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="2c3e2-246">ContentRootPath veya ContentRootFileProvider kullanın</span><span class="sxs-lookup"><span data-stu-id="2c3e2-246">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="2c3e2-247">Bir uygulamanın kaynaklarını bulmak için [ıhostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-247">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="2c3e2-248">Uygulama bir hizmet olarak çalıştığında, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory)olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-248">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="2c3e2-249">Uygulamanın varsayılan ayar dosyaları, *appSettings. JSON* ve *appSettings. { '. JSON ortamı*, [konak oluşturma sırasında Createdefaultbuilder](xref:fundamentals/host/generic-host#set-up-a-host)' ı çağırarak uygulamanın içerik kökünden yüklenir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-249">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="2c3e2-250"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>geliştirici kodu tarafından yüklenen diğer ayar dosyaları için, <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>çağırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-250">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2c3e2-251">Aşağıdaki örnekte, *custom_settings. JSON* dosyası uygulamanın içerik kökünde bulunur ve açıkça bir temel yolu ayarlamadan yüklenir:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-251">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="2c3e2-252">Bir Windows hizmeti uygulaması geçerli dizini olarak *C:\\Windows\\system32* klasörünü döndürdüğünden, kaynak yolunu almak için <xref:System.IO.Directory.GetCurrentDirectory*> kullanmayı denemeyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-252">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="2c3e2-253">Uygulamanın klasörü için içerik kök yolunu ayarla</span><span class="sxs-lookup"><span data-stu-id="2c3e2-253">Set the content root path to the app's folder</span></span>

<span data-ttu-id="2c3e2-254"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*>, bir hizmet oluşturulduğunda `binPath` bağımsız değişkenine sunulan yoldur.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-254">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="2c3e2-255">Ayar dosyalarına yollar oluşturmak için `GetCurrentDirectory` çağırmak yerine, uygulamanın [içerik kökünün](xref:fundamentals/index#content-root)yolunu kullanarak <xref:System.IO.Directory.SetCurrentDirectory*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-255">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="2c3e2-256">`Program.Main`, hizmetin yürütülebilir dosyasının yolunu belirleyin ve uygulamanın içerik kökünü oluşturmak için yolu kullanın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-256">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="2c3e2-257">Hizmetin dosyalarını diskte uygun bir konumda depolayın</span><span class="sxs-lookup"><span data-stu-id="2c3e2-257">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="2c3e2-258">Dosyaları içeren klasörde <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanırken <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> mutlak bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-258">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="2c3e2-259">Sorunları Gider</span><span class="sxs-lookup"><span data-stu-id="2c3e2-259">Troubleshoot</span></span>

<span data-ttu-id="2c3e2-260">Windows hizmet uygulamasının sorunlarını gidermek için bkz. <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-260">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="2c3e2-261">Sık karşılaşılan hatalar</span><span class="sxs-lookup"><span data-stu-id="2c3e2-261">Common errors</span></span>

* <span data-ttu-id="2c3e2-262">PowerShell 'in eski veya yayın öncesi sürümü kullanımda.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-262">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="2c3e2-263">Kayıtlı hizmet, [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutundan uygulamanın **yayımlanmış** çıktısını kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-263">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="2c3e2-264">[DotNet Build](/dotnet/core/tools/dotnet-build) komutunun çıkışı, uygulama dağıtımı için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-264">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="2c3e2-265">Yayımlanan varlıklar, dağıtım türüne bağlı olarak aşağıdaki klasörlerden birinde bulunur:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-265">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="2c3e2-266">*bin/Release/{Target Framework}/Publish* (FDD)</span><span class="sxs-lookup"><span data-stu-id="2c3e2-266">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="2c3e2-267">*bin/Release/{Target Framework}/{RUNTIME tanımlayıcısı}/Publish* (SCD)</span><span class="sxs-lookup"><span data-stu-id="2c3e2-267">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="2c3e2-268">Hizmet çalışır durumda değil.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-268">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="2c3e2-269">Uygulamanın kullandığı kaynakların yolları (örneğin, sertifikalar) yanlış.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-269">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="2c3e2-270">Windows hizmetinin temel yolu *c:\\windows\\system32*' dir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-270">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="2c3e2-271">Kullanıcı, hizmet hakları *olarak oturum* açma hakkına sahip değil.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-271">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="2c3e2-272">`New-Service` PowerShell komutu çalıştırılırken kullanıcının parolasının kullanım dışı veya yanlış şekilde geçirilmesi.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-272">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="2c3e2-273">Uygulama ASP.NET Core kimlik doğrulaması gerektiriyor, ancak güvenli bağlantılar (HTTPS) için yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-273">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="2c3e2-274">İstek URL 'SI bağlantı noktası yanlış veya uygulamada doğru şekilde yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-274">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="2c3e2-275">Sistem ve uygulama olay günlükleri</span><span class="sxs-lookup"><span data-stu-id="2c3e2-275">System and Application Event Logs</span></span>

<span data-ttu-id="2c3e2-276">Sistem ve uygulama olay günlüklerine erişin:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-276">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="2c3e2-277">Başlat menüsünü açın, *Olay Görüntüleyicisi*araması yapın ve **Olay Görüntüleyicisi** uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-277">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="2c3e2-278">İçinde **Olay Görüntüleyicisi'ni**açın **Windows Günlükleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-278">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="2c3e2-279">Sistem olay günlüğünü açmak için **sistem** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-279">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="2c3e2-280">Seçin **uygulama** uygulama olay günlüğünü açın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-280">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="2c3e2-281">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-281">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="2c3e2-282">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="2c3e2-282">Run the app at a command prompt</span></span>

<span data-ttu-id="2c3e2-283">Birçok başlatma hatası olay günlüklerinde yararlı bilgiler oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-283">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="2c3e2-284">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-284">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="2c3e2-285">Uygulamadan ek ayrıntı günlüğe kaydetmek için, [günlük düzeyini](xref:fundamentals/logging/index#log-level) düşürün veya [geliştirme ortamında](xref:fundamentals/environments)uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-285">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="2c3e2-286">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="2c3e2-286">Clear package caches</span></span>

<span data-ttu-id="2c3e2-287">Çalışan bir uygulama, geliştirme makinesindeki .NET Core SDK yükseltmeden veya uygulama içindeki paket sürümlerini değiştirirken hemen başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-287">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="2c3e2-288">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-288">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="2c3e2-289">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-289">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="2c3e2-290">Silme *bin* ve *obj* klasörleri.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-290">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="2c3e2-291">Bir komut kabuğundan [DotNet NuGet yerelleri, Tümünü Temizle](/dotnet/core/tools/dotnet-nuget-locals) ' i yürüterek paket önbelleklerini temizleyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-291">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="2c3e2-292">Paket önbelleklerini Temizleme, [NuGet. exe](https://www.nuget.org/downloads) aracı ile de gerçekleştirilebilir ve komut `nuget locals all -clear`yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-292">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="2c3e2-293">*nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen bir yükleme değildir ve gelen ayrı olarak edinilmelidir [NuGet Web sitesi](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-293">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="2c3e2-294">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-294">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="2c3e2-295">Uygulamayı yeniden dağıtmadan önce sunucusundaki dağıtım klasöründeki tüm dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-295">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="2c3e2-296">Yavaş veya asılı uygulama</span><span class="sxs-lookup"><span data-stu-id="2c3e2-296">Slow or hanging app</span></span>

<span data-ttu-id="2c3e2-297">*Kilitlenme dökümü* , sistem belleğinin bir anlık görüntüsüdür ve uygulama kilitlenmesinin, başlatma hatasının veya yavaş uygulamanın nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-297">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="2c3e2-298">Uygulama kilitleniyor veya bir özel durumla karşılaşırsa</span><span class="sxs-lookup"><span data-stu-id="2c3e2-298">App crashes or encounters an exception</span></span>

<span data-ttu-id="2c3e2-299">Windows Hata Bildirimi bir döküm edinin ve çözümleyin [(WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="2c3e2-299">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="2c3e2-300">Kilitlenme döküm dosyalarını `c:\dumps`tutmak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-300">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="2c3e2-301">[Enabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) uygulama yürütülebilir adıyla çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-301">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="2c3e2-302">Uygulamayı kilitlenmenin oluşmasına neden olan koşullar altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-302">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="2c3e2-303">Kilitlenme gerçekleştirildikten sonra, [Disabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2c3e2-303">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="2c3e2-304">Uygulama kilitlenmeleri ve döküm koleksiyonu tamamlandıktan sonra, uygulamanın normal olarak sonlandırılmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-304">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="2c3e2-305">PowerShell betiği, WER 'i uygulama başına en fazla beş döküm toplayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-305">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="2c3e2-306">Kilitlenme dökümleri büyük miktarda disk alanı kaplar (her birine kadar çok gigabayt kadar).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-306">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="2c3e2-307">Uygulama askıda kalıyor, başlatma sırasında başarısız oluyor veya normal şekilde çalışıyor</span><span class="sxs-lookup"><span data-stu-id="2c3e2-307">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="2c3e2-308">Bir uygulama *askıda* kaldığında (yanıt vermeyi keser ancak kilitlenmez), başlatma sırasında başarısız olur veya normal şekilde çalışır. [Kullanıcı modu döküm dosyaları:](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçmek üzere en iyi aracı seçme.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-308">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="2c3e2-309">Dökümü çözümle</span><span class="sxs-lookup"><span data-stu-id="2c3e2-309">Analyze the dump</span></span>

<span data-ttu-id="2c3e2-310">Bir döküm çeşitli yaklaşımlar kullanılarak analiz edilebilir.</span><span class="sxs-lookup"><span data-stu-id="2c3e2-310">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="2c3e2-311">Daha fazla bilgi için bkz. [Kullanıcı modu döküm dosyasını çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="2c3e2-311">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2c3e2-312">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2c3e2-312">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="2c3e2-313">[Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (https yapılandırması ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="2c3e2-313">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="2c3e2-314">[Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (https yapılandırması ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="2c3e2-314">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
