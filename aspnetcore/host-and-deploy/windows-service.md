---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/03/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4cfca4b38543ff073bb98dc09b483d96096928ae
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692563"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="882d5-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="882d5-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="882d5-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="882d5-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="882d5-105">ASP.NET Core uygulaması Windows barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) IIS kullanmadan.</span><span class="sxs-lookup"><span data-stu-id="882d5-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="882d5-106">Bir Windows hizmeti olarak barındırıldığında sunucu yeniden başlatıldıktan sonra uygulamayı otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="882d5-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="882d5-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="882d5-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="882d5-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="882d5-108">Prerequisites</span></span>

* [<span data-ttu-id="882d5-109">ASP.NET Core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="882d5-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="882d5-110">PowerShell 6.2 veya üstü</span><span class="sxs-lookup"><span data-stu-id="882d5-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="882d5-111">Çalışan hizmet şablonu</span><span class="sxs-lookup"><span data-stu-id="882d5-111">Worker Service template</span></span>

<span data-ttu-id="882d5-112">ASP.NET Core çalışan hizmet şablonu yazmak için bir başlangıç noktası sağlar. hizmet uygulamaları uzun süre çalışan.</span><span class="sxs-lookup"><span data-stu-id="882d5-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="882d5-113">Şablon, bir Windows hizmeti uygulaması için temel olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="882d5-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="882d5-114">.NET Core şablonundan bir çalışan Service uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="882d5-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="882d5-115">Sunulan yönergeleri [uygulama yapılandırması](#app-configuration) bir Windows hizmeti olarak çalıştırabilmeniz için çalışan hizmet uygulamasını güncelleştirmek için bölüm.</span><span class="sxs-lookup"><span data-stu-id="882d5-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="882d5-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="882d5-116">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="882d5-117">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="882d5-117">Create a new project.</span></span>
1. <span data-ttu-id="882d5-118">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="882d5-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="882d5-119">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="882d5-119">Select **Next**.</span></span>
1. <span data-ttu-id="882d5-120">Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="882d5-120">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="882d5-121">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="882d5-121">Select **Create**.</span></span>
1. <span data-ttu-id="882d5-122">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.</span><span class="sxs-lookup"><span data-stu-id="882d5-122">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="882d5-123">Seçin **çalışan hizmet** şablonu.</span><span class="sxs-lookup"><span data-stu-id="882d5-123">Select the **Worker Service** template.</span></span> <span data-ttu-id="882d5-124">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="882d5-124">Select **Create**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="882d5-125">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="882d5-125">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="882d5-126">Çalışan hizmetin kullanın (`worker`) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komut kabuğu komutunu.</span><span class="sxs-lookup"><span data-stu-id="882d5-126">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="882d5-127">Aşağıdaki örnekte, bir çalışan hizmet uygulaması adlandırılmış oluşturulduğunda `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="882d5-127">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="882d5-128">İçin bir klasör `ContosoWorkerService` uygulama komut yürütülürken otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="882d5-128">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="882d5-129">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="882d5-129">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="882d5-130">`IHostBuilder.UseWindowsService`, tarafından sağlanan [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) paketini, ana bilgisayar oluşturma sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="882d5-130">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="882d5-131">Uygulamayı yöntemi bir Windows hizmeti olarak çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="882d5-131">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="882d5-132">Ana kullanım ömrü ayarlar `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="882d5-132">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="882d5-133">İçerik kök ayarlar.</span><span class="sxs-lookup"><span data-stu-id="882d5-133">Sets the content root.</span></span>
* <span data-ttu-id="882d5-134">Varsayılan kaynak adıyla uygulama adı ile olay günlüğü için günlük kaydını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="882d5-134">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="882d5-135">Günlük düzeyi kullanılarak yapılandırılabilir `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="882d5-135">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="882d5-136">Yalnızca Yöneticiler yeni olay kaynakları oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="882d5-136">Only administrators can create new event sources.</span></span> <span data-ttu-id="882d5-137">Uygulama adı kullanarak bir olay kaynağı oluşturulamıyor olduğunda bir uyarı günlüğe kaydedilir *uygulama* kaynağı ve olay günlüklerini devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="882d5-137">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="882d5-138">Uygulama için paket başvuruları gerekiyor. [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="882d5-138">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="882d5-139">Test ve hizmet dışında çalışırken hata ayıklama için uygulamayı bir hizmet veya bir konsol uygulaması olarak çalışıp çalışmadığını belirlemek için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="882d5-139">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="882d5-140">Hata ayıklayıcıyı eklediyseniz veya inceleyin `--console` anahtar.</span><span class="sxs-lookup"><span data-stu-id="882d5-140">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="882d5-141">İki koşuldan birinin (uygulamayı değil çalıştırma hizmet olarak) true ise, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="882d5-141">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="882d5-142">Koşullar (uygulama, hizmet olarak çalıştırıldığında) false olduğunda:</span><span class="sxs-lookup"><span data-stu-id="882d5-142">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="882d5-143">Çağrı <xref:System.IO.Directory.SetCurrentDirectory*> ve uygulamanın yayımlanmış konumuna bir yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="882d5-143">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="882d5-144">Remove() çağırmayın <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti uygulaması döndürüldüğünden yolunu almak için *C:\\WINDOWS\\system32* klasör zaman <xref:System.IO.Directory.GetCurrentDirectory*> çağrılır.</span><span class="sxs-lookup"><span data-stu-id="882d5-144">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="882d5-145">Daha fazla bilgi için [geçerli dizin ve içerik kök](#current-directory-and-content-root) bölümü.</span><span class="sxs-lookup"><span data-stu-id="882d5-145">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="882d5-146">Bu adım, uygulamanın yapılandırılan önce gerçekleştirilir `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="882d5-146">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="882d5-147">Çağrı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulamasını bir hizmet olarak çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="882d5-147">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="882d5-148">Çünkü [komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirir `--console` anahtarı bağımsız değişkenlerden önce kaldırılır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bağımsız değişkenleri alır.</span><span class="sxs-lookup"><span data-stu-id="882d5-148">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="882d5-149">Windows olay günlüğüne yazmak için olay günlüğü Sağlayıcısı Ekle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="882d5-149">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="882d5-150">İle günlük tutma düzeyini ayarlamaya `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="882d5-150">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="882d5-151">Aşağıdaki örnekte, örnek uygulamadan `RunAsCustomService` yerine adlı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulama içinde ömür olayları işlemek için.</span><span class="sxs-lookup"><span data-stu-id="882d5-151">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="882d5-152">Daha fazla bilgi için [başlatılması ve durdurulması olaylarını işlemek](#handle-starting-and-stopping-events) bölümü.</span><span class="sxs-lookup"><span data-stu-id="882d5-152">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="882d5-153">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="882d5-153">Deployment type</span></span>

<span data-ttu-id="882d5-154">Bilgi ve dağıtım senaryoları hakkında daha fazla öneri için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="882d5-154">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="882d5-155">Framework bağımlı dağıtım (FDD)</span><span class="sxs-lookup"><span data-stu-id="882d5-155">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="882d5-156">.NET Core hedef sistemdeki bir paylaşılan sistem genelinde sürüm varlığını Framework bağımlı dağıtım (FDD) kullanır.</span><span class="sxs-lookup"><span data-stu-id="882d5-156">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="882d5-157">Bu makaledeki yönergeleri izleyerek FDD senaryo benimsenen, SDK'sı bir çalıştırılabilir dosyası oluşturur ( *.exe*) adlı bir *framework bağımlı yürütülebilir*.</span><span class="sxs-lookup"><span data-stu-id="882d5-157">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="882d5-158">Aşağıdaki özellik öğeleri, proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="882d5-158">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="882d5-159">`<OutputType>` &ndash; Uygulama türü Çıktı (`Exe` yürütülebilir dosya için).</span><span class="sxs-lookup"><span data-stu-id="882d5-159">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="882d5-160">`<LangVersion>` &ndash; C# Dil sürümü (`latest` veya `preview`).</span><span class="sxs-lookup"><span data-stu-id="882d5-160">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="882d5-161">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="882d5-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="882d5-162">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="882d5-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="882d5-163">Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="882d5-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="882d5-164">Aşağıdaki örnekte, RID kümesine `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="882d5-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="882d5-165">`<SelfContained>` Özelliği `false`.</span><span class="sxs-lookup"><span data-stu-id="882d5-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="882d5-166">Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin ( *.exe*) Windows ve paylaşılan olan .NET Core Framework'e bağlı bir uygulama dosyası.</span><span class="sxs-lookup"><span data-stu-id="882d5-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="882d5-167">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="882d5-167">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="882d5-168">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="882d5-168">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="882d5-169">Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="882d5-169">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="882d5-170">Aşağıdaki örnekte, RID kümesine `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="882d5-170">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="882d5-171">`<SelfContained>` Özelliği `false`.</span><span class="sxs-lookup"><span data-stu-id="882d5-171">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="882d5-172">Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin ( *.exe*) Windows ve paylaşılan olan .NET Core Framework'e bağlı bir uygulama dosyası.</span><span class="sxs-lookup"><span data-stu-id="882d5-172">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="882d5-173">`<UseAppHost>` Özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="882d5-173">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="882d5-174">Bu özellik etkinleştirme yolu hizmetiyle sağlar (bir yürütülebilir dosya *.exe*) bir FDD için.</span><span class="sxs-lookup"><span data-stu-id="882d5-174">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="882d5-175">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="882d5-175">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="882d5-176">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="882d5-176">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="882d5-177">Kendi başına dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="882d5-177">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="882d5-178">Kendi başına dağıtım (SCD), konak sistemindeki paylaşılan bir çerçeve varlığına içermez.</span><span class="sxs-lookup"><span data-stu-id="882d5-178">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="882d5-179">Çalışma zamanı ve uygulamanın bağımlılıklarını uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="882d5-179">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="882d5-180">Bir Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) dahil `<PropertyGroup>` , hedef Framework'ü içerir:</span><span class="sxs-lookup"><span data-stu-id="882d5-180">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="882d5-181">İçin birden fazla RID yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="882d5-181">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="882d5-182">RID noktalı virgülle ayrılmış bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="882d5-182">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="882d5-183">Özellik adını kullanan [ \<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (çoğul).</span><span class="sxs-lookup"><span data-stu-id="882d5-183">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="882d5-184">Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="882d5-184">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="882d5-185">A `<SelfContained>` özelliği `true`:</span><span class="sxs-lookup"><span data-stu-id="882d5-185">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="882d5-186">Hizmet kullanıcı hesabı</span><span class="sxs-lookup"><span data-stu-id="882d5-186">Service user account</span></span>

<span data-ttu-id="882d5-187">Bir hizmet için bir kullanıcı hesabı oluşturmak için kullanın [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet'i bir yönetim PowerShell 6'yı komut kabuğundan.</span><span class="sxs-lookup"><span data-stu-id="882d5-187">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="882d5-188">Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/derleme 10.0.17763) veya daha sonra:</span><span class="sxs-lookup"><span data-stu-id="882d5-188">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="882d5-189">Windows 10 Ekim 2018'den önceki Windows işletim sisteminde Update (sürüm 1809/derleme 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="882d5-189">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="882d5-190">Sağlayan bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) istendiğinde.</span><span class="sxs-lookup"><span data-stu-id="882d5-190">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="882d5-191">Sürece `-AccountExpires` parametre tarafından sağlanan [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) bir sona erme cmdlet'iyle <xref:System.DateTime>, hesabın süresi sona ermiyor.</span><span class="sxs-lookup"><span data-stu-id="882d5-191">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="882d5-192">Daha fazla bilgi için [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="882d5-192">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="882d5-193">Active Directory kullanarak kullanıcıları yönetme için alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="882d5-193">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="882d5-194">Daha fazla bilgi için [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="882d5-194">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="882d5-195">Bir hizmet hakları oturum açın</span><span class="sxs-lookup"><span data-stu-id="882d5-195">Log on as a service rights</span></span>

<span data-ttu-id="882d5-196">Kurmak için *hizmet oturum açma* rights hizmeti kullanıcı hesabı için:</span><span class="sxs-lookup"><span data-stu-id="882d5-196">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="882d5-197">Yerel Güvenlik İlkesi Düzenleyicisi'ni çalıştırarak açmak *secpool.msc*.</span><span class="sxs-lookup"><span data-stu-id="882d5-197">Open the Local Security Policy editor by running *secpool.msc*.</span></span>
1. <span data-ttu-id="882d5-198">Genişletin **yerel ilkeler** düğümünü seçip alt **kullanıcı hakları ataması**.</span><span class="sxs-lookup"><span data-stu-id="882d5-198">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="882d5-199">Açık **hizmet oturum açma** ilkesi.</span><span class="sxs-lookup"><span data-stu-id="882d5-199">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="882d5-200">Seçin **kullanıcı veya grup ekleme**.</span><span class="sxs-lookup"><span data-stu-id="882d5-200">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="882d5-201">Aşağıdaki yaklaşımlardan birini kullanarak nesne adı (kullanıcı hesabı) sağlayın:</span><span class="sxs-lookup"><span data-stu-id="882d5-201">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="882d5-202">Kullanıcı hesabını yazın (`{DOMAIN OR COMPUTER NAME\USER}`) nesne adını seçin ve alan **Tamam** kullanıcı ilkesine eklemek için.</span><span class="sxs-lookup"><span data-stu-id="882d5-202">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="882d5-203">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="882d5-203">Select **Advanced**.</span></span> <span data-ttu-id="882d5-204">Seçin **Şimdi Bul**.</span><span class="sxs-lookup"><span data-stu-id="882d5-204">Select **Find Now**.</span></span> <span data-ttu-id="882d5-205">Kullanıcı hesabı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="882d5-205">Select the user account from the list.</span></span> <span data-ttu-id="882d5-206">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="882d5-206">Select **OK**.</span></span> <span data-ttu-id="882d5-207">Seçin **Tamam** yeniden kullanıcı ilkesine eklemek için.</span><span class="sxs-lookup"><span data-stu-id="882d5-207">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="882d5-208">Seçin **Tamam** veya **Uygula** değişiklikleri kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="882d5-208">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="882d5-209">Oluşturma ve yönetme Windows hizmeti</span><span class="sxs-lookup"><span data-stu-id="882d5-209">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="882d5-210">Bir hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="882d5-210">Create a service</span></span>

<span data-ttu-id="882d5-211">Bir hizmeti kaydetmek için PowerShell komutlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="882d5-211">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="882d5-212">Bir yönetici PowerShell 6'yı komut kabuğundan, aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="882d5-212">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="882d5-213">`{EXE PATH}` &ndash; Konaktaki uygulamanın klasör yolu (örneğin, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="882d5-213">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="882d5-214">Uygulamanın yürütülebilir yolu dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="882d5-214">Don't include the app's executable in the path.</span></span> <span data-ttu-id="882d5-215">Sonunda eğik çizgi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="882d5-215">A trailing slash isn't required.</span></span>
* <span data-ttu-id="882d5-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Hizmeti kullanıcı hesabını (örneğin, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="882d5-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="882d5-217">`{NAME}` &ndash; Hizmet adı (örneğin, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="882d5-217">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="882d5-218">`{EXE FILE PATH}` &ndash; Uygulamanın yürütülebilir dosya yolu (örneğin, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="882d5-218">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="882d5-219">Uzantılı dosya adı yürütülebilir dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="882d5-219">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="882d5-220">`{DESCRIPTION}` &ndash; Hizmet açıklaması (örneğin, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="882d5-220">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="882d5-221">`{DISPLAY NAME}` &ndash; Hizmet görünen adı (örneğin, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="882d5-221">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="882d5-222">Bir hizmeti Başlat</span><span class="sxs-lookup"><span data-stu-id="882d5-222">Start a service</span></span>

<span data-ttu-id="882d5-223">Bir hizmet PowerShell 6'yı aşağıdaki komutla başlatın:</span><span class="sxs-lookup"><span data-stu-id="882d5-223">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="882d5-224">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="882d5-224">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="882d5-225">Bir hizmetin durumunu belirleme</span><span class="sxs-lookup"><span data-stu-id="882d5-225">Determine a service's status</span></span>

<span data-ttu-id="882d5-226">Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="882d5-226">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="882d5-227">Durumu aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="882d5-227">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="882d5-228">Bir hizmeti Durdur</span><span class="sxs-lookup"><span data-stu-id="882d5-228">Stop a service</span></span>

<span data-ttu-id="882d5-229">Powershell 6'yı aşağıdaki komutla bir hizmet durdurun:</span><span class="sxs-lookup"><span data-stu-id="882d5-229">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="882d5-230">Bir hizmeti Kaldır</span><span class="sxs-lookup"><span data-stu-id="882d5-230">Remove a service</span></span>

<span data-ttu-id="882d5-231">Bir hizmeti durdurmak için kısa bir gecikmeyle, hizmet Powershell 6'yı aşağıdaki komutla kaldırın:</span><span class="sxs-lookup"><span data-stu-id="882d5-231">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="882d5-232">Başlatma ve durdurma olayları işleme</span><span class="sxs-lookup"><span data-stu-id="882d5-232">Handle starting and stopping events</span></span>

<span data-ttu-id="882d5-233">İşlenecek <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olayları:</span><span class="sxs-lookup"><span data-stu-id="882d5-233">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="882d5-234">Türetilen bir sınıf oluşturmanız <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> ile `OnStarting`, `OnStarted`, ve `OnStopping` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="882d5-234">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="882d5-235">Bir genişletme yöntemi için oluşturma <xref:Microsoft.AspNetCore.Hosting.IWebHost> Geçiren `CustomWebHostService` için <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="882d5-235">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="882d5-236">İçinde `Program.Main`, çağrı `RunAsCustomService` genişletme yöntemi yerine <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="882d5-236">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="882d5-237">Konumu görmek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> içinde `Program.Main`, gösterilen kod örneği başvurmak [dağıtım türü](#deployment-type) bölümü.</span><span class="sxs-lookup"><span data-stu-id="882d5-237">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="882d5-238">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="882d5-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="882d5-239">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="882d5-239">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="882d5-240">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="882d5-240">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="882d5-241">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="882d5-241">Configure HTTPS</span></span>

<span data-ttu-id="882d5-242">Güvenli bir uç nokta ile bir hizmeti yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="882d5-242">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="882d5-243">Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="882d5-243">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="882d5-244">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="882d5-244">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="882d5-245">Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="882d5-245">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="882d5-246">Geçerli dizin ve içerik kök</span><span class="sxs-lookup"><span data-stu-id="882d5-246">Current directory and content root</span></span>

<span data-ttu-id="882d5-247">Geçerli çalışma dizini çağırarak döndürülen <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör.</span><span class="sxs-lookup"><span data-stu-id="882d5-247">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="882d5-248">*System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum.</span><span class="sxs-lookup"><span data-stu-id="882d5-248">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="882d5-249">Korumak ve bir hizmetin varlıklar ve ayar dosyaları erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="882d5-249">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="882d5-250">Kullanım ContentRootPath veya ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="882d5-250">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="882d5-251">Kullanım [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> uygulamanın kaynaklar bulunamıyor.</span><span class="sxs-lookup"><span data-stu-id="882d5-251">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="882d5-252">Uygulamanın klasör için içerik kök yolu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="882d5-252">Set the content root path to the app's folder</span></span>

<span data-ttu-id="882d5-253"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Sağlanan aynı yol `binPath` hizmet oluşturulduğunda bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="882d5-253">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="882d5-254">Çağırmak yerine `GetCurrentDirectory` ayarları dosyalara olan yolları oluşturmak için arama <xref:System.IO.Directory.SetCurrentDirectory*> ile içerik uygulamanın kök yolu.</span><span class="sxs-lookup"><span data-stu-id="882d5-254">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="882d5-255">İçinde `Program.Main`, hizmetin yürütülebilir dosya klasörü yolunu belirlemek ve uygulamanın içerik kök'kurmak için yolunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="882d5-255">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="882d5-256">Disk üzerinde uygun bir konumda bir hizmetin dosyaları Store</span><span class="sxs-lookup"><span data-stu-id="882d5-256">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="882d5-257">Mutlak bir yol belirtin <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> kullanırken bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> dosyaları içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="882d5-257">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="882d5-258">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="882d5-258">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="882d5-259">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="882d5-259">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="882d5-260">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="882d5-260">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
