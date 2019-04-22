---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068342"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="b4830-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="b4830-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="b4830-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b4830-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b4830-105">ASP.NET Core uygulaması Windows barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) IIS kullanmadan.</span><span class="sxs-lookup"><span data-stu-id="b4830-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="b4830-106">Bir Windows hizmeti olarak barındırıldığında, uygulama yeniden başlatma sonrasında otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="b4830-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="b4830-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b4830-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4830-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b4830-108">Prerequisites</span></span>

* [<span data-ttu-id="b4830-109">PowerShell 6.2 veya üstü</span><span class="sxs-lookup"><span data-stu-id="b4830-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> <span data-ttu-id="b4830-110">Windows 10 Ekim 2018'den önceki Windows işletim sistemi için Update (sürüm 1809/derleme 10.0.17763) [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) modülü içeri ile [WindowsCompatibility Modülü](https://github.com/PowerShell/WindowsCompatibility)erişim kazanmak için [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) kullanılan cmdlet'i [bir kullanıcı hesabı oluşturma](#create-a-user-account) bölümü:</span><span class="sxs-lookup"><span data-stu-id="b4830-110">For Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763), the [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) module must be imported with the [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) to gain access to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet used in the [Create a user account](#create-a-user-account) section:</span></span>
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a><span data-ttu-id="b4830-111">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="b4830-111">Deployment type</span></span>

<span data-ttu-id="b4830-112">Her iki framework bağımlı veya kendi içinde Windows hizmet dağıtımı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b4830-112">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="b4830-113">Bilgi ve dağıtım senaryoları hakkında daha fazla öneri için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="b4830-113">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="b4830-114">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="b4830-114">Framework-dependent deployment</span></span>

<span data-ttu-id="b4830-115">.NET Core hedef sistemdeki bir paylaşılan sistem genelinde sürüm varlığını Framework bağımlı dağıtım (FDD) kullanır.</span><span class="sxs-lookup"><span data-stu-id="b4830-115">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="b4830-116">FDD senaryo ile bir ASP.NET Core Windows hizmeti uygulaması kullanıldığında, SDK'sı bir çalıştırılabilir dosyası oluşturur (*\*.exe*) adlı bir *framework bağımlı yürütülebilir*.</span><span class="sxs-lookup"><span data-stu-id="b4830-116">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="b4830-117">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="b4830-117">Self-contained deployment</span></span>

<span data-ttu-id="b4830-118">Hedef sistemdeki paylaşılan Bileşenler'in müstakil dağıtım (SCD) içermez.</span><span class="sxs-lookup"><span data-stu-id="b4830-118">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="b4830-119">Çalışma zamanı ve uygulamanın bağımlılıklarını, barındıran sistemde uygulamaya ile dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="b4830-119">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="b4830-120">Bir Windows hizmetinde bir projeyi dönüştürmeye</span><span class="sxs-lookup"><span data-stu-id="b4830-120">Convert a project into a Windows Service</span></span>

<span data-ttu-id="b4830-121">Uygulamayı bir hizmet olarak çalıştırmak için mevcut bir ASP.NET Core projesi için aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="b4830-121">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="b4830-122">Proje dosyası güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="b4830-122">Project file updates</span></span>

<span data-ttu-id="b4830-123">Tercih ettiğiniz tabanlı [dağıtım türü](#deployment-type), proje dosyasını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b4830-123">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="b4830-124">Framework bağımlı dağıtım (FDD)</span><span class="sxs-lookup"><span data-stu-id="b4830-124">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="b4830-125">Bir Windows ekleme [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) için `<PropertyGroup>` , hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="b4830-125">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="b4830-126">Aşağıdaki örnekte, RID kümesine `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="b4830-126">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="b4830-127">Ekleme `<SelfContained>` özelliğini `false`.</span><span class="sxs-lookup"><span data-stu-id="b4830-127">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="b4830-128">Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin (*.exe*) için Windows dosyası.</span><span class="sxs-lookup"><span data-stu-id="b4830-128">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="b4830-129">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="b4830-129">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="b4830-130">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="b4830-130">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

<span data-ttu-id="b4830-131">Ekleme `<UseAppHost>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="b4830-131">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="b4830-132">Bu özellik etkinleştirme yolu hizmetiyle sağlar (bir yürütülebilir dosya *.exe*) bir FDD için.</span><span class="sxs-lookup"><span data-stu-id="b4830-132">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="b4830-133">Kendi başına dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="b4830-133">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="b4830-134">Bir Windows varlığını onaylamak [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) veya eklemek için bir RID `<PropertyGroup>` , hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="b4830-134">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="b4830-135">Oluşturulmasını devre dışı bir *web.config* ekleyerek dosya `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="b4830-135">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="b4830-136">İçin birden fazla RID yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="b4830-136">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="b4830-137">RID noktalı virgülle ayrılmış bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="b4830-137">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="b4830-138">Özellik adını kullanan `<RuntimeIdentifiers>` (çoğul).</span><span class="sxs-lookup"><span data-stu-id="b4830-138">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="b4830-139">Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="b4830-139">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="b4830-140">İçin bir paket başvurusu ekleme [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="b4830-140">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="b4830-141">Windows olay günlüğü günlük kaydını etkinleştirmek için paket başvurusu ekleyin [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="b4830-141">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="b4830-142">Daha fazla bilgi için [başlatılması ve durdurulması olaylarını işlemek](#handle-starting-and-stopping-events) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b4830-142">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="b4830-143">Program.Main güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="b4830-143">Program.Main updates</span></span>

<span data-ttu-id="b4830-144">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b4830-144">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="b4830-145">Test ve hizmet dışında çalışırken hata ayıklama için uygulamayı bir hizmet veya bir konsol uygulaması olarak çalışıp çalışmadığını belirlemek için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b4830-145">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="b4830-146">Hata ayıklayıcıyı eklediyseniz veya inceleyin `--console` komut satırı bağımsız değişkeni varsa.</span><span class="sxs-lookup"><span data-stu-id="b4830-146">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="b4830-147">İki koşuldan birinin (uygulamayı değil çalıştırma hizmet olarak) true ise, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> Web ana bilgisayarı üzerinde.</span><span class="sxs-lookup"><span data-stu-id="b4830-147">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="b4830-148">Koşullar (uygulama, hizmet olarak çalıştırıldığında) false olduğunda:</span><span class="sxs-lookup"><span data-stu-id="b4830-148">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="b4830-149">Çağrı <xref:System.IO.Directory.SetCurrentDirectory*> ve uygulamanın yayımlanmış konumuna bir yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="b4830-149">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="b4830-150">Remove() çağırmayın <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti uygulaması döndürüldüğünden yolunu almak için *C:\\WINDOWS\\system32* klasör zaman <xref:System.IO.Directory.GetCurrentDirectory*> çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b4830-150">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="b4830-151">Daha fazla bilgi için [geçerli dizin ve içerik kök](#current-directory-and-content-root) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b4830-151">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="b4830-152">Çağrı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulamasını bir hizmet olarak çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="b4830-152">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="b4830-153">Çünkü [komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirir `--console` anahtarı bağımsız değişkenlerden önce kaldırılır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bunları alır.</span><span class="sxs-lookup"><span data-stu-id="b4830-153">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="b4830-154">Windows olay günlüğüne yazmak için olay günlüğü Sağlayıcısı Ekle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="b4830-154">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="b4830-155">İle günlük tutma düzeyini ayarlamaya `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="b4830-155">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="b4830-156">Tanıtım ve test amacıyla örnek uygulamanın üretim ayarları dosyası günlüğe kaydetme düzeyini ayarlar `Information`.</span><span class="sxs-lookup"><span data-stu-id="b4830-156">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="b4830-157">Üretim ortamında genellikle değerine `Error`.</span><span class="sxs-lookup"><span data-stu-id="b4830-157">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="b4830-158">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="b4830-158">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="b4830-159">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="b4830-159">Publish the app</span></span>

<span data-ttu-id="b4830-160">Kullanarak uygulama yayımlamayı [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish), [Visual Studio yayımlama profilini](xref:host-and-deploy/visual-studio-publish-profiles), veya Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b4830-160">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="b4830-161">Visual Studio kullanırken **FolderProfile** ve yapılandırma **hedef konum** seçmeden önce **Yayımla** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b4830-161">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="b4830-162">Komut satırı arabirimi (CLI) araçlarını kullanarak örnek uygulamayı yayımlamak için çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) proje klasöründeki geçirilen bir yayın yapılandırmasına sahip bir Windows komut kabuğu komutunu [- c |--yapılandırma ](/dotnet/core/tools/dotnet-publish#options) seçeneği.</span><span class="sxs-lookup"><span data-stu-id="b4830-162">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command in a Windows command shell from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="b4830-163">Kullanım [-o |--çıktı](/dotnet/core/tools/dotnet-publish#options) uygulama dışında bir klasöre yayımlamak için bir yol ile seçeneği.</span><span class="sxs-lookup"><span data-stu-id="b4830-163">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="b4830-164">Framework bağımlı dağıtım (FDD) yayımlama</span><span class="sxs-lookup"><span data-stu-id="b4830-164">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="b4830-165">Aşağıdaki örnekte, uygulama için yayımlanan *c:\\svc* klasörü:</span><span class="sxs-lookup"><span data-stu-id="b4830-165">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="b4830-166">Kendi içinde bir dağıtım (SCD) yayımlama</span><span class="sxs-lookup"><span data-stu-id="b4830-166">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="b4830-167">RID belirtilmelidir `<RuntimeIdenfifier>` (veya `<RuntimeIdentifiers>`) özelliği proje dosyasının.</span><span class="sxs-lookup"><span data-stu-id="b4830-167">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="b4830-168">Çalışma zamanı kaynağı [- r |--çalışma zamanı](/dotnet/core/tools/dotnet-publish#options) seçeneği `dotnet publish` komutu.</span><span class="sxs-lookup"><span data-stu-id="b4830-168">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="b4830-169">Aşağıdaki örnekte, uygulama için yayımlanan `win7-x64` çalışma zamanına *c:\\svc* klasörü:</span><span class="sxs-lookup"><span data-stu-id="b4830-169">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="b4830-170">Bir kullanıcı hesabı oluşturun</span><span class="sxs-lookup"><span data-stu-id="b4830-170">Create a user account</span></span>

<span data-ttu-id="b4830-171">Hizmet kullanımı için bir kullanıcı hesabı oluşturma [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet'i bir yönetim PowerShell 6'yı komut kabuğundan:</span><span class="sxs-lookup"><span data-stu-id="b4830-171">Create a user account for the service using the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell:</span></span>

```powershell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="b4830-172">Sağlayan bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) istendiğinde.</span><span class="sxs-lookup"><span data-stu-id="b4830-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="b4830-173">Örnek uygulama için bir kullanıcı hesabı adı ile oluşturun. `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="b4830-173">For the sample app, create a user account with the name `ServiceUser`.</span></span>

```powershell
New-LocalUser -Name ServiceUser
```

<span data-ttu-id="b4830-174">Sürece `-AccountExpires` parametre tarafından sağlanan [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) bir sona erme cmdlet'iyle <xref:System.DateTime>, hesabın süresi sona ermiyor.</span><span class="sxs-lookup"><span data-stu-id="b4830-174">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="b4830-175">Daha fazla bilgi için [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="b4830-175">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="b4830-176">Active Directory kullanarak kullanıcıları yönetme için alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="b4830-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="b4830-177">Daha fazla bilgi için [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="b4830-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="b4830-178">İzin ayarlama: Bir hizmet olarak oturum aç</span><span class="sxs-lookup"><span data-stu-id="b4830-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="b4830-179">Uygulamanın klasörüne yazma/okuma/yürütme erişimi vermek kullanarak [icacls](/windows-server/administration/windows-commands/icacls) bir yönetici PowerShell 6'yı komut kabuğu komut.</span><span class="sxs-lookup"><span data-stu-id="b4830-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* <span data-ttu-id="b4830-180">`{PATH}` &ndash; Uygulamanın klasörün yolu.</span><span class="sxs-lookup"><span data-stu-id="b4830-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="b4830-181">`{USER ACCOUNT}` &ndash; Kullanıcı hesabı (SID).</span><span class="sxs-lookup"><span data-stu-id="b4830-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="b4830-182">`(OI)` &ndash; Nesne devral bayrağı dosyaları alt izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="b4830-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="b4830-183">`(CI)` &ndash; Kapsayıcı devral bayrağı, alt klasörler için izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="b4830-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="b4830-184">`{PERMISSION FLAGS}` &ndash; Uygulamanın erişim izinlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b4830-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="b4830-185">Yazma (`W`)</span><span class="sxs-lookup"><span data-stu-id="b4830-185">Write (`W`)</span></span>
  * <span data-ttu-id="b4830-186">Okuma (`R`)</span><span class="sxs-lookup"><span data-stu-id="b4830-186">Read (`R`)</span></span>
  * <span data-ttu-id="b4830-187">Yürütme (`X`)</span><span class="sxs-lookup"><span data-stu-id="b4830-187">Execute (`X`)</span></span>
  * <span data-ttu-id="b4830-188">Tam (`F`)</span><span class="sxs-lookup"><span data-stu-id="b4830-188">Full (`F`)</span></span>
  * <span data-ttu-id="b4830-189">Değiştir (`M`)</span><span class="sxs-lookup"><span data-stu-id="b4830-189">Modify (`M`)</span></span>
* <span data-ttu-id="b4830-190">`/t` &ndash; Yinelemeli olarak mevcut alt klasörler ve dosyalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b4830-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="b4830-191">Örnek uygulamayı yayımlanan *c:\\svc* klasörü ve `ServiceUser` hesap yazma/okuma/Yürütme izinleri ile bir yönetici PowerShell 6'yı komut kabuğuna şu komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="b4830-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

<span data-ttu-id="b4830-192">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="b4830-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="b4830-193">Hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="b4830-193">Create the service</span></span>

<span data-ttu-id="b4830-194">Kullanım [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) hizmeti kaydetmek için PowerShell Betiği.</span><span class="sxs-lookup"><span data-stu-id="b4830-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="b4830-195">Bir yönetici PowerShell 6'yı komut kabuğundan betiği aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="b4830-195">From an administrative PowerShell 6 command shell, execute the script with the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

<span data-ttu-id="b4830-196">Aşağıdaki örnekte örnek uygulama için:</span><span class="sxs-lookup"><span data-stu-id="b4830-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="b4830-197">Adlı hizmetin **MyService**.</span><span class="sxs-lookup"><span data-stu-id="b4830-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="b4830-198">Yayınlanan hizmet bulunan *c:\\svc* klasör.</span><span class="sxs-lookup"><span data-stu-id="b4830-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="b4830-199">Uygulama yürütülebilir dosyası adlı *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="b4830-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="b4830-200">Altında çalışacağı `ServiceUser` hesabı.</span><span class="sxs-lookup"><span data-stu-id="b4830-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="b4830-201">Aşağıdaki örnek komut yerel makine adıdır `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="b4830-201">In the following example command, the local machine name is `Desktop-PC`.</span></span> <span data-ttu-id="b4830-202">Değiştirin `Desktop-PC` bilgisayar adı veya etki alanı sistemi.</span><span class="sxs-lookup"><span data-stu-id="b4830-202">Replace `Desktop-PC` with the computer name or domain for your system.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="b4830-203">Hizmeti yönetme</span><span class="sxs-lookup"><span data-stu-id="b4830-203">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="b4830-204">Hizmeti Başlat</span><span class="sxs-lookup"><span data-stu-id="b4830-204">Start the service</span></span>

<span data-ttu-id="b4830-205">Hizmetle başlar `Start-Service -Name {NAME}` PowerShell 6 komutu.</span><span class="sxs-lookup"><span data-stu-id="b4830-205">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="b4830-206">Örnek uygulama hizmeti başlatmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="b4830-206">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="b4830-207">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="b4830-207">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="b4830-208">Hizmet durumunu belirleme</span><span class="sxs-lookup"><span data-stu-id="b4830-208">Determine the service status</span></span>

<span data-ttu-id="b4830-209">Hizmet durumunu denetlemek için kullanmak `Get-Service -Name {NAME}` PowerShell 6 komutu.</span><span class="sxs-lookup"><span data-stu-id="b4830-209">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="b4830-210">Durumu aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="b4830-210">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="b4830-211">Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="b4830-211">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="b4830-212">Bir web app service Gözat</span><span class="sxs-lookup"><span data-stu-id="b4830-212">Browse a web app service</span></span>

<span data-ttu-id="b4830-213">Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama, bir yola göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendiren `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi ara yazılım](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="b4830-213">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="b4830-214">Örnek app service için uygulamaya Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b4830-214">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="b4830-215">Hizmeti Durdur</span><span class="sxs-lookup"><span data-stu-id="b4830-215">Stop the service</span></span>

<span data-ttu-id="b4830-216">Hizmetle Durdur `Stop-Service -Name {NAME}` Powershell 6 komutu.</span><span class="sxs-lookup"><span data-stu-id="b4830-216">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="b4830-217">Aşağıdaki komut örnek uygulama hizmetini durdurur:</span><span class="sxs-lookup"><span data-stu-id="b4830-217">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="b4830-218">Hizmeti Kaldır</span><span class="sxs-lookup"><span data-stu-id="b4830-218">Remove the service</span></span>

<span data-ttu-id="b4830-219">Hizmeti ile bir hizmeti durdurmak için bir kısa bir gecikmeyle kaldırmak `Remove-Service -Name {NAME}` Powershell 6 komutu.</span><span class="sxs-lookup"><span data-stu-id="b4830-219">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="b4830-220">Örnek uygulama hizmeti durumunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="b4830-220">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="b4830-221">Başlatma ve durdurma olayları işleme</span><span class="sxs-lookup"><span data-stu-id="b4830-221">Handle starting and stopping events</span></span>

<span data-ttu-id="b4830-222">İşlenecek <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olayları, aşağıdaki ek değişiklikleri gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="b4830-222">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="b4830-223">Türetilen bir sınıf oluşturmanız <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> ile `OnStarting`, `OnStarted`, ve `OnStopping` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="b4830-223">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="b4830-224">Bir genişletme yöntemi için oluşturma <xref:Microsoft.AspNetCore.Hosting.IWebHost> Geçiren `CustomWebHostService` için <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="b4830-224">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="b4830-225">İçinde `Program.Main`, çağrı `RunAsCustomService` genişletme yöntemi yerine <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="b4830-225">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="b4830-226">Konumu görmek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> içinde `Program.Main`, gösterilen kod örneği başvurmak [bir Windows hizmetinde bir projeyi dönüştürmeye](#convert-a-project-into-a-windows-service) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b4830-226">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b4830-227">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="b4830-227">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b4830-228">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="b4830-228">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="b4830-229">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="b4830-229">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="b4830-230">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b4830-230">Configure HTTPS</span></span>

<span data-ttu-id="b4830-231">Hizmet güvenli bir uç nokta ile yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="b4830-231">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="b4830-232">Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b4830-232">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="b4830-233">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="b4830-233">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="b4830-234">Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="b4830-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="b4830-235">Geçerli dizin ve içerik kök</span><span class="sxs-lookup"><span data-stu-id="b4830-235">Current directory and content root</span></span>

<span data-ttu-id="b4830-236">Geçerli çalışma dizini çağırarak döndürülen <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör.</span><span class="sxs-lookup"><span data-stu-id="b4830-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="b4830-237">*System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum.</span><span class="sxs-lookup"><span data-stu-id="b4830-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="b4830-238">Korumak ve bir hizmetin varlıklar ve ayar dosyaları erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="b4830-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="b4830-239">Uygulamanın klasör için içerik kök yolu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="b4830-239">Set the content root path to the app's folder</span></span>

<span data-ttu-id="b4830-240"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Sağlanan aynı yol `binPath` hizmeti oluşturulduğunda bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="b4830-240">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="b4830-241">Çağırmak yerine `GetCurrentDirectory` ayarları dosyalara olan yolları oluşturmak için arama <xref:System.IO.Directory.SetCurrentDirectory*> ile içerik uygulamanın kök yolu.</span><span class="sxs-lookup"><span data-stu-id="b4830-241">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="b4830-242">İçinde `Program.Main`, hizmetin yürütülebilir dosya klasörü yolunu belirlemek ve uygulamanın içerik kök'kurmak için yolunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="b4830-242">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="b4830-243">Disk üzerinde uygun bir konumda hizmetin dosyaları Store</span><span class="sxs-lookup"><span data-stu-id="b4830-243">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="b4830-244">Mutlak bir yol belirtin <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> kullanırken bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> dosyaları içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="b4830-244">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4830-245">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b4830-245">Additional resources</span></span>

* <span data-ttu-id="b4830-246">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="b4830-246">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
