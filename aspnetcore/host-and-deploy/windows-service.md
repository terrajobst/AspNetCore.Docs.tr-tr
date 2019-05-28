---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ab36bc1b2827c80bb1e7b9e8cee558b346a991f8
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251429"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="4ab15-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="4ab15-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="4ab15-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4ab15-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4ab15-105">ASP.NET Core uygulaması Windows barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) IIS kullanmadan.</span><span class="sxs-lookup"><span data-stu-id="4ab15-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="4ab15-106">Bir Windows hizmeti olarak barındırıldığında sunucu yeniden başlatıldıktan sonra uygulamayı otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="4ab15-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="4ab15-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ab15-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ab15-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4ab15-108">Prerequisites</span></span>

* [<span data-ttu-id="4ab15-109">ASP.NET Core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4ab15-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="4ab15-110">PowerShell 6.2 veya üstü</span><span class="sxs-lookup"><span data-stu-id="4ab15-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="4ab15-111">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4ab15-111">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4ab15-112">`IHostBuilder.UseWindowsService`, tarafından sağlanan [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) paketini, ana bilgisayar oluşturma sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4ab15-112">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="4ab15-113">Uygulamayı yöntemi bir Windows hizmeti olarak çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="4ab15-113">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="4ab15-114">Ana kullanım ömrü ayarlar `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-114">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="4ab15-115">İçerik kök ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4ab15-115">Sets the content root.</span></span>
* <span data-ttu-id="4ab15-116">Varsayılan kaynak adıyla uygulama adı ile olay günlüğü için günlük kaydını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="4ab15-116">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="4ab15-117">Günlük düzeyi kullanılarak yapılandırılabilir `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="4ab15-117">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="4ab15-118">Yalnızca Yöneticiler yeni olay kaynakları oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ab15-118">Only administrators can create new event sources.</span></span> <span data-ttu-id="4ab15-119">Uygulama adı kullanarak bir olay kaynağı oluşturulamıyor olduğunda bir uyarı günlüğe kaydedilir *uygulama* kaynağı ve olay günlüklerini devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="4ab15-119">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4ab15-120">Uygulama için paket başvuruları gerekiyor. [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="4ab15-120">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="4ab15-121">Test ve hizmet dışında çalışırken hata ayıklama için uygulamayı bir hizmet veya bir konsol uygulaması olarak çalışıp çalışmadığını belirlemek için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4ab15-121">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="4ab15-122">Hata ayıklayıcıyı eklediyseniz veya inceleyin `--console` anahtar.</span><span class="sxs-lookup"><span data-stu-id="4ab15-122">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="4ab15-123">İki koşuldan birinin (uygulamayı değil çalıştırma hizmet olarak) true ise, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="4ab15-123">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="4ab15-124">Koşullar (uygulama, hizmet olarak çalıştırıldığında) false olduğunda:</span><span class="sxs-lookup"><span data-stu-id="4ab15-124">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="4ab15-125">Çağrı <xref:System.IO.Directory.SetCurrentDirectory*> ve uygulamanın yayımlanmış konumuna bir yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="4ab15-125">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="4ab15-126">Remove() çağırmayın <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti uygulaması döndürüldüğünden yolunu almak için *C:\\WINDOWS\\system32* klasör zaman <xref:System.IO.Directory.GetCurrentDirectory*> çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4ab15-126">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="4ab15-127">Daha fazla bilgi için [geçerli dizin ve içerik kök](#current-directory-and-content-root) bölümü.</span><span class="sxs-lookup"><span data-stu-id="4ab15-127">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="4ab15-128">Bu adım, uygulamanın yapılandırılan önce gerçekleştirilir `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-128">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="4ab15-129">Çağrı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulamasını bir hizmet olarak çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="4ab15-129">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="4ab15-130">Çünkü [komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirir `--console` anahtarı bağımsız değişkenlerden önce kaldırılır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bağımsız değişkenleri alır.</span><span class="sxs-lookup"><span data-stu-id="4ab15-130">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="4ab15-131">Windows olay günlüğüne yazmak için olay günlüğü Sağlayıcısı Ekle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="4ab15-131">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="4ab15-132">İle günlük tutma düzeyini ayarlamaya `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="4ab15-132">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="4ab15-133">Aşağıdaki örnekte, örnek uygulamadan `RunAsCustomService` yerine adlı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulama içinde ömür olayları işlemek için.</span><span class="sxs-lookup"><span data-stu-id="4ab15-133">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="4ab15-134">Daha fazla bilgi için [başlatılması ve durdurulması olaylarını işlemek](#handle-starting-and-stopping-events) bölümü.</span><span class="sxs-lookup"><span data-stu-id="4ab15-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="4ab15-135">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="4ab15-135">Deployment type</span></span>

<span data-ttu-id="4ab15-136">Bilgi ve dağıtım senaryoları hakkında daha fazla öneri için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="4ab15-136">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="4ab15-137">Framework bağımlı dağıtım (FDD)</span><span class="sxs-lookup"><span data-stu-id="4ab15-137">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="4ab15-138">.NET Core hedef sistemdeki bir paylaşılan sistem genelinde sürüm varlığını Framework bağımlı dağıtım (FDD) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4ab15-138">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="4ab15-139">Bu makaledeki yönergeleri izleyerek FDD senaryo benimsenen, SDK'sı bir çalıştırılabilir dosyası oluşturur ( *.exe*) adlı bir *framework bağımlı yürütülebilir*.</span><span class="sxs-lookup"><span data-stu-id="4ab15-139">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4ab15-140">Aşağıdaki özellik öğeleri, proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4ab15-140">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="4ab15-141">`<OutputType>` &ndash; Uygulama türü Çıktı (`Exe` yürütülebilir dosya için).</span><span class="sxs-lookup"><span data-stu-id="4ab15-141">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="4ab15-142">`<LangVersion>` &ndash; C# Dil sürümü (`latest` veya `preview`).</span><span class="sxs-lookup"><span data-stu-id="4ab15-142">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="4ab15-143">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="4ab15-143">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="4ab15-144">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-144">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="4ab15-145">Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="4ab15-145">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="4ab15-146">Aşağıdaki örnekte, RID kümesine `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-146">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="4ab15-147">`<SelfContained>` Özelliği `false`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-147">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="4ab15-148">Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin ( *.exe*) Windows ve paylaşılan olan .NET Core Framework'e bağlı bir uygulama dosyası.</span><span class="sxs-lookup"><span data-stu-id="4ab15-148">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="4ab15-149">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="4ab15-149">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="4ab15-150">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-150">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="4ab15-151">Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="4ab15-151">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="4ab15-152">Aşağıdaki örnekte, RID kümesine `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-152">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="4ab15-153">`<SelfContained>` Özelliği `false`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-153">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="4ab15-154">Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin ( *.exe*) Windows ve paylaşılan olan .NET Core Framework'e bağlı bir uygulama dosyası.</span><span class="sxs-lookup"><span data-stu-id="4ab15-154">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="4ab15-155">`<UseAppHost>` Özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-155">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="4ab15-156">Bu özellik etkinleştirme yolu hizmetiyle sağlar (bir yürütülebilir dosya *.exe*) bir FDD için.</span><span class="sxs-lookup"><span data-stu-id="4ab15-156">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="4ab15-157">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="4ab15-157">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="4ab15-158">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="4ab15-158">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="4ab15-159">Kendi başına dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="4ab15-159">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="4ab15-160">Kendi başına dağıtım (SCD), konak sistemindeki paylaşılan bir çerçeve varlığına içermez.</span><span class="sxs-lookup"><span data-stu-id="4ab15-160">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="4ab15-161">Çalışma zamanı ve uygulamanın bağımlılıklarını uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="4ab15-161">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="4ab15-162">Bir Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) dahil `<PropertyGroup>` , hedef Framework'ü içerir:</span><span class="sxs-lookup"><span data-stu-id="4ab15-162">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="4ab15-163">İçin birden fazla RID yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="4ab15-163">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="4ab15-164">RID noktalı virgülle ayrılmış bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ab15-164">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="4ab15-165">Özellik adını kullanan [ \<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (çoğul).</span><span class="sxs-lookup"><span data-stu-id="4ab15-165">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="4ab15-166">Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="4ab15-166">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4ab15-167">A `<SelfContained>` özelliği `true`:</span><span class="sxs-lookup"><span data-stu-id="4ab15-167">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="4ab15-168">Hizmet kullanıcı hesabı</span><span class="sxs-lookup"><span data-stu-id="4ab15-168">Service user account</span></span>

<span data-ttu-id="4ab15-169">Bir hizmet için bir kullanıcı hesabı oluşturmak için kullanın [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet'i bir yönetim PowerShell 6'yı komut kabuğundan.</span><span class="sxs-lookup"><span data-stu-id="4ab15-169">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="4ab15-170">Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/derleme 10.0.17763) veya daha sonra:</span><span class="sxs-lookup"><span data-stu-id="4ab15-170">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="4ab15-171">Windows 10 Ekim 2018'den önceki Windows işletim sisteminde Update (sürüm 1809/derleme 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="4ab15-171">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="4ab15-172">Sağlayan bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) istendiğinde.</span><span class="sxs-lookup"><span data-stu-id="4ab15-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="4ab15-173">Sürece `-AccountExpires` parametre tarafından sağlanan [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) bir sona erme cmdlet'iyle <xref:System.DateTime>, hesabın süresi sona ermiyor.</span><span class="sxs-lookup"><span data-stu-id="4ab15-173">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="4ab15-174">Daha fazla bilgi için [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="4ab15-174">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="4ab15-175">Active Directory kullanarak kullanıcıları yönetme için alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="4ab15-175">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="4ab15-176">Daha fazla bilgi için [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="4ab15-176">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="4ab15-177">Bir hizmet hakları oturum açın</span><span class="sxs-lookup"><span data-stu-id="4ab15-177">Log on as a service rights</span></span>

<span data-ttu-id="4ab15-178">Kurmak için *hizmet oturum açma* rights hizmeti kullanıcı hesabı için:</span><span class="sxs-lookup"><span data-stu-id="4ab15-178">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="4ab15-179">Yerel Güvenlik İlkesi Düzenleyicisi'ni çalıştırarak açmak *secpool.msc*.</span><span class="sxs-lookup"><span data-stu-id="4ab15-179">Open the Local Security Policy editor by running *secpool.msc*.</span></span>
1. <span data-ttu-id="4ab15-180">Genişletin **yerel ilkeler** düğümünü seçip alt **kullanıcı hakları ataması**.</span><span class="sxs-lookup"><span data-stu-id="4ab15-180">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="4ab15-181">Açık **hizmet oturum açma** ilkesi.</span><span class="sxs-lookup"><span data-stu-id="4ab15-181">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="4ab15-182">Seçin **kullanıcı veya grup ekleme**.</span><span class="sxs-lookup"><span data-stu-id="4ab15-182">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="4ab15-183">Aşağıdaki yaklaşımlardan birini kullanarak nesne adı (kullanıcı hesabı) sağlayın:</span><span class="sxs-lookup"><span data-stu-id="4ab15-183">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="4ab15-184">Kullanıcı hesabını yazın (`{DOMAIN OR COMPUTER NAME\USER}`) nesne adını seçin ve alan **Tamam** kullanıcı ilkesine eklemek için.</span><span class="sxs-lookup"><span data-stu-id="4ab15-184">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="4ab15-185">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="4ab15-185">Select **Advanced**.</span></span> <span data-ttu-id="4ab15-186">Seçin **Şimdi Bul**.</span><span class="sxs-lookup"><span data-stu-id="4ab15-186">Select **Find Now**.</span></span> <span data-ttu-id="4ab15-187">Kullanıcı hesabı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="4ab15-187">Select the user account from the list.</span></span> <span data-ttu-id="4ab15-188">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4ab15-188">Select **OK**.</span></span> <span data-ttu-id="4ab15-189">Seçin **Tamam** yeniden kullanıcı ilkesine eklemek için.</span><span class="sxs-lookup"><span data-stu-id="4ab15-189">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="4ab15-190">Seçin **Tamam** veya **Uygula** değişiklikleri kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="4ab15-190">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="4ab15-191">Oluşturma ve yönetme Windows hizmeti</span><span class="sxs-lookup"><span data-stu-id="4ab15-191">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="4ab15-192">Bir hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ab15-192">Create a service</span></span>

<span data-ttu-id="4ab15-193">Bir hizmeti kaydetmek için PowerShell komutlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="4ab15-193">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="4ab15-194">Bir yönetici PowerShell 6'yı komut kabuğundan, aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="4ab15-194">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="4ab15-195">`{EXE PATH}` &ndash; Konaktaki uygulamanın klasör yolu (örneğin, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="4ab15-195">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="4ab15-196">Uygulamanın yürütülebilir yolu dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="4ab15-196">Don't include the app's executable in the path.</span></span> <span data-ttu-id="4ab15-197">Sonunda eğik çizgi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4ab15-197">A trailing slash isn't required.</span></span>
* <span data-ttu-id="4ab15-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Hizmeti kullanıcı hesabını (örneğin, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="4ab15-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="4ab15-199">`{NAME}` &ndash; Hizmet adı (örneğin, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="4ab15-199">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="4ab15-200">`{EXE FILE PATH}` &ndash; Uygulamanın yürütülebilir dosya yolu (örneğin, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="4ab15-200">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="4ab15-201">Uzantılı dosya adı yürütülebilir dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="4ab15-201">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="4ab15-202">`{DESCRIPTION}` &ndash; Hizmet açıklaması (örneğin, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="4ab15-202">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="4ab15-203">`{DISPLAY NAME}` &ndash; Hizmet görünen adı (örneğin, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="4ab15-203">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="4ab15-204">Bir hizmeti Başlat</span><span class="sxs-lookup"><span data-stu-id="4ab15-204">Start a service</span></span>

<span data-ttu-id="4ab15-205">Bir hizmet PowerShell 6'yı aşağıdaki komutla başlatın:</span><span class="sxs-lookup"><span data-stu-id="4ab15-205">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="4ab15-206">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="4ab15-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="4ab15-207">Bir hizmetin durumunu belirleme</span><span class="sxs-lookup"><span data-stu-id="4ab15-207">Determine a service's status</span></span>

<span data-ttu-id="4ab15-208">Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="4ab15-208">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="4ab15-209">Durumu aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="4ab15-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="4ab15-210">Bir hizmeti Durdur</span><span class="sxs-lookup"><span data-stu-id="4ab15-210">Stop a service</span></span>

<span data-ttu-id="4ab15-211">Powershell 6'yı aşağıdaki komutla bir hizmet durdurun:</span><span class="sxs-lookup"><span data-stu-id="4ab15-211">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="4ab15-212">Bir hizmeti Kaldır</span><span class="sxs-lookup"><span data-stu-id="4ab15-212">Remove a service</span></span>

<span data-ttu-id="4ab15-213">Bir hizmeti durdurmak için kısa bir gecikmeyle, hizmet Powershell 6'yı aşağıdaki komutla kaldırın:</span><span class="sxs-lookup"><span data-stu-id="4ab15-213">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="4ab15-214">Başlatma ve durdurma olayları işleme</span><span class="sxs-lookup"><span data-stu-id="4ab15-214">Handle starting and stopping events</span></span>

<span data-ttu-id="4ab15-215">İşlenecek <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olayları:</span><span class="sxs-lookup"><span data-stu-id="4ab15-215">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="4ab15-216">Türetilen bir sınıf oluşturmanız <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> ile `OnStarting`, `OnStarted`, ve `OnStopping` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="4ab15-216">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="4ab15-217">Bir genişletme yöntemi için oluşturma <xref:Microsoft.AspNetCore.Hosting.IWebHost> Geçiren `CustomWebHostService` için <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="4ab15-217">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="4ab15-218">İçinde `Program.Main`, çağrı `RunAsCustomService` genişletme yöntemi yerine <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="4ab15-218">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="4ab15-219">Konumu görmek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> içinde `Program.Main`, gösterilen kod örneği başvurmak [dağıtım türü](#deployment-type) bölümü.</span><span class="sxs-lookup"><span data-stu-id="4ab15-219">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="4ab15-220">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="4ab15-220">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="4ab15-221">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="4ab15-221">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="4ab15-222">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="4ab15-222">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="4ab15-223">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4ab15-223">Configure HTTPS</span></span>

<span data-ttu-id="4ab15-224">Güvenli bir uç nokta ile bir hizmeti yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="4ab15-224">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="4ab15-225">Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4ab15-225">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="4ab15-226">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="4ab15-226">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="4ab15-227">Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="4ab15-227">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="4ab15-228">Geçerli dizin ve içerik kök</span><span class="sxs-lookup"><span data-stu-id="4ab15-228">Current directory and content root</span></span>

<span data-ttu-id="4ab15-229">Geçerli çalışma dizini çağırarak döndürülen <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör.</span><span class="sxs-lookup"><span data-stu-id="4ab15-229">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="4ab15-230">*System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum.</span><span class="sxs-lookup"><span data-stu-id="4ab15-230">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="4ab15-231">Korumak ve bir hizmetin varlıklar ve ayar dosyaları erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4ab15-231">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="4ab15-232">Kullanım ContentRootPath veya ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="4ab15-232">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="4ab15-233">Kullanım [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> uygulamanın kaynaklar bulunamıyor.</span><span class="sxs-lookup"><span data-stu-id="4ab15-233">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="4ab15-234">Uygulamanın klasör için içerik kök yolu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="4ab15-234">Set the content root path to the app's folder</span></span>

<span data-ttu-id="4ab15-235"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Sağlanan aynı yol `binPath` hizmet oluşturulduğunda bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="4ab15-235">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="4ab15-236">Çağırmak yerine `GetCurrentDirectory` ayarları dosyalara olan yolları oluşturmak için arama <xref:System.IO.Directory.SetCurrentDirectory*> ile içerik uygulamanın kök yolu.</span><span class="sxs-lookup"><span data-stu-id="4ab15-236">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="4ab15-237">İçinde `Program.Main`, hizmetin yürütülebilir dosya klasörü yolunu belirlemek ve uygulamanın içerik kök'kurmak için yolunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="4ab15-237">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="4ab15-238">Disk üzerinde uygun bir konumda bir hizmetin dosyaları Store</span><span class="sxs-lookup"><span data-stu-id="4ab15-238">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="4ab15-239">Mutlak bir yol belirtin <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> kullanırken bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> dosyaları içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="4ab15-239">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ab15-240">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4ab15-240">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="4ab15-241">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="4ab15-241">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="4ab15-242">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="4ab15-242">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
