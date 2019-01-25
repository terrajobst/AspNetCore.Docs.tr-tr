---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: eedaf64710506f2a2aac65c178a9888d2ab33d38
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837487"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="abbd8-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="abbd8-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="abbd8-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="abbd8-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="abbd8-105">ASP.NET Core uygulaması Windows barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) IIS kullanmadan.</span><span class="sxs-lookup"><span data-stu-id="abbd8-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="abbd8-106">Bir Windows hizmeti olarak barındırıldığında, uygulama yeniden başlatma sonrasında otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="abbd8-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="abbd8-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="abbd8-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="abbd8-108">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="abbd8-108">Deployment type</span></span>

<span data-ttu-id="abbd8-109">Her iki framework bağımlı veya kendi içinde Windows hizmet dağıtımı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abbd8-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="abbd8-110">Bilgi ve dağıtım senaryoları hakkında daha fazla öneri için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="abbd8-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="abbd8-111">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="abbd8-111">Framework-dependent deployment</span></span>

<span data-ttu-id="abbd8-112">.NET Core hedef sistemdeki bir paylaşılan sistem genelinde sürüm varlığını Framework bağımlı dağıtım (FDD) kullanır.</span><span class="sxs-lookup"><span data-stu-id="abbd8-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="abbd8-113">FDD senaryo ile bir ASP.NET Core Windows hizmeti uygulaması kullanıldığında, SDK'sı bir çalıştırılabilir dosyası oluşturur (*\*.exe*) adlı bir *framework bağımlı yürütülebilir*.</span><span class="sxs-lookup"><span data-stu-id="abbd8-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="abbd8-114">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="abbd8-114">Self-contained deployment</span></span>

<span data-ttu-id="abbd8-115">Hedef sistemdeki paylaşılan Bileşenler'in müstakil dağıtım (SCD) içermez.</span><span class="sxs-lookup"><span data-stu-id="abbd8-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="abbd8-116">Çalışma zamanı ve uygulamanın bağımlılıklarını, barındıran sistemde uygulamaya ile dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="abbd8-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="abbd8-117">Bir Windows hizmetinde bir projeyi dönüştürmeye</span><span class="sxs-lookup"><span data-stu-id="abbd8-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="abbd8-118">Uygulamayı bir hizmet olarak çalıştırmak için mevcut bir ASP.NET Core projesi için aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="abbd8-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="abbd8-119">Proje dosyası güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="abbd8-119">Project file updates</span></span>

<span data-ttu-id="abbd8-120">Tercih ettiğiniz tabanlı [dağıtım türü](#deployment-type), proje dosyasını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="abbd8-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="abbd8-121">Framework bağımlı dağıtım (FDD)</span><span class="sxs-lookup"><span data-stu-id="abbd8-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="abbd8-122">Bir Windows ekleme [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) için `<PropertyGroup>` , hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="abbd8-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="abbd8-123">Aşağıdaki örnekte, RID kümesine `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-123">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="abbd8-124">Ekleme `<SelfContained>` özelliğini `false`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-124">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="abbd8-125">Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin (*.exe*) için Windows dosyası.</span><span class="sxs-lookup"><span data-stu-id="abbd8-125">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="abbd8-126">A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz.</span><span class="sxs-lookup"><span data-stu-id="abbd8-126">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="abbd8-127">Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-127">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="abbd8-128">Ekleme `<UseAppHost>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-128">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="abbd8-129">Bu özellik etkinleştirme yolu hizmetiyle sağlar (bir yürütülebilir dosya *.exe*) bir FDD için.</span><span class="sxs-lookup"><span data-stu-id="abbd8-129">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="abbd8-130">Kendi başına dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="abbd8-130">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="abbd8-131">Bir Windows varlığını onaylamak [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) veya eklemek için bir RID `<PropertyGroup>` , hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="abbd8-131">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="abbd8-132">Oluşturulmasını devre dışı bir *web.config* ekleyerek dosya `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-132">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="abbd8-133">İçin birden fazla RID yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="abbd8-133">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="abbd8-134">RID noktalı virgülle ayrılmış bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="abbd8-134">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="abbd8-135">Özellik adını kullanan `<RuntimeIdentifiers>` (çoğul).</span><span class="sxs-lookup"><span data-stu-id="abbd8-135">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="abbd8-136">Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="abbd8-136">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="abbd8-137">İçin bir paket başvurusu ekleme [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="abbd8-137">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="abbd8-138">Windows olay günlüğü günlük kaydını etkinleştirmek için paket başvurusu ekleyin [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="abbd8-138">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="abbd8-139">Daha fazla bilgi için [başlatılması ve durdurulması olaylarını işlemek](#handle-starting-and-stopping-events) bölümü.</span><span class="sxs-lookup"><span data-stu-id="abbd8-139">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="abbd8-140">Program.Main güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="abbd8-140">Program.Main updates</span></span>

<span data-ttu-id="abbd8-141">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="abbd8-141">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="abbd8-142">Test ve hizmet dışında çalışırken hata ayıklama için uygulamayı bir hizmet veya bir konsol uygulaması olarak çalışıp çalışmadığını belirlemek için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="abbd8-142">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="abbd8-143">Hata ayıklayıcıyı eklediyseniz veya inceleyin `--console` komut satırı bağımsız değişkeni varsa.</span><span class="sxs-lookup"><span data-stu-id="abbd8-143">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="abbd8-144">İki koşuldan birinin (uygulamayı değil çalıştırma hizmet olarak) true ise, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> Web ana bilgisayarı üzerinde.</span><span class="sxs-lookup"><span data-stu-id="abbd8-144">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="abbd8-145">Koşullar (uygulama, hizmet olarak çalıştırıldığında) false olduğunda:</span><span class="sxs-lookup"><span data-stu-id="abbd8-145">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="abbd8-146">Çağrı <xref:System.IO.Directory.SetCurrentDirectory*> ve uygulamanın yayımlanmış konumuna bir yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="abbd8-146">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="abbd8-147">Remove() çağırmayın <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti uygulaması döndürüldüğünden yolunu almak için *C:\\WINDOWS\\system32* klasör zaman `GetCurrentDirectory` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="abbd8-147">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="abbd8-148">Daha fazla bilgi için [geçerli dizin ve içerik kök](#current-directory-and-content-root) bölümü.</span><span class="sxs-lookup"><span data-stu-id="abbd8-148">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="abbd8-149">Çağrı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulamasını bir hizmet olarak çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="abbd8-149">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="abbd8-150">Çünkü [komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirir `--console` anahtarı bağımsız değişkenlerden önce kaldırılır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bunları alır.</span><span class="sxs-lookup"><span data-stu-id="abbd8-150">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="abbd8-151">Windows olay günlüğüne yazmak için olay günlüğü Sağlayıcısı Ekle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="abbd8-151">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="abbd8-152">İle günlük tutma düzeyini ayarlamaya `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="abbd8-152">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="abbd8-153">Tanıtım ve test amacıyla örnek uygulamanın üretim ayarları dosyası günlüğe kaydetme düzeyini ayarlar `Information`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-153">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="abbd8-154">Üretim ortamında genellikle değerine `Error`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-154">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="abbd8-155">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="abbd8-155">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="abbd8-156">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="abbd8-156">Publish the app</span></span>

<span data-ttu-id="abbd8-157">Kullanarak uygulama yayımlamayı [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish), [Visual Studio yayımlama profilini](xref:host-and-deploy/visual-studio-publish-profiles), veya Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="abbd8-157">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="abbd8-158">Visual Studio kullanırken **FolderProfile** ve yapılandırma **hedef konum** seçmeden önce **Yayımla** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="abbd8-158">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="abbd8-159">Komut satırı arabirimi (CLI) araçlarını kullanarak örnek uygulamayı yayımlamak için çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) geçirilen bir sürüm yapılandırması ile proje klasöründeki bir komut isteminde komutunu [- c |--yapılandırma](/dotnet/core/tools/dotnet-publish#options)seçeneği.</span><span class="sxs-lookup"><span data-stu-id="abbd8-159">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="abbd8-160">Kullanım [-o |--çıktı](/dotnet/core/tools/dotnet-publish#options) uygulama dışında bir klasöre yayımlamak için bir yol ile seçeneği.</span><span class="sxs-lookup"><span data-stu-id="abbd8-160">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="abbd8-161">Framework bağımlı dağıtım (FDD) yayımlama</span><span class="sxs-lookup"><span data-stu-id="abbd8-161">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="abbd8-162">Aşağıdaki örnekte, uygulama için yayımlanan *c:\\svc* klasörü:</span><span class="sxs-lookup"><span data-stu-id="abbd8-162">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="abbd8-163">Kendi içinde bir dağıtım (SCD) yayımlama</span><span class="sxs-lookup"><span data-stu-id="abbd8-163">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="abbd8-164">RID belirtilmelidir `<RuntimeIdenfifier>` (veya `<RuntimeIdentifiers>`) özelliği proje dosyasının.</span><span class="sxs-lookup"><span data-stu-id="abbd8-164">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="abbd8-165">Çalışma zamanı kaynağı [- r |--çalışma zamanı](/dotnet/core/tools/dotnet-publish#options) seçeneği `dotnet publish` komutu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-165">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="abbd8-166">Aşağıdaki örnekte, uygulama için yayımlanan `win7-x64` çalışma zamanına *c:\\svc* klasörü:</span><span class="sxs-lookup"><span data-stu-id="abbd8-166">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="abbd8-167">Bir kullanıcı hesabı oluşturun</span><span class="sxs-lookup"><span data-stu-id="abbd8-167">Create a user account</span></span>

<span data-ttu-id="abbd8-168">Hizmet kullanımı için bir kullanıcı hesabı oluşturma `net user` komutu:</span><span class="sxs-lookup"><span data-stu-id="abbd8-168">Create a user account for the service using the `net user` command:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="abbd8-169">Örnek uygulama için bir kullanıcı hesabı adı ile oluşturun. `ServiceUser` ve parola.</span><span class="sxs-lookup"><span data-stu-id="abbd8-169">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="abbd8-170">Aşağıdaki komutta `{PASSWORD}` ile bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="abbd8-170">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="abbd8-171">Bir gruba kullanıcı eklemeniz gerekiyorsa, kullanın `net localgroup` komutu, burada `{GROUP}` grubunun adıdır:</span><span class="sxs-lookup"><span data-stu-id="abbd8-171">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="abbd8-172">Daha fazla bilgi için [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="abbd8-172">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="abbd8-173">İzinleri ayarlama</span><span class="sxs-lookup"><span data-stu-id="abbd8-173">Set permissions</span></span>

<span data-ttu-id="abbd8-174">Uygulamanın klasörüne yazma/okuma/yürütme erişimi vermek kullanarak [icacls](/windows-server/administration/windows-commands/icacls) komutu:</span><span class="sxs-lookup"><span data-stu-id="abbd8-174">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="abbd8-175">`{PATH}` &ndash; Uygulamanın klasörün yolu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-175">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="abbd8-176">`{USER ACCOUNT}` &ndash; Kullanıcı hesabı (SID).</span><span class="sxs-lookup"><span data-stu-id="abbd8-176">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="abbd8-177">`(OI)` &ndash; Nesne devral bayrağı dosyaları alt izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="abbd8-177">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="abbd8-178">`(CI)` &ndash; Kapsayıcı devral bayrağı, alt klasörler için izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="abbd8-178">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="abbd8-179">`{PERMISSION FLAGS}` &ndash; Uygulamanın erişim izinlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="abbd8-179">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="abbd8-180">Yazma (`W`)</span><span class="sxs-lookup"><span data-stu-id="abbd8-180">Write (`W`)</span></span>
  * <span data-ttu-id="abbd8-181">Okuma (`R`)</span><span class="sxs-lookup"><span data-stu-id="abbd8-181">Read (`R`)</span></span>
  * <span data-ttu-id="abbd8-182">Yürütme (`X`)</span><span class="sxs-lookup"><span data-stu-id="abbd8-182">Execute (`X`)</span></span>
  * <span data-ttu-id="abbd8-183">Tam (`F`)</span><span class="sxs-lookup"><span data-stu-id="abbd8-183">Full (`F`)</span></span>
  * <span data-ttu-id="abbd8-184">Değiştir (`M`)</span><span class="sxs-lookup"><span data-stu-id="abbd8-184">Modify (`M`)</span></span>
* <span data-ttu-id="abbd8-185">`/t` &ndash; Yinelemeli olarak mevcut alt klasörler ve dosyalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="abbd8-185">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="abbd8-186">Örnek uygulamayı yayımlanan *c:\\svc* klasörü ve `ServiceUser` hesap yazma/okuma/Yürütme izinleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="abbd8-186">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="abbd8-187">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="abbd8-187">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="abbd8-188">Hizmeti yönetme</span><span class="sxs-lookup"><span data-stu-id="abbd8-188">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="abbd8-189">Hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="abbd8-189">Create the service</span></span>

<span data-ttu-id="abbd8-190">Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="abbd8-190">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="abbd8-191">`binPath` Değerdir yürütülebilir dosya adını içeren uygulamanın yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-191">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="abbd8-192">**Eşittir işareti ve tırnak karakteri her bir parametre ve değer arasında gerekli bir alandır.**</span><span class="sxs-lookup"><span data-stu-id="abbd8-192">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="abbd8-193">`{SERVICE NAME}` &ndash; Hizmete atanacak ad [Hizmet Denetimi Yöneticisi](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="abbd8-193">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="abbd8-194">`{PATH}` &ndash; Hizmet yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-194">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="abbd8-195">`{DOMAIN}` &ndash; Etki alanına katılmış bir makine etki alanı.</span><span class="sxs-lookup"><span data-stu-id="abbd8-195">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="abbd8-196">Makine etki alanına katılmış değilse yerel makine adını.</span><span class="sxs-lookup"><span data-stu-id="abbd8-196">If the machine isn't domain-joined, the local machine name.</span></span>
* <span data-ttu-id="abbd8-197">`{USER ACCOUNT}` &ndash; Hizmetinin çalıştığı kullanıcı hesabı.</span><span class="sxs-lookup"><span data-stu-id="abbd8-197">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="abbd8-198">`{PASSWORD}` &ndash; Kullanıcı hesabı parolası.</span><span class="sxs-lookup"><span data-stu-id="abbd8-198">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="abbd8-199">Yapmak **değil** atlamak `obj` parametresi.</span><span class="sxs-lookup"><span data-stu-id="abbd8-199">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="abbd8-200">İçin varsayılan değer `obj` olduğu [LocalSystem hesabı](/windows/desktop/services/localsystem-account) hesabı.</span><span class="sxs-lookup"><span data-stu-id="abbd8-200">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="abbd8-201">Altında bir hizmeti çalıştıran `LocalSystem` hesabı önemli bir güvenlik riski sunar.</span><span class="sxs-lookup"><span data-stu-id="abbd8-201">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="abbd8-202">Her zaman bir servis ayrıcalıkları sınırlı sahip bir kullanıcı hesabı ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="abbd8-202">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="abbd8-203">Aşağıdaki örnekte örnek uygulama için:</span><span class="sxs-lookup"><span data-stu-id="abbd8-203">In the following example for the sample app:</span></span>

* <span data-ttu-id="abbd8-204">Adlı hizmetin **MyService**.</span><span class="sxs-lookup"><span data-stu-id="abbd8-204">The service is named **MyService**.</span></span>
* <span data-ttu-id="abbd8-205">Yayınlanan hizmet bulunan *c:\\svc* klasör.</span><span class="sxs-lookup"><span data-stu-id="abbd8-205">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="abbd8-206">Uygulama yürütülebilir dosyası adlı *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="abbd8-206">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="abbd8-207">İçine `binPath` çift tırnak (") değeri.</span><span class="sxs-lookup"><span data-stu-id="abbd8-207">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="abbd8-208">Altında çalışacağı `ServiceUser` hesabı.</span><span class="sxs-lookup"><span data-stu-id="abbd8-208">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="abbd8-209">Değiştirin `{DOMAIN}` kullanıcı hesabının etki alanı veya yerel makine adı.</span><span class="sxs-lookup"><span data-stu-id="abbd8-209">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="abbd8-210">İçine `obj` çift tırnak (") değeri.</span><span class="sxs-lookup"><span data-stu-id="abbd8-210">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="abbd8-211">Örnek: Barındıran sistemde adlı bir yerel makineye ise `MairaPC`ayarlayın `obj` için `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-211">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="abbd8-212">Değiştirin `{PASSWORD}` ile kullanıcı hesabının parolası.</span><span class="sxs-lookup"><span data-stu-id="abbd8-212">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="abbd8-213">İçine `password` çift tırnak (") değeri.</span><span class="sxs-lookup"><span data-stu-id="abbd8-213">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="abbd8-214">Parametreleri eşittir işareti ve parametrelerin değerleri arasında boşluk bulunmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="abbd8-214">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="abbd8-215">Hizmeti Başlat</span><span class="sxs-lookup"><span data-stu-id="abbd8-215">Start the service</span></span>

<span data-ttu-id="abbd8-216">Hizmetle başlar `sc start {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-216">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="abbd8-217">Örnek uygulama hizmeti başlatmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="abbd8-217">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="abbd8-218">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="abbd8-218">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="abbd8-219">Hizmet durumunu belirleme</span><span class="sxs-lookup"><span data-stu-id="abbd8-219">Determine the service status</span></span>

<span data-ttu-id="abbd8-220">Hizmet durumunu denetlemek için kullanmak `sc query {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-220">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="abbd8-221">Durumu aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="abbd8-221">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="abbd8-222">Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="abbd8-222">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="abbd8-223">Bir web app service Gözat</span><span class="sxs-lookup"><span data-stu-id="abbd8-223">Browse a web app service</span></span>

<span data-ttu-id="abbd8-224">Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama, bir yola göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendiren `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi ara yazılım](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="abbd8-224">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="abbd8-225">Örnek app service için uygulamaya Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="abbd8-225">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="abbd8-226">Hizmeti Durdur</span><span class="sxs-lookup"><span data-stu-id="abbd8-226">Stop the service</span></span>

<span data-ttu-id="abbd8-227">Hizmetle Durdur `sc stop {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-227">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="abbd8-228">Aşağıdaki komut örnek uygulama hizmetini durdurur:</span><span class="sxs-lookup"><span data-stu-id="abbd8-228">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="abbd8-229">Hizmeti Sil</span><span class="sxs-lookup"><span data-stu-id="abbd8-229">Delete the service</span></span>

<span data-ttu-id="abbd8-230">Hizmeti ile bir hizmeti durdurmak için bir kısa bir gecikmeyle kaldırmanız `sc delete {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-230">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="abbd8-231">Örnek uygulama hizmeti durumunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="abbd8-231">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="abbd8-232">Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmeti kaldırmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="abbd8-232">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="abbd8-233">Başlatma ve durdurma olayları işleme</span><span class="sxs-lookup"><span data-stu-id="abbd8-233">Handle starting and stopping events</span></span>

<span data-ttu-id="abbd8-234">İşlenecek <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olayları, aşağıdaki ek değişiklikleri gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="abbd8-234">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="abbd8-235">Türetilen bir sınıf oluşturmanız <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> ile `OnStarting`, `OnStarted`, ve `OnStopping` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="abbd8-235">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="abbd8-236">Bir genişletme yöntemi için oluşturma <xref:Microsoft.AspNetCore.Hosting.IWebHost> Geçiren `CustomWebHostService` için <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="abbd8-236">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="abbd8-237">İçinde `Program.Main`, çağrı `RunAsCustomService` genişletme yöntemi yerine <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="abbd8-237">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="abbd8-238">Konumu görmek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> içinde `Program.Main`, gösterilen kod örneği başvurmak [bir Windows hizmetinde bir projeyi dönüştürmeye](#convert-a-project-into-a-windows-service) bölümü.</span><span class="sxs-lookup"><span data-stu-id="abbd8-238">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="abbd8-239">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="abbd8-239">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="abbd8-240">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="abbd8-240">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="abbd8-241">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="abbd8-241">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="abbd8-242">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="abbd8-242">Configure HTTPS</span></span>

<span data-ttu-id="abbd8-243">Hizmet güvenli bir uç nokta ile yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="abbd8-243">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="abbd8-244">Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="abbd8-244">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="abbd8-245">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="abbd8-245">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="abbd8-246">Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="abbd8-246">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="abbd8-247">Geçerli dizin ve içerik kök</span><span class="sxs-lookup"><span data-stu-id="abbd8-247">Current directory and content root</span></span>

<span data-ttu-id="abbd8-248">Geçerli çalışma dizini çağırarak döndürülen <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör.</span><span class="sxs-lookup"><span data-stu-id="abbd8-248">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="abbd8-249">*System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum.</span><span class="sxs-lookup"><span data-stu-id="abbd8-249">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="abbd8-250">Korumak ve bir hizmetin varlıklar ve ayar dosyaları erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="abbd8-250">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="abbd8-251">Uygulamanın klasör için içerik kök yolu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="abbd8-251">Set the content root path to the app's folder</span></span>

<span data-ttu-id="abbd8-252"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Sağlanan aynı yol `binPath` hizmeti oluşturulduğunda bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="abbd8-252">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="abbd8-253">Çağırmak yerine `GetCurrentDirectory` ayarları dosyalara olan yolları oluşturmak için arama <xref:System.IO.Directory.SetCurrentDirectory*> ile içerik uygulamanın kök yolu.</span><span class="sxs-lookup"><span data-stu-id="abbd8-253">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="abbd8-254">İçinde `Program.Main`, hizmetin yürütülebilir dosya klasörü yolunu belirlemek ve uygulamanın içerik kök'kurmak için yolunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="abbd8-254">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="abbd8-255">Disk üzerinde uygun bir konumda hizmetin dosyaları Store</span><span class="sxs-lookup"><span data-stu-id="abbd8-255">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="abbd8-256">Mutlak bir yol belirtin <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> kullanırken bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> dosyaları içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="abbd8-256">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="abbd8-257">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="abbd8-257">Additional resources</span></span>

* <span data-ttu-id="abbd8-258">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="abbd8-258">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
