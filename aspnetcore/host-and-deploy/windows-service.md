---
title: Windows hizmetinde konak ASP.NET Core
author: guardrex
description: ASP.NET Core uygulamasının bir Windows hizmetinde nasıl barındıralınacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/09/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 995fdd2bbba30ff983bc2055fcb97c14541e2ac6
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081477"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="ec41b-103">Windows hizmetinde konak ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec41b-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="ec41b-104">[Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="ec41b-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ec41b-105">Bir ASP.NET Core uygulaması, IIS kullanmadan Windows [hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) olarak Windows üzerinde barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="ec41b-106">Windows hizmeti olarak barındırıldığı zaman, uygulama otomatik olarak sunucu yeniden başlatıldıktan sonra başlatılır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="ec41b-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ec41b-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec41b-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ec41b-108">Prerequisites</span></span>

* [<span data-ttu-id="ec41b-109">ASP.NET Core SDK 2,1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="ec41b-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="ec41b-110">PowerShell 6,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="ec41b-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="ec41b-111">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="ec41b-111">Worker Service template</span></span>

<span data-ttu-id="ec41b-112">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec41b-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="ec41b-113">Şablonu bir Windows Hizmeti uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="ec41b-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="ec41b-114">.NET Core şablonundan bir çalışan hizmeti uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ec41b-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="ec41b-115">Çalışan hizmeti uygulamasını bir Windows hizmeti olarak çalışacak şekilde güncelleştirmek için [uygulama yapılandırma](#app-configuration) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec41b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec41b-116">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ec41b-117">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ec41b-117">Create a new project.</span></span>
1. <span data-ttu-id="ec41b-118">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="ec41b-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ec41b-119">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-119">Select **Next**.</span></span>
1. <span data-ttu-id="ec41b-120">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-120">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="ec41b-121">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-121">Select **Create**.</span></span>
1. <span data-ttu-id="ec41b-122">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-122">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="ec41b-123">**Çalışan hizmeti** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-123">Select the **Worker Service** template.</span></span> <span data-ttu-id="ec41b-124">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-124">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ec41b-125">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ec41b-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ec41b-126">Çalışan hizmeti (`worker`) şablonunu bir komut kabuğundan [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla birlikte kullanın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-126">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="ec41b-127">Aşağıdaki örnekte, adlı `ContosoWorkerService`bir çalışan hizmeti uygulaması oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ec41b-127">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="ec41b-128">Komut yürütüldüğünde, `ContosoWorkerService` uygulamanın bir klasörü otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ec41b-128">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="ec41b-129">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ec41b-129">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ec41b-130">`IHostBuilder.UseWindowsService`, ana bilgisayar oluşturulurken [Microsoft. Extensions. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) paketi tarafından verilen çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-130">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="ec41b-131">Uygulama bir Windows hizmeti olarak çalışıyorsa, yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ec41b-131">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="ec41b-132">Ana bilgisayar ömrünü olarak `WindowsServiceLifetime`ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ec41b-132">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="ec41b-133">İçerik kökünü ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ec41b-133">Sets the content root.</span></span>
* <span data-ttu-id="ec41b-134">Varsayılan kaynak adı olarak uygulama adı ile olay günlüğüne günlük kaydını sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec41b-134">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="ec41b-135">Günlük düzeyi appSettings içindeki `Logging:LogLevel:Default` anahtar kullanılarak yapılandırılabilir *. Production. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="ec41b-135">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="ec41b-136">Yeni olay kaynakları yalnızca yöneticiler tarafından oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-136">Only administrators can create new event sources.</span></span> <span data-ttu-id="ec41b-137">Uygulama adı kullanılarak bir olay kaynağı oluşturuoluşturumadığında, *uygulama* kaynağına bir uyarı kaydedilir ve olay günlükleri devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-137">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ec41b-138">Uygulama [Microsoft. AspNetCore. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)için paket başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-138">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="ec41b-139">Bir hizmetin dışında çalışırken test ve hata ayıklamak için, uygulamanın bir hizmet olarak mı yoksa bir konsol uygulaması mi çalıştığını belirleme kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-139">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="ec41b-140">Hata ayıklayıcının ekli olduğunu veya bir `--console` anahtarın mevcut olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-140">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="ec41b-141">Her iki koşul de geçerliyse (uygulama bir hizmet olarak çalıştırılmadıysa), çağırın <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="ec41b-141">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="ec41b-142">Koşullar yanlışsa (uygulama bir hizmet olarak çalıştırılır):</span><span class="sxs-lookup"><span data-stu-id="ec41b-142">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="ec41b-143">Uygulamanın <xref:System.IO.Directory.SetCurrentDirectory*> yayımlanmış konumunun yolunu çağırın ve kullanın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-143">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="ec41b-144">Bir Windows <xref:System.IO.Directory.GetCurrentDirectory*> hizmeti uygulaması çağrıldığında *C:\\Windows\\system32* klasörünü <xref:System.IO.Directory.GetCurrentDirectory*> döndürdüğünden yolu almak için çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-144">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="ec41b-145">Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-145">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="ec41b-146">Bu adım, uygulamanın ' de `CreateWebHostBuilder`yapılandırılmadan önce gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-146">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="ec41b-147">Uygulamayı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> bir hizmet olarak çalıştırmak için çağırın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-147">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="ec41b-148">[Komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden, `--console` bağımsız değişkenleri almadan önce <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> anahtar bağımsız değişkenlerden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-148">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="ec41b-149">Windows olay günlüğü 'ne yazmak için EventLog sağlayıcısını öğesine <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-149">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="ec41b-150">Günlük kaydı düzeyini `Logging:LogLevel:Default` appSettings içindeki anahtarla ayarlayın *. Production. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="ec41b-150">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="ec41b-151">Örnek uygulamadan aşağıdaki örnekte, `RunAsCustomService` uygulama içindeki ömür olaylarını işlemek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> yerine çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-151">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="ec41b-152">Daha fazla bilgi için [olayları başlatma ve durdurma olaylarını](#handle-starting-and-stopping-events) inceleyin bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-152">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="ec41b-153">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="ec41b-153">Deployment type</span></span>

<span data-ttu-id="ec41b-154">Dağıtım senaryoları hakkında bilgi ve öneriler için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="ec41b-154">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="ec41b-155">Çerçeveye bağımlı dağıtım (FDD)</span><span class="sxs-lookup"><span data-stu-id="ec41b-155">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="ec41b-156">Çerçeveye bağımlı dağıtım (FDD), hedef sistemde .NET Core 'un paylaşılan sistem genelindeki bir sürümünün varlığına dayanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-156">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="ec41b-157">Bu makaledeki kılavuzdan sonra FDD senaryosu benimsendiği zaman SDK, *çerçeveye bağlı yürütülebilir dosya*olarak adlandırılan yürütülebilir bir dosya ( *. exe*) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ec41b-157">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ec41b-158">Aşağıdaki özellik öğelerini proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ec41b-158">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="ec41b-159">`<OutputType>`Uygulamanın çıkış türü (`Exe` yürütülebilir için). &ndash;</span><span class="sxs-lookup"><span data-stu-id="ec41b-159">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="ec41b-160">`<LangVersion>`Dil sürümü(`latest` veya`preview`). &ndash; C#</span><span class="sxs-lookup"><span data-stu-id="ec41b-160">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="ec41b-161">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ec41b-162">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="ec41b-163">Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="ec41b-164">Aşağıdaki örnekte, RID olarak `win7-x64`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="ec41b-165">`<SelfContained>` Özelliği olarak`false`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="ec41b-166">Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.</span><span class="sxs-lookup"><span data-stu-id="ec41b-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="ec41b-167">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-167">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ec41b-168">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-168">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="ec41b-169">Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-169">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="ec41b-170">Aşağıdaki örnekte, RID olarak `win7-x64`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-170">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="ec41b-171">`<SelfContained>` Özelliği olarak`false`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-171">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="ec41b-172">Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.</span><span class="sxs-lookup"><span data-stu-id="ec41b-172">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="ec41b-173">`<UseAppHost>` Özelliği olarak`true`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-173">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="ec41b-174">Bu özellik, bir FDD için bir etkinleştirme yolu (yürütülebilir, *. exe*) ile hizmeti sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec41b-174">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="ec41b-175">Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-175">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ec41b-176">*Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-176">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="ec41b-177">Kendi içinde dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="ec41b-177">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="ec41b-178">Kendinden bağımsız dağıtım (SCD), ana bilgisayar sisteminde paylaşılan bir Framework varlığına güvenmez.</span><span class="sxs-lookup"><span data-stu-id="ec41b-178">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="ec41b-179">Çalışma zamanı ve uygulamanın bağımlılıkları uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-179">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="ec41b-180">Hedef Framework 'ü içeren ' de `<PropertyGroup>` bir Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) bulunur:</span><span class="sxs-lookup"><span data-stu-id="ec41b-180">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="ec41b-181">Birden çok RID için yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="ec41b-181">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="ec41b-182">RID 'leri, noktalı virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-182">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="ec41b-183">> (Plural) adlı [ \<runtimetanımlayıcılar](/dotnet/core/tools/csproj#runtimeidentifiers) özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-183">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="ec41b-184">Daha fazla bilgi için bkz. [.NET Core RID kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ec41b-184">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ec41b-185">Bir `<SelfContained>` özellik şu şekilde `true`ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="ec41b-185">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="ec41b-186">Hizmet Kullanıcı hesabı</span><span class="sxs-lookup"><span data-stu-id="ec41b-186">Service user account</span></span>

<span data-ttu-id="ec41b-187">Bir hizmet için Kullanıcı hesabı oluşturmak için, bir yönetim PowerShell 6 komut kabuğundan [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-187">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="ec41b-188">Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/Build 10.0.17763) veya sonraki sürümler:</span><span class="sxs-lookup"><span data-stu-id="ec41b-188">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="ec41b-189">Windows 10 Ekim 2018 (sürüm 1809/Build 10.0.17763) sürümünden önceki Windows IŞLETIM sistemlerinde:</span><span class="sxs-lookup"><span data-stu-id="ec41b-189">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="ec41b-190">İstendiğinde [güçlü bir parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) sağlayın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-190">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="ec41b-191">Parametresi New [-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ine bir süre sonu <xref:System.DateTime>ile sağlanmamışsa hesabın süresi dolmaz. `-AccountExpires`</span><span class="sxs-lookup"><span data-stu-id="ec41b-191">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="ec41b-192">Daha fazla bilgi için bkz. [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmet Kullanıcı hesapları](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="ec41b-192">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="ec41b-193">Active Directory kullanırken kullanıcıları yönetmeye yönelik alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-193">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="ec41b-194">Daha fazla bilgi için bkz. [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="ec41b-194">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="ec41b-195">Hizmet hakları olarak oturum açma</span><span class="sxs-lookup"><span data-stu-id="ec41b-195">Log on as a service rights</span></span>

<span data-ttu-id="ec41b-196">Hizmet Kullanıcı hesabı için *hizmet hakları olarak oturum* açma oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="ec41b-196">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="ec41b-197">Yerel Güvenlik Ilkesi düzenleyicisini, *secpol. msc*' i çalıştırarak açın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-197">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="ec41b-198">**Yerel ilkeler** düğümünü genişletin ve **Kullanıcı hakları ataması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-198">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="ec41b-199">**Hizmet olarak oturum** açma ilkesi açın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-199">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="ec41b-200">**Kullanıcı veya Grup Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-200">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="ec41b-201">Aşağıdaki yaklaşımlardan birini kullanarak nesne adını (Kullanıcı hesabı) sağlayın:</span><span class="sxs-lookup"><span data-stu-id="ec41b-201">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="ec41b-202">Kullanıcı hesabını (`{DOMAIN OR COMPUTER NAME\USER}`) nesne adı alanına yazın ve kullanıcıyı ilkeye eklemek için **Tamam** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-202">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="ec41b-203">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-203">Select **Advanced**.</span></span> <span data-ttu-id="ec41b-204">**Şimdi bul**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-204">Select **Find Now**.</span></span> <span data-ttu-id="ec41b-205">Listeden Kullanıcı hesabını seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-205">Select the user account from the list.</span></span> <span data-ttu-id="ec41b-206">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-206">Select **OK**.</span></span> <span data-ttu-id="ec41b-207">Kullanıcıyı ilkeye eklemek için yeniden **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-207">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="ec41b-208">Değişiklikleri kabul etmek için **Tamam ' ı** veya **Uygula** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-208">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="ec41b-209">Windows hizmetini oluşturma ve yönetme</span><span class="sxs-lookup"><span data-stu-id="ec41b-209">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="ec41b-210">Bir hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="ec41b-210">Create a service</span></span>

<span data-ttu-id="ec41b-211">Bir hizmeti kaydetmek için PowerShell komutlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-211">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="ec41b-212">Bir yönetim PowerShell 6 komut kabuğundan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="ec41b-212">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="ec41b-213">`{EXE PATH}`Konaktaki uygulamanın klasörünün yolu (örneğin, `d:\myservice`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="ec41b-213">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="ec41b-214">Uygulamanın yürütülebilir dosyasını yola eklemeyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-214">Don't include the app's executable in the path.</span></span> <span data-ttu-id="ec41b-215">Sondaki eğik çizgi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-215">A trailing slash isn't required.</span></span>
* <span data-ttu-id="ec41b-216">`{DOMAIN OR COMPUTER NAME\USER}`Hizmet Kullanıcı hesabı (örneğin, `Contoso\ServiceUser`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="ec41b-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="ec41b-217">`{NAME}`Hizmet adı (örneğin, `MyService`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="ec41b-217">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="ec41b-218">`{EXE FILE PATH}`Uygulamanın yürütülebilir yolu (örneğin, `d:\myservice\myservice.exe`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="ec41b-218">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="ec41b-219">Yürütülebilir dosyanın dosya adını uzantısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-219">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="ec41b-220">`{DESCRIPTION}`Hizmet açıklaması (örneğin, `My sample service`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="ec41b-220">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="ec41b-221">`{DISPLAY NAME}`Hizmet görünen adı (örneğin, `My Service`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="ec41b-221">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="ec41b-222">Hizmet başlatma</span><span class="sxs-lookup"><span data-stu-id="ec41b-222">Start a service</span></span>

<span data-ttu-id="ec41b-223">Aşağıdaki PowerShell 6 komutuyla bir hizmet başlatın:</span><span class="sxs-lookup"><span data-stu-id="ec41b-223">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="ec41b-224">Komutun başlatılması birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="ec41b-224">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="ec41b-225">Hizmetin durumunu belirleme</span><span class="sxs-lookup"><span data-stu-id="ec41b-225">Determine a service's status</span></span>

<span data-ttu-id="ec41b-226">Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ec41b-226">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="ec41b-227">Durum, aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="ec41b-227">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="ec41b-228">Bir hizmeti durdur</span><span class="sxs-lookup"><span data-stu-id="ec41b-228">Stop a service</span></span>

<span data-ttu-id="ec41b-229">Aşağıdaki PowerShell 6 komutuyla bir hizmeti durdurun:</span><span class="sxs-lookup"><span data-stu-id="ec41b-229">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="ec41b-230">Hizmeti Kaldır</span><span class="sxs-lookup"><span data-stu-id="ec41b-230">Remove a service</span></span>

<span data-ttu-id="ec41b-231">Bir hizmeti durdurmak için kısa bir gecikmeden sonra, aşağıdaki PowerShell 6 komutuyla bir hizmeti kaldırın:</span><span class="sxs-lookup"><span data-stu-id="ec41b-231">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="ec41b-232">Olayları başlatma ve durdurma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="ec41b-232">Handle starting and stopping events</span></span>

<span data-ttu-id="ec41b-233">, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*> <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>Ve olaylarınıişlemekiçin:<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*></span><span class="sxs-lookup"><span data-stu-id="ec41b-233">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="ec41b-234">`OnStarting`, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> Ve`OnStarted`yöntemleriyle türetilen bir sınıfoluşturun:`OnStopping`</span><span class="sxs-lookup"><span data-stu-id="ec41b-234">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="ec41b-235">' A <xref:Microsoft.AspNetCore.Hosting.IWebHost> `CustomWebHostService` geçişi içinbirgenişletmeyöntemioluşturun:<xref:System.ServiceProcess.ServiceBase.Run*></span><span class="sxs-lookup"><span data-stu-id="ec41b-235">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="ec41b-236">İçinde `Program.Main`, `RunAsCustomService` yerine<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>genişletme yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="ec41b-236">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="ec41b-237"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> Konumunu`Program.Main`görmek için, [dağıtım türü](#deployment-type) bölümünde gösterilen kod örneğine bakın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-237">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ec41b-238">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="ec41b-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ec41b-239">Internet 'ten veya şirket ağından gelen isteklerle etkileşime geçen ve bir ara sunucu veya yük dengeleyicinin arkasındaki Hizmetler ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-239">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="ec41b-240">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="ec41b-240">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="ec41b-241">Uç noktaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ec41b-241">Configure endpoints</span></span>

<span data-ttu-id="ec41b-242">Varsayılan olarak, ASP.NET Core öğesine `http://localhost:5000`bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-242">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ec41b-243">`ASPNETCORE_URLS` Ortam değişkenini ayarlayarak URL 'yi ve bağlantı noktasını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-243">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="ec41b-244">HTTPS uç noktaları için destek de dahil olmak üzere ek URL ve bağlantı noktası yapılandırma yaklaşımları için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="ec41b-244">For additional URL and port configuration approaches, including support for HTTPS endpoints, see the following topics:</span></span>

* <span data-ttu-id="ec41b-245"><xref:fundamentals/servers/kestrel#endpoint-configuration>Kestrel</span><span class="sxs-lookup"><span data-stu-id="ec41b-245"><xref:fundamentals/servers/kestrel#endpoint-configuration> (Kestrel)</span></span>
* <span data-ttu-id="ec41b-246"><xref:fundamentals/servers/httpsys#configure-windows-server>(HTTP. sys)</span><span class="sxs-lookup"><span data-stu-id="ec41b-246"><xref:fundamentals/servers/httpsys#configure-windows-server> (HTTP.sys)</span></span>

> [!NOTE]
> <span data-ttu-id="ec41b-247">Hizmet uç noktasının güvenliğini sağlamak için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="ec41b-247">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="ec41b-248">Geçerli dizin ve içerik kökü</span><span class="sxs-lookup"><span data-stu-id="ec41b-248">Current directory and content root</span></span>

<span data-ttu-id="ec41b-249">Windows hizmeti için çağırarak <xref:System.IO.Directory.GetCurrentDirectory*> döndürülen geçerli çalışma dizini *C:\\Windows\\system32* klasörüdür.</span><span class="sxs-lookup"><span data-stu-id="ec41b-249">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="ec41b-250">*System32* klasörü, bir hizmetin dosyalarını (örneğin, ayarlar dosyaları) depolamak için uygun bir konum değildir.</span><span class="sxs-lookup"><span data-stu-id="ec41b-250">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="ec41b-251">Bir hizmetin varlık ve ayar dosyalarını sürdürmek ve erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-251">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="ec41b-252">ContentRootPath veya ContentRootFileProvider kullanın</span><span class="sxs-lookup"><span data-stu-id="ec41b-252">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="ec41b-253">[Ihostenvironment. contentrootpath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) kullanın veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> bir uygulamanın kaynaklarını bulun.</span><span class="sxs-lookup"><span data-stu-id="ec41b-253">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="ec41b-254">Uygulamanın klasörü için içerik kök yolunu ayarla</span><span class="sxs-lookup"><span data-stu-id="ec41b-254">Set the content root path to the app's folder</span></span>

<span data-ttu-id="ec41b-255">, <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Bir hizmet oluşturulduğunda `binPath` bağımsız değişkene aynı yol olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ec41b-255">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="ec41b-256">Ayarlar dosyalarına yollar `GetCurrentDirectory` oluşturmak için çağırmak yerine, uygulamanın içerik kökünün <xref:System.IO.Directory.SetCurrentDirectory*> yolunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="ec41b-256">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="ec41b-257">İçinde `Program.Main`, hizmetin yürütülebilir dosyasının yolunu belirleyin ve uygulamanın içerik kökünü oluşturmak için yolu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ec41b-257">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="ec41b-258">Hizmetin dosyalarını diskte uygun bir konumda depolayın</span><span class="sxs-lookup"><span data-stu-id="ec41b-258">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="ec41b-259">Dosyaları içeren klasöre <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanırken ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> mutlak bir yol belirtin.</span><span class="sxs-lookup"><span data-stu-id="ec41b-259">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec41b-260">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ec41b-260">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="ec41b-261">[Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırması ve SNı desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="ec41b-261">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="ec41b-262">[Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırması ve SNı desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="ec41b-262">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
