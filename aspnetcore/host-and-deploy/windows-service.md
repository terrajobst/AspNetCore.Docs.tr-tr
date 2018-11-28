---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f857e96108b68bb6ec64a85910bf4d889cdf2822
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458523"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="c8992-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="c8992-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="c8992-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c8992-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c8992-105">ASP.NET Core uygulaması Windows barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) IIS kullanmadan.</span><span class="sxs-lookup"><span data-stu-id="c8992-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="c8992-106">Bir Windows hizmeti olarak barındırıldığında, uygulama yeniden başlatma sonrasında otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="c8992-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="c8992-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8992-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="c8992-108">Dağıtım türü</span><span class="sxs-lookup"><span data-stu-id="c8992-108">Deployment type</span></span>

<span data-ttu-id="c8992-109">Her iki framework bağımlı veya kendi içinde Windows hizmet dağıtımı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c8992-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="c8992-110">Bilgi ve dağıtım senaryoları hakkında daha fazla öneri için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="c8992-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="c8992-111">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="c8992-111">Framework-dependent deployment</span></span>

<span data-ttu-id="c8992-112">.NET Core hedef sistemdeki bir paylaşılan sistem genelinde sürüm varlığını Framework bağımlı dağıtım (FDD) kullanır.</span><span class="sxs-lookup"><span data-stu-id="c8992-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="c8992-113">FDD senaryo ile bir ASP.NET Core Windows hizmeti uygulaması kullanıldığında, SDK'sı bir çalıştırılabilir dosyası oluşturur (*\*.exe*) adlı bir *framework bağımlı yürütülebilir*.</span><span class="sxs-lookup"><span data-stu-id="c8992-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="c8992-114">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="c8992-114">Self-contained deployment</span></span>

<span data-ttu-id="c8992-115">Hedef sistemdeki paylaşılan Bileşenler'in müstakil dağıtım (SCD) içermez.</span><span class="sxs-lookup"><span data-stu-id="c8992-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="c8992-116">Çalışma zamanı ve uygulamanın bağımlılıklarını, barındıran sistemde uygulamaya ile dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="c8992-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="c8992-117">Bir Windows hizmetinde bir projeyi dönüştürmeye</span><span class="sxs-lookup"><span data-stu-id="c8992-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="c8992-118">Uygulamayı bir hizmet olarak çalıştırmak için mevcut bir ASP.NET Core projesi için aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="c8992-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

1. <span data-ttu-id="c8992-119">Tercih ettiğiniz tabanlı [dağıtım türü](#deployment-type), proje dosyasını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="c8992-119">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

   * <span data-ttu-id="c8992-120">**Framework bağımlı dağıtım (FDD)** &ndash; bir Windows ekleme [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) için `<PropertyGroup>` , hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="c8992-120">**Framework-dependent Deployment (FDD)** &ndash; Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="c8992-121">Ekleme `<SelfContained>` özelliğini `false`.</span><span class="sxs-lookup"><span data-stu-id="c8992-121">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="c8992-122">Oluşturulmasını devre dışı bir *web.config* ekleyerek dosya `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="c8992-122">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <SelfContained>false</SelfContained>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     <span data-ttu-id="c8992-123">**Kendi başına dağıtım (SCD)** &ndash; bir Windows varlığını onaylamak [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) veya eklemek için bir RID `<PropertyGroup>` , hedef Framework'ü içerir.</span><span class="sxs-lookup"><span data-stu-id="c8992-123">**Self-contained Deployment (SCD)** &ndash; Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="c8992-124">Oluşturulmasını devre dışı bir *web.config* ekleyerek dosya `<IsTransformWebConfigDisabled>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="c8992-124">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     <span data-ttu-id="c8992-125">İçin birden fazla RID yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="c8992-125">To publish for multiple RIDs:</span></span>

     * <span data-ttu-id="c8992-126">RID noktalı virgülle ayrılmış bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="c8992-126">Provide the RIDs in a semicolon-delimited list.</span></span>
     * <span data-ttu-id="c8992-127">Özellik adını kullanan `<RuntimeIdentifiers>` (çoğul).</span><span class="sxs-lookup"><span data-stu-id="c8992-127">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

     <span data-ttu-id="c8992-128">Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="c8992-128">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="c8992-129">İçin bir paket başvurusu ekleme [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="c8992-129">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

   * <span data-ttu-id="c8992-130">Windows olay günlüğü günlük kaydını etkinleştirmek için paket başvurusu ekleyin [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="c8992-130">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

     <span data-ttu-id="c8992-131">Daha fazla bilgi için [başlatılması ve durdurulması olaylarını işlemek](#handle-starting-and-stopping-events) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c8992-131">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

1. <span data-ttu-id="c8992-132">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c8992-132">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="c8992-133">Test ve hizmet dışında çalışırken hata ayıklama için uygulamayı bir hizmet veya bir konsol uygulaması olarak çalışıp çalışmadığını belirlemek için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c8992-133">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="c8992-134">Hata ayıklayıcıyı eklediyseniz veya inceleyin `--console` komut satırı bağımsız değişkeni varsa.</span><span class="sxs-lookup"><span data-stu-id="c8992-134">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

     <span data-ttu-id="c8992-135">İki koşuldan birinin (uygulamayı değil çalıştırma hizmet olarak) true ise, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> Web ana bilgisayarı üzerinde.</span><span class="sxs-lookup"><span data-stu-id="c8992-135">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

     <span data-ttu-id="c8992-136">Koşullar (uygulama, hizmet olarak çalıştırıldığında) false olduğunda:</span><span class="sxs-lookup"><span data-stu-id="c8992-136">If the conditions are false (the app is run as a service):</span></span>

     * <span data-ttu-id="c8992-137">Çağrı <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> ve uygulamanın yayımlanmış konumuna bir yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="c8992-137">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> and use a path to the app's published location.</span></span> <span data-ttu-id="c8992-138">Remove() çağırmayın <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti uygulaması döndürüldüğünden yolunu almak için *C:\\WINDOWS\\system32* klasör zaman `GetCurrentDirectory` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c8992-138">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="c8992-139">Daha fazla bilgi için [geçerli dizin ve içerik kök](#current-directory-and-content-root) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c8992-139">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
     * <span data-ttu-id="c8992-140">Çağrı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulamasını bir hizmet olarak çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="c8992-140">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

     <span data-ttu-id="c8992-141">Çünkü [komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirir `--console` anahtarı bağımsız değişkenlerden önce kaldırılır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bunları alır.</span><span class="sxs-lookup"><span data-stu-id="c8992-141">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

   * <span data-ttu-id="c8992-142">Windows olay günlüğüne yazmak için olay günlüğü Sağlayıcısı Ekle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="c8992-142">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="c8992-143">İle günlük tutma düzeyini ayarlamaya `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="c8992-143">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="c8992-144">Tanıtım ve test amacıyla örnek uygulamanın üretim ayarları dosyası günlüğe kaydetme düzeyini ayarlar `Information`.</span><span class="sxs-lookup"><span data-stu-id="c8992-144">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="c8992-145">Üretim ortamında genellikle değerine `Error`.</span><span class="sxs-lookup"><span data-stu-id="c8992-145">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="c8992-146">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="c8992-146">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

1. <span data-ttu-id="c8992-147">Kullanarak uygulama yayımlamayı [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish), [Visual Studio yayımlama profilini](xref:host-and-deploy/visual-studio-publish-profiles), veya Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c8992-147">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="c8992-148">Visual Studio kullanırken **FolderProfile** ve yapılandırma **hedef konum** seçmeden önce **Yayımla** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c8992-148">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="c8992-149">Komut satırı arabirimi (CLI) araçlarını kullanarak örnek uygulamayı yayımlamak için çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) geçirilen bir sürüm yapılandırması ile proje klasöründeki bir komut isteminde komutunu [- c |--yapılandırma](/dotnet/core/tools/dotnet-publish#options)seçeneği.</span><span class="sxs-lookup"><span data-stu-id="c8992-149">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="c8992-150">Kullanım [-o |--çıktı](/dotnet/core/tools/dotnet-publish#options) uygulama dışında bir klasöre yayımlamak için bir yol ile seçeneği.</span><span class="sxs-lookup"><span data-stu-id="c8992-150">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

   * <span data-ttu-id="c8992-151">**Framework bağımlı dağıtım (FDD)**</span><span class="sxs-lookup"><span data-stu-id="c8992-151">**Framework-dependent Deployment (FDD)**</span></span>

     <span data-ttu-id="c8992-152">Aşağıdaki örnekte, uygulama için yayımlanan *c:\\svc* klasörü:</span><span class="sxs-lookup"><span data-stu-id="c8992-152">In the following example, the app is published to the *c:\\svc* folder:</span></span>

     ```console
     dotnet publish --configuration Release --output c:\svc
     ```

   * <span data-ttu-id="c8992-153">**Kendi başına dağıtım (SCD)** &ndash; RID belirtilmelidir `<RuntimeIdenfifier>` (veya `<RuntimeIdentifiers>`) özelliği proje dosyasının.</span><span class="sxs-lookup"><span data-stu-id="c8992-153">**Self-contained Deployment (SCD)** &ndash; The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="c8992-154">Çalışma zamanı kaynağı [- r |--çalışma zamanı](/dotnet/core/tools/dotnet-publish#options) seçeneği `dotnet publish` komutu.</span><span class="sxs-lookup"><span data-stu-id="c8992-154">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

     <span data-ttu-id="c8992-155">Aşağıdaki örnekte, uygulama için yayımlanan `win7-x64` çalışma zamanına *c:\\svc* klasörü:</span><span class="sxs-lookup"><span data-stu-id="c8992-155">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

     ```console
     dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
     ```

1. <span data-ttu-id="c8992-156">Hizmet kullanımı için bir kullanıcı hesabı oluşturma `net user` komutu:</span><span class="sxs-lookup"><span data-stu-id="c8992-156">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="c8992-157">Örnek uygulama için bir kullanıcı hesabı adı ile oluşturun. `ServiceUser` ve parola.</span><span class="sxs-lookup"><span data-stu-id="c8992-157">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="c8992-158">Aşağıdaki komutta `{PASSWORD}` ile bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="c8992-158">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="c8992-159">Bir gruba kullanıcı eklemeniz gerekiyorsa, kullanın `net localgroup` komutu, burada `{GROUP}` grubunun adıdır:</span><span class="sxs-lookup"><span data-stu-id="c8992-159">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="c8992-160">Daha fazla bilgi için [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="c8992-160">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="c8992-161">Uygulamanın klasörüne yazma/okuma/yürütme erişimi vermek kullanarak [icacls](/windows-server/administration/windows-commands/icacls) komutu:</span><span class="sxs-lookup"><span data-stu-id="c8992-161">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="c8992-162">`{PATH}` &ndash; Uygulamanın klasörün yolu.</span><span class="sxs-lookup"><span data-stu-id="c8992-162">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="c8992-163">`{USER ACCOUNT}` &ndash; Kullanıcı hesabı (SID).</span><span class="sxs-lookup"><span data-stu-id="c8992-163">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="c8992-164">`(OI)` &ndash; Nesne devral bayrağı dosyaları alt izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="c8992-164">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="c8992-165">`(CI)` &ndash; Kapsayıcı devral bayrağı, alt klasörler için izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="c8992-165">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="c8992-166">`{PERMISSION FLAGS}` &ndash; Uygulamanın erişim izinlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c8992-166">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="c8992-167">Yazma (`W`)</span><span class="sxs-lookup"><span data-stu-id="c8992-167">Write (`W`)</span></span>
     * <span data-ttu-id="c8992-168">Okuma (`R`)</span><span class="sxs-lookup"><span data-stu-id="c8992-168">Read (`R`)</span></span>
     * <span data-ttu-id="c8992-169">Yürütme (`X`)</span><span class="sxs-lookup"><span data-stu-id="c8992-169">Execute (`X`)</span></span>
     * <span data-ttu-id="c8992-170">Tam (`F`)</span><span class="sxs-lookup"><span data-stu-id="c8992-170">Full (`F`)</span></span>
     * <span data-ttu-id="c8992-171">Değiştir (`M`)</span><span class="sxs-lookup"><span data-stu-id="c8992-171">Modify (`M`)</span></span>
   * <span data-ttu-id="c8992-172">`/t` &ndash; Yinelemeli olarak mevcut alt klasörler ve dosyalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c8992-172">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="c8992-173">Örnek uygulamayı yayımlanan *c:\\svc* klasörü ve `ServiceUser` hesap yazma/okuma/Yürütme izinleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="c8992-173">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="c8992-174">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="c8992-174">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="c8992-175">Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="c8992-175">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="c8992-176">`binPath` Değerdir yürütülebilir dosya adını içeren uygulamanın yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="c8992-176">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="c8992-177">**Eşittir işareti ve tırnak karakteri her bir parametre ve değer arasında gerekli bir alandır.**</span><span class="sxs-lookup"><span data-stu-id="c8992-177">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="c8992-178">`{SERVICE NAME}` &ndash; Hizmete atanacak ad [Hizmet Denetimi Yöneticisi](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="c8992-178">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="c8992-179">`{PATH}` &ndash; Hizmet yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="c8992-179">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="c8992-180">`{DOMAIN}` &ndash; Etki alanına katılmış bir makine etki alanı.</span><span class="sxs-lookup"><span data-stu-id="c8992-180">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="c8992-181">Makine etki alanına katılmış değilse yerel makine adını.</span><span class="sxs-lookup"><span data-stu-id="c8992-181">If the machine isn't domain-joined, the local machine name.</span></span>
   * <span data-ttu-id="c8992-182">`{USER ACCOUNT}` &ndash; Hizmetinin çalıştığı kullanıcı hesabı.</span><span class="sxs-lookup"><span data-stu-id="c8992-182">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
   * <span data-ttu-id="c8992-183">`{PASSWORD}` &ndash; Kullanıcı hesabı parolası.</span><span class="sxs-lookup"><span data-stu-id="c8992-183">`{PASSWORD}` &ndash; The user account password.</span></span>

   > [!WARNING]
   > <span data-ttu-id="c8992-184">Yapmak **değil** atlamak `obj` parametresi.</span><span class="sxs-lookup"><span data-stu-id="c8992-184">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="c8992-185">İçin varsayılan değer `obj` olduğu [LocalSystem hesabı](/windows/desktop/services/localsystem-account) hesabı.</span><span class="sxs-lookup"><span data-stu-id="c8992-185">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="c8992-186">Altında bir hizmeti çalıştıran `LocalSystem` hesabı önemli bir güvenlik riski sunar.</span><span class="sxs-lookup"><span data-stu-id="c8992-186">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="c8992-187">Her zaman bir servis ayrıcalıkları sınırlı sahip bir kullanıcı hesabı ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c8992-187">Always run a service with a user account that has restricted privileges.</span></span>

   <span data-ttu-id="c8992-188">Aşağıdaki örnekte örnek uygulama için:</span><span class="sxs-lookup"><span data-stu-id="c8992-188">In the following example for the sample app:</span></span>

   * <span data-ttu-id="c8992-189">Adlı hizmetin **MyService**.</span><span class="sxs-lookup"><span data-stu-id="c8992-189">The service is named **MyService**.</span></span>
   * <span data-ttu-id="c8992-190">Yayınlanan hizmet bulunan *c:\\svc* klasör.</span><span class="sxs-lookup"><span data-stu-id="c8992-190">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="c8992-191">Uygulama yürütülebilir dosyası adlı *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="c8992-191">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="c8992-192">İçine `binPath` çift tırnak (") değeri.</span><span class="sxs-lookup"><span data-stu-id="c8992-192">Enclose the `binPath` value in double quotation marks (").</span></span>
   * <span data-ttu-id="c8992-193">Altında çalışacağı `ServiceUser` hesabı.</span><span class="sxs-lookup"><span data-stu-id="c8992-193">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="c8992-194">Değiştirin `{DOMAIN}` kullanıcı hesabının etki alanı veya yerel makine adı.</span><span class="sxs-lookup"><span data-stu-id="c8992-194">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="c8992-195">İçine `obj` çift tırnak (") değeri.</span><span class="sxs-lookup"><span data-stu-id="c8992-195">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="c8992-196">Örnek: barındıran sistemde adlı bir yerel makineye ise `MairaPC`ayarlayın `obj` için `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="c8992-196">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="c8992-197">Değiştirin `{PASSWORD}` ile kullanıcı hesabının parolası.</span><span class="sxs-lookup"><span data-stu-id="c8992-197">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="c8992-198">İçine `password` çift tırnak (") değeri.</span><span class="sxs-lookup"><span data-stu-id="c8992-198">Enclose the `password` value in double quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="c8992-199">Parametreleri eşittir işareti ve parametrelerin değerleri arasında boşluk bulunmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="c8992-199">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="c8992-200">Hizmetle başlar `sc start {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="c8992-200">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="c8992-201">Örnek uygulama hizmeti başlatmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="c8992-201">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="c8992-202">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="c8992-202">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="c8992-203">Hizmet durumunu denetlemek için kullanmak `sc query {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="c8992-203">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="c8992-204">Durumu aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="c8992-204">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="c8992-205">Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="c8992-205">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="c8992-206">Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama, bir yola göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendiren `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi ara yazılım](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="c8992-206">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="c8992-207">Örnek app service için uygulamaya Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c8992-207">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="c8992-208">Hizmetle Durdur `sc stop {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="c8992-208">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="c8992-209">Aşağıdaki komut örnek uygulama hizmetini durdurur:</span><span class="sxs-lookup"><span data-stu-id="c8992-209">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="c8992-210">Hizmeti ile bir hizmeti durdurmak için bir kısa bir gecikmeyle kaldırmanız `sc delete {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="c8992-210">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="c8992-211">Örnek uygulama hizmeti durumunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="c8992-211">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="c8992-212">Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmeti kaldırmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="c8992-212">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="c8992-213">Başlatma ve durdurma olayları işleme</span><span class="sxs-lookup"><span data-stu-id="c8992-213">Handle starting and stopping events</span></span>

<span data-ttu-id="c8992-214">İşlenecek <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olayları, aşağıdaki ek değişiklikleri gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="c8992-214">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="c8992-215">Türetilen bir sınıf oluşturmanız <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> ile `OnStarting`, `OnStarted`, ve `OnStopping` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="c8992-215">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="c8992-216">Bir genişletme yöntemi için oluşturma <xref:Microsoft.AspNetCore.Hosting.IWebHost> Geçiren `CustomWebHostService` için <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="c8992-216">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="c8992-217">İçinde `Program.Main`, çağrı `RunAsCustomService` genişletme yöntemi yerine <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="c8992-217">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="c8992-218">Konumu görmek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> içinde `Program.Main`, gösterilen kod örneği başvurmak [bir Windows hizmetinde bir projeyi dönüştürmeye](#convert-a-project-into-a-windows-service) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c8992-218">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="c8992-219">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="c8992-219">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="c8992-220">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="c8992-220">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="c8992-221">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="c8992-221">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="c8992-222">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c8992-222">Configure HTTPS</span></span>

<span data-ttu-id="c8992-223">Hizmet güvenli bir uç nokta ile yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="c8992-223">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="c8992-224">Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c8992-224">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="c8992-225">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="c8992-225">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="c8992-226">Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c8992-226">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="c8992-227">Geçerli dizin ve içerik kök</span><span class="sxs-lookup"><span data-stu-id="c8992-227">Current directory and content root</span></span>

<span data-ttu-id="c8992-228">Geçerli çalışma dizini çağırarak döndürülen <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör.</span><span class="sxs-lookup"><span data-stu-id="c8992-228">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="c8992-229">*System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum.</span><span class="sxs-lookup"><span data-stu-id="c8992-229">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="c8992-230">Korumak ve bir hizmetin varlıklar ve ayar dosyaları erişmek için aşağıdaki yaklaşımlardan birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c8992-230">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="c8992-231">Uygulamanın klasör için içerik kök yolu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="c8992-231">Set the content root path to the app's folder</span></span>

<span data-ttu-id="c8992-232"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Sağlanan aynı yol `binPath` hizmeti oluşturulduğunda bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="c8992-232">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="c8992-233">Çağırmak yerine `GetCurrentDirectory` ayarları dosyalara olan yolları oluşturmak için arama <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> ile içerik uygulamanın kök yolu.</span><span class="sxs-lookup"><span data-stu-id="c8992-233">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> with the path to the app's content root.</span></span>

<span data-ttu-id="c8992-234">İçinde `Program.Main`, hizmetin yürütülebilir dosya klasörü yolunu belirlemek ve uygulamanın içerik kök'kurmak için yolunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="c8992-234">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="c8992-235">Disk üzerinde uygun bir konumda hizmetin dosyaları Store</span><span class="sxs-lookup"><span data-stu-id="c8992-235">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="c8992-236">Mutlak bir yol belirtin <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> kullanırken bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> dosyaları içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="c8992-236">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8992-237">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c8992-237">Additional resources</span></span>

* <span data-ttu-id="c8992-238">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="c8992-238">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
