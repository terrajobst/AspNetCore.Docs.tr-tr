---
title: Windows hizmetinde konak ASP.NET Core
author: guardrex
description: ASP.NET Core uygulamasının bir Windows hizmetinde nasıl barındıralınacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/09/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544037a2a1f836e51b4f10551316312ef55c68da
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688082"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="a28df-103">Windows hizmetinde konak ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a28df-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="a28df-104">[Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="a28df-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a28df-105">Bir ASP.NET Core uygulaması, IIS kullanmadan Windows [hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) olarak Windows üzerinde barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a28df-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="a28df-106">Windows hizmeti olarak barındırıldığı zaman, uygulama otomatik olarak sunucu yeniden başlatıldıktan sonra başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a28df-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="a28df-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a28df-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a28df-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a28df-108">Prerequisites</span></span>

* [<span data-ttu-id="a28df-109">ASP.NET Core SDK 2,1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a28df-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="a28df-110">PowerShell 6,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a28df-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="a28df-111">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="a28df-111">Worker Service template</span></span>

<span data-ttu-id="a28df-112">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="a28df-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="a28df-113">Şablonu bir Windows Hizmeti uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="a28df-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="a28df-114">.NET Core şablonundan bir çalışan hizmeti uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a28df-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="a28df-115">Çalışan hizmeti uygulamasını bir Windows hizmeti olarak çalışacak şekilde güncelleştirmek için [uygulama yapılandırma](#app-configuration) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="a28df-116">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a28df-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a28df-117">`IHostBuilder.UseWindowsService`, ana bilgisayar oluşturulurken [Microsoft. Extensions. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) paketi tarafından verilen çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a28df-117">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="a28df-118">Uygulama bir Windows hizmeti olarak çalışıyorsa, yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a28df-118">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="a28df-119">Ana bilgisayar ömrünü olarak `WindowsServiceLifetime`ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a28df-119">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="a28df-120">İçerik kökünü ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a28df-120">Sets the content root.</span></span>
* <span data-ttu-id="a28df-121">Varsayılan kaynak adı olarak uygulama adı ile olay günlüğüne günlük kaydını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a28df-121">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="a28df-122">Günlük düzeyi appSettings içindeki `Logging:LogLevel:Default` anahtar kullanılarak yapılandırılabilir *. Production. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="a28df-122">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="a28df-123">Yeni olay kaynakları yalnızca yöneticiler tarafından oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="a28df-123">Only administrators can create new event sources.</span></span> <span data-ttu-id="a28df-124">Uygulama adı kullanılarak bir olay kaynağı oluşturuoluşturumadığında, *uygulama* kaynağına bir uyarı kaydedilir ve olay günlükleri devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="a28df-124">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a28df-125">Uygulama [Microsoft. AspNetCore. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)için paket başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a28df-125">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="a28df-126">Bir hizmetin dışında çalışırken test ve hata ayıklamak için, uygulamanın bir hizmet olarak mı yoksa bir konsol uygulaması mi çalıştığını belirleme kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-126">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="a28df-127">Hata ayıklayıcının ekli olduğunu veya bir `--console` anahtarın mevcut olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-127">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="a28df-128">Her iki koşul de geçerliyse (uygulama bir hizmet olarak çalıştırılmadıysa), çağırın <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="a28df-128">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="a28df-129">Koşullar yanlışsa (uygulama bir hizmet olarak çalıştırılır):</span><span class="sxs-lookup"><span data-stu-id="a28df-129">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="a28df-130">Uygulamanın <xref:System.IO.Directory.SetCurrentDirectory*> yayımlanmış konumunun yolunu çağırın ve kullanın.</span><span class="sxs-lookup"><span data-stu-id="a28df-130">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="a28df-131">Bir Windows <xref:System.IO.Directory.GetCurrentDirectory*> hizmeti uygulaması çağrıldığında *C:\\Windows\\system32* klasörünü <xref:System.IO.Directory.GetCurrentDirectory*> döndürdüğünden yolu almak için çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="a28df-131">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="a28df-132">Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a28df-132">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="a28df-133">Bu adım, uygulamanın ' de `CreateWebHostBuilder`yapılandırılmadan önce gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a28df-133">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="a28df-134">Uygulamayı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> bir hizmet olarak çalıştırmak için çağırın.</span><span class="sxs-lookup"><span data-stu-id="a28df-134">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="a28df-135">[Komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden, `--console` bağımsız değişkenleri almadan önce <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> anahtar bağımsız değişkenlerden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="a28df-135">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="a28df-136">Windows olay günlüğü 'ne yazmak için EventLog sağlayıcısını öğesine <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-136">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="a28df-137">Günlük kaydı düzeyini `Logging:LogLevel:Default` appSettings içindeki anahtarla ayarlayın *. Production. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="a28df-137">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="a28df-138">Örnek uygulamadan aşağıdaki örnekte, `RunAsCustomService` uygulama içindeki ömür olaylarını işlemek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> yerine çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a28df-138">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="a28df-139">Daha fazla bilgi için [olayları başlatma ve durdurma olaylarını](#handle-starting-and-stopping-events) inceleyin bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a28df-139">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="a28df-140">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="a28df-140">Deployment type</span></span>

<span data-ttu-id="a28df-141">Dağıtım senaryoları hakkında bilgi ve öneriler için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="a28df-141">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="a28df-142">Çerçeveye bağımlı dağıtım (FDD)</span><span class="sxs-lookup"><span data-stu-id="a28df-142">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="a28df-143">Çerçeveye bağımlı dağıtım (FDD), hedef sistemde .NET Core 'un paylaşılan sistem genelindeki bir sürümünün varlığına dayanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-143">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="a28df-144">Bu makaledeki kılavuzdan sonra FDD senaryosu benimsendiği zaman SDK, *çerçeveye bağlı yürütülebilir dosya*olarak adlandırılan yürütülebilir bir dosya ( *. exe*) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a28df-144">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a28df-145">Aşağıdaki özellik öğelerini proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a28df-145">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="a28df-146">`<OutputType>`Uygulamanın çıkış türü (`Exe` yürütülebilir için). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a28df-146">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="a28df-147">`<LangVersion>`Dil sürümü(`latest` veya`preview`). &ndash; C#</span><span class="sxs-lookup"><span data-stu-id="a28df-147">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="a28df-148">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="a28df-148">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="a28df-149">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-149">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a28df-150">Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="a28df-150">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="a28df-151">Aşağıdaki örnekte, RID olarak `win7-x64`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-151">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="a28df-152">`<SelfContained>` Özelliği olarak`false`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-152">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="a28df-153">Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.</span><span class="sxs-lookup"><span data-stu-id="a28df-153">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="a28df-154">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="a28df-154">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="a28df-155">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-155">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="a28df-156">Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="a28df-156">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="a28df-157">Aşağıdaki örnekte, RID olarak `win7-x64`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-157">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="a28df-158">`<SelfContained>` Özelliği olarak`false`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-158">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="a28df-159">Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.</span><span class="sxs-lookup"><span data-stu-id="a28df-159">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="a28df-160">`<UseAppHost>` Özelliği olarak`true`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-160">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="a28df-161">Bu özellik, bir FDD için bir etkinleştirme yolu (yürütülebilir, *. exe*) ile hizmeti sağlar.</span><span class="sxs-lookup"><span data-stu-id="a28df-161">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="a28df-162">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="a28df-162">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="a28df-163">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-163">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="a28df-164">Kendi içinde dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="a28df-164">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="a28df-165">Kendinden bağımsız dağıtım (SCD), ana bilgisayar sisteminde paylaşılan bir Framework varlığına güvenmez.</span><span class="sxs-lookup"><span data-stu-id="a28df-165">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="a28df-166">Çalışma zamanı ve uygulamanın bağımlılıkları uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="a28df-166">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="a28df-167">Hedef Framework 'ü içeren ' de `<PropertyGroup>` bir Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) bulunur:</span><span class="sxs-lookup"><span data-stu-id="a28df-167">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="a28df-168">Birden çok RID için yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="a28df-168">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="a28df-169">RID 'leri, noktalı virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="a28df-169">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="a28df-170">> (Plural) adlı [ \<runtimetanımlayıcılar](/dotnet/core/tools/csproj#runtimeidentifiers) özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a28df-170">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="a28df-171">Daha fazla bilgi için bkz. [.NET Core RID kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="a28df-171">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a28df-172">Bir `<SelfContained>` özellik şu şekilde `true`ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="a28df-172">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="a28df-173">Hizmet Kullanıcı hesabı</span><span class="sxs-lookup"><span data-stu-id="a28df-173">Service user account</span></span>

<span data-ttu-id="a28df-174">Bir hizmet için Kullanıcı hesabı oluşturmak için, bir yönetim PowerShell 6 komut kabuğundan [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a28df-174">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="a28df-175">Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/Build 10.0.17763) veya sonraki sürümler:</span><span class="sxs-lookup"><span data-stu-id="a28df-175">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="a28df-176">Windows 10 Ekim 2018 (sürüm 1809/Build 10.0.17763) sürümünden önceki Windows IŞLETIM sistemlerinde:</span><span class="sxs-lookup"><span data-stu-id="a28df-176">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="a28df-177">İstendiğinde [güçlü bir parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a28df-177">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="a28df-178">Parametresi New [-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ine bir süre sonu <xref:System.DateTime>ile sağlanmamışsa hesabın süresi dolmaz. `-AccountExpires`</span><span class="sxs-lookup"><span data-stu-id="a28df-178">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="a28df-179">Daha fazla bilgi için bkz. [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmet Kullanıcı hesapları](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="a28df-179">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="a28df-180">Active Directory kullanırken kullanıcıları yönetmeye yönelik alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a28df-180">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="a28df-181">Daha fazla bilgi için bkz. [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="a28df-181">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="a28df-182">Hizmet hakları olarak oturum açma</span><span class="sxs-lookup"><span data-stu-id="a28df-182">Log on as a service rights</span></span>

<span data-ttu-id="a28df-183">Hizmet Kullanıcı hesabı için *hizmet hakları olarak oturum* açma oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="a28df-183">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="a28df-184">Yerel Güvenlik Ilkesi düzenleyicisini, *secpol. msc*' i çalıştırarak açın.</span><span class="sxs-lookup"><span data-stu-id="a28df-184">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="a28df-185">**Yerel ilkeler** düğümünü genişletin ve **Kullanıcı hakları ataması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-185">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="a28df-186">**Hizmet olarak oturum** açma ilkesi açın.</span><span class="sxs-lookup"><span data-stu-id="a28df-186">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="a28df-187">**Kullanıcı veya Grup Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-187">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="a28df-188">Aşağıdaki yaklaşımlardan birini kullanarak nesne adını (Kullanıcı hesabı) sağlayın:</span><span class="sxs-lookup"><span data-stu-id="a28df-188">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="a28df-189">Kullanıcı hesabını (`{DOMAIN OR COMPUTER NAME\USER}`) nesne adı alanına yazın ve kullanıcıyı ilkeye eklemek için **Tamam** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-189">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="a28df-190">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-190">Select **Advanced**.</span></span> <span data-ttu-id="a28df-191">**Şimdi bul**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-191">Select **Find Now**.</span></span> <span data-ttu-id="a28df-192">Listeden Kullanıcı hesabını seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-192">Select the user account from the list.</span></span> <span data-ttu-id="a28df-193">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-193">Select **OK**.</span></span> <span data-ttu-id="a28df-194">Kullanıcıyı ilkeye eklemek için yeniden **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-194">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="a28df-195">Değişiklikleri kabul etmek için **Tamam ' ı** veya **Uygula** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="a28df-195">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="a28df-196">Windows hizmetini oluşturma ve yönetme</span><span class="sxs-lookup"><span data-stu-id="a28df-196">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="a28df-197">Bir hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="a28df-197">Create a service</span></span>

<span data-ttu-id="a28df-198">Bir hizmeti kaydetmek için PowerShell komutlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a28df-198">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="a28df-199">Bir yönetim PowerShell 6 komut kabuğundan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="a28df-199">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="a28df-200">`{EXE PATH}`Konaktaki uygulamanın klasörünün yolu (örneğin, `d:\myservice`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a28df-200">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="a28df-201">Uygulamanın yürütülebilir dosyasını yola eklemeyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-201">Don't include the app's executable in the path.</span></span> <span data-ttu-id="a28df-202">Sondaki eğik çizgi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a28df-202">A trailing slash isn't required.</span></span>
* <span data-ttu-id="a28df-203">`{DOMAIN OR COMPUTER NAME\USER}`Hizmet Kullanıcı hesabı (örneğin, `Contoso\ServiceUser`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a28df-203">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="a28df-204">`{NAME}`Hizmet adı (örneğin, `MyService`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a28df-204">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="a28df-205">`{EXE FILE PATH}`Uygulamanın yürütülebilir yolu (örneğin, `d:\myservice\myservice.exe`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a28df-205">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="a28df-206">Yürütülebilir dosyanın dosya adını uzantısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a28df-206">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="a28df-207">`{DESCRIPTION}`Hizmet açıklaması (örneğin, `My sample service`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a28df-207">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="a28df-208">`{DISPLAY NAME}`Hizmet görünen adı (örneğin, `My Service`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a28df-208">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="a28df-209">Hizmet başlatma</span><span class="sxs-lookup"><span data-stu-id="a28df-209">Start a service</span></span>

<span data-ttu-id="a28df-210">Aşağıdaki PowerShell 6 komutuyla bir hizmet başlatın:</span><span class="sxs-lookup"><span data-stu-id="a28df-210">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="a28df-211">Komutun başlatılması birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="a28df-211">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="a28df-212">Hizmetin durumunu belirleme</span><span class="sxs-lookup"><span data-stu-id="a28df-212">Determine a service's status</span></span>

<span data-ttu-id="a28df-213">Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="a28df-213">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="a28df-214">Durum, aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="a28df-214">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="a28df-215">Bir hizmeti durdur</span><span class="sxs-lookup"><span data-stu-id="a28df-215">Stop a service</span></span>

<span data-ttu-id="a28df-216">Aşağıdaki PowerShell 6 komutuyla bir hizmeti durdurun:</span><span class="sxs-lookup"><span data-stu-id="a28df-216">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="a28df-217">Hizmeti Kaldır</span><span class="sxs-lookup"><span data-stu-id="a28df-217">Remove a service</span></span>

<span data-ttu-id="a28df-218">Bir hizmeti durdurmak için kısa bir gecikmeden sonra, aşağıdaki PowerShell 6 komutuyla bir hizmeti kaldırın:</span><span class="sxs-lookup"><span data-stu-id="a28df-218">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="a28df-219">Olayları başlatma ve durdurma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="a28df-219">Handle starting and stopping events</span></span>

<span data-ttu-id="a28df-220">, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*> <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>Ve olaylarınıişlemekiçin:<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*></span><span class="sxs-lookup"><span data-stu-id="a28df-220">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="a28df-221">`OnStarting`, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> Ve`OnStarted`yöntemleriyle türetilen bir sınıfoluşturun:`OnStopping`</span><span class="sxs-lookup"><span data-stu-id="a28df-221">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="a28df-222">' A <xref:Microsoft.AspNetCore.Hosting.IWebHost> `CustomWebHostService` geçişi içinbirgenişletmeyöntemioluşturun:<xref:System.ServiceProcess.ServiceBase.Run*></span><span class="sxs-lookup"><span data-stu-id="a28df-222">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="a28df-223">İçinde `Program.Main`, `RunAsCustomService` yerine<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>genişletme yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="a28df-223">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="a28df-224"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> Konumunu`Program.Main`görmek için, [dağıtım türü](#deployment-type) bölümünde gösterilen kod örneğine bakın.</span><span class="sxs-lookup"><span data-stu-id="a28df-224">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="a28df-225">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="a28df-225">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="a28df-226">Internet 'ten veya şirket ağından gelen isteklerle etkileşime geçen ve bir ara sunucu veya yük dengeleyicinin arkasındaki Hizmetler ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="a28df-226">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="a28df-227">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="a28df-227">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="a28df-228">Uç noktaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a28df-228">Configure endpoints</span></span>

<span data-ttu-id="a28df-229">Varsayılan olarak, ASP.NET Core öğesine `http://localhost:5000`bağlanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-229">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a28df-230">`ASPNETCORE_URLS` Ortam değişkenini ayarlayarak URL 'yi ve bağlantı noktasını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a28df-230">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="a28df-231">HTTPS uç noktaları için destek de dahil olmak üzere ek URL ve bağlantı noktası yapılandırma yaklaşımları için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="a28df-231">For additional URL and port configuration approaches, including support for HTTPS endpoints, see the following topics:</span></span>

* <span data-ttu-id="a28df-232"><xref:fundamentals/servers/kestrel#endpoint-configuration>Kestrel</span><span class="sxs-lookup"><span data-stu-id="a28df-232"><xref:fundamentals/servers/kestrel#endpoint-configuration> (Kestrel)</span></span>
* <span data-ttu-id="a28df-233"><xref:fundamentals/servers/httpsys#configure-windows-server>(HTTP. sys)</span><span class="sxs-lookup"><span data-stu-id="a28df-233"><xref:fundamentals/servers/httpsys#configure-windows-server> (HTTP.sys)</span></span>

> [!NOTE]
> <span data-ttu-id="a28df-234">Hizmet uç noktasının güvenliğini sağlamak için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a28df-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="a28df-235">Geçerli dizin ve içerik kökü</span><span class="sxs-lookup"><span data-stu-id="a28df-235">Current directory and content root</span></span>

<span data-ttu-id="a28df-236">Windows hizmeti için çağırarak <xref:System.IO.Directory.GetCurrentDirectory*> döndürülen geçerli çalışma dizini *C:\\Windows\\system32* klasörüdür.</span><span class="sxs-lookup"><span data-stu-id="a28df-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="a28df-237">*System32* klasörü, bir hizmetin dosyalarını (örneğin, ayarlar dosyaları) depolamak için uygun bir konum değildir.</span><span class="sxs-lookup"><span data-stu-id="a28df-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="a28df-238">Bir hizmetin varlık ve ayar dosyalarını sürdürmek ve erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a28df-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="a28df-239">ContentRootPath veya ContentRootFileProvider kullanın</span><span class="sxs-lookup"><span data-stu-id="a28df-239">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="a28df-240">[Ihostenvironment. contentrootpath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) kullanın veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> bir uygulamanın kaynaklarını bulun.</span><span class="sxs-lookup"><span data-stu-id="a28df-240">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="a28df-241">Uygulamanın klasörü için içerik kök yolunu ayarla</span><span class="sxs-lookup"><span data-stu-id="a28df-241">Set the content root path to the app's folder</span></span>

<span data-ttu-id="a28df-242">, <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Bir hizmet oluşturulduğunda `binPath` bağımsız değişkene aynı yol olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a28df-242">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="a28df-243">Ayarlar dosyalarına yollar `GetCurrentDirectory` oluşturmak için çağırmak yerine, uygulamanın içerik kökünün <xref:System.IO.Directory.SetCurrentDirectory*> yolunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="a28df-243">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="a28df-244">İçinde `Program.Main`, hizmetin yürütülebilir dosyasının yolunu belirleyin ve uygulamanın içerik kökünü oluşturmak için yolu kullanın:</span><span class="sxs-lookup"><span data-stu-id="a28df-244">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="a28df-245">Hizmetin dosyalarını diskte uygun bir konumda depolayın</span><span class="sxs-lookup"><span data-stu-id="a28df-245">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="a28df-246">Dosyaları içeren klasöre <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanırken ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> mutlak bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="a28df-246">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a28df-247">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a28df-247">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="a28df-248">[Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırması ve SNı desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="a28df-248">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="a28df-249">[Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırması ve SNı desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="a28df-249">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
