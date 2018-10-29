---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f9b1c3fbfafa839c116688e0ac63804afcd5dbe0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206679"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="5bf47-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="5bf47-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="5bf47-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5bf47-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5bf47-105">ASP.NET Core uygulaması olarak IIS kullanmadan Windows üzerinde barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="5bf47-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="5bf47-106">Bir Windows hizmeti olarak barındırıldığında, uygulama yeniden başlatma sonrasında otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="5bf47-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5bf47-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="5bf47-108">Bir Windows hizmetinde bir projeyi dönüştürmeye</span><span class="sxs-lookup"><span data-stu-id="5bf47-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="5bf47-109">Mevcut bir ASP.NET Core projesini ayarlarsınız bir hizmet olarak çalışacak şekilde ayarlamak için aşağıdaki en düşük değişiklikleri gerekir:</span><span class="sxs-lookup"><span data-stu-id="5bf47-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="5bf47-110">Proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="5bf47-110">In the project file:</span></span>

   * <span data-ttu-id="5bf47-111">Bir Windows varlığını onaylamak [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) veya eklemek `<PropertyGroup>` , hedef Framework'ü içerir:</span><span class="sxs-lookup"><span data-stu-id="5bf47-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      <span data-ttu-id="5bf47-112">İçin birden fazla RID yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="5bf47-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="5bf47-113">RID noktalı virgülle ayrılmış bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="5bf47-114">Özellik adını kullanan `<RuntimeIdentifiers>` (çoğul).</span><span class="sxs-lookup"><span data-stu-id="5bf47-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="5bf47-115">Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="5bf47-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="5bf47-116">İçin bir paket başvurusu ekleme [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="5bf47-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="5bf47-117">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="5bf47-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="5bf47-118">Çağrı [ana bilgisayar. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) yerine `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="5bf47-119">Çağrı [UseContentRoot](xref:fundamentals/host/web-host#content-root) ve uygulama için bir yol yerine konumu yayımlanan `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="5bf47-120">Uygulamayı yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="5bf47-120">Publish the app.</span></span> <span data-ttu-id="5bf47-121">Kullanım [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profilini](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="5bf47-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="5bf47-122">Visual Studio kullanırken **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="5bf47-122">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="5bf47-123">Komut satırı arabirimi (CLI) araçlarını kullanarak örnek uygulamayı yayımlamak için çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) proje klasöründeki bir komut isteminde komutu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-123">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="5bf47-124">RID belirtilmelidir `<RuntimeIdenfifier>` (veya `<RuntimeIdentifiers>`) özelliği proje dosyasının.</span><span class="sxs-lookup"><span data-stu-id="5bf47-124">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="5bf47-125">Aşağıdaki örnekte, uygulama için sürüm yapılandırmasında yayımlanır `win7-x64` çalışma zamanı:</span><span class="sxs-lookup"><span data-stu-id="5bf47-125">In the following example, the app is published in Release configuration for the `win7-x64` runtime:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. <span data-ttu-id="5bf47-126">Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="5bf47-126">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="5bf47-127">`binPath` Değerdir yürütülebilir dosya adını içeren uygulamanın yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-127">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="5bf47-128">**Eşittir işareti ve tırnak karakteri yolunun başında arasındaki boşluk gereklidir.**</span><span class="sxs-lookup"><span data-stu-id="5bf47-128">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="5bf47-129">Proje klasöründe yayımlanan bir hizmet için yolunu kullanın *yayımlama* hizmet oluşturmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="5bf47-129">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="5bf47-130">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="5bf47-130">In the following example:</span></span>

   * <span data-ttu-id="5bf47-131">Projenin bulunduğu *c:\\my_services\\AspNetCoreService* klasör.</span><span class="sxs-lookup"><span data-stu-id="5bf47-131">The project resides in the *c:\\my_services\\AspNetCoreService* folder.</span></span>
   * <span data-ttu-id="5bf47-132">Proje yayınlanan `Release` yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="5bf47-132">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="5bf47-133">Hedef Çerçeve adı (TFM) olan `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-133">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="5bf47-134">Çalışma zamanı tanımlayıcı (RID) olan `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-134">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="5bf47-135">Uygulama yürütülebilir dosyası adlı *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="5bf47-135">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="5bf47-136">Adlı hizmetin **MyService**.</span><span class="sxs-lookup"><span data-stu-id="5bf47-136">The service is named **MyService**.</span></span>

   <span data-ttu-id="5bf47-137">Örnek:</span><span class="sxs-lookup"><span data-stu-id="5bf47-137">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="5bf47-138">Alanı arasında mevcut olduğundan emin olun `binPath=` bağımsız değişkeni ve değeri.</span><span class="sxs-lookup"><span data-stu-id="5bf47-138">Make sure the space is present between the `binPath=` argument and its value.</span></span>

   <span data-ttu-id="5bf47-139">Yayımlama ve farklı bir klasör hizmeti başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="5bf47-139">To publish and start the service from a different folder:</span></span>

      * <span data-ttu-id="5bf47-140">Kullanım [--çıktı &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) seçeneğini `dotnet publish` komutu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-140">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="5bf47-141">Visual Studio kullanıyorsanız, yapılandırma **hedef konum** içinde **FolderProfile** özellik sayfasında seçmeden önce yayımlama **Yayımla** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="5bf47-141">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
      * <span data-ttu-id="5bf47-142">Hizmetle `sc.exe` çıkış klasör yolunu kullanarak komutu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-142">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="5bf47-143">Sağlanan yol hizmetin yürütülebilir dosya adı dahil `binPath`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-143">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="5bf47-144">Hizmetle başlar `sc start <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-144">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="5bf47-145">Örnek uygulama hizmeti başlatmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="5bf47-145">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="5bf47-146">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="5bf47-146">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="5bf47-147">Hizmet durumunu denetlemek için kullanmak `sc query <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-147">To check the status of the service, use the `sc query <SERVICE_NAME>` command.</span></span> <span data-ttu-id="5bf47-148">Durumu aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="5bf47-148">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="5bf47-149">Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="5bf47-149">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="5bf47-150">Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama, bir yola göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendiren `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi ara yazılım](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="5bf47-150">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="5bf47-151">Örnek app service için uygulamaya Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-151">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="5bf47-152">Hizmetle Durdur `sc stop <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-152">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="5bf47-153">Aşağıdaki komut örnek uygulama hizmetini durdurur:</span><span class="sxs-lookup"><span data-stu-id="5bf47-153">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="5bf47-154">Hizmeti ile bir hizmeti durdurmak için bir kısa bir gecikmeyle kaldırmanız `sc delete <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="5bf47-154">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="5bf47-155">Örnek uygulama hizmeti durumunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="5bf47-155">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="5bf47-156">Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmeti kaldırmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="5bf47-156">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="5bf47-157">Uygulama dışında bir hizmet çalıştırma</span><span class="sxs-lookup"><span data-stu-id="5bf47-157">Run the app outside of a service</span></span>

<span data-ttu-id="5bf47-158">Çağıran kod eklemek için her zamanki şekilde bir hizmet dışında çalışırken hata ayıklama ve test etmek daha kolay `RunAsService` yalnızca belirli koşullar altında.</span><span class="sxs-lookup"><span data-stu-id="5bf47-158">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="5bf47-159">Örneğin, bir konsol uygulaması olarak uygulama çalıştırabilirsiniz bir `--console` komut satırı bağımsız değişkeni veya hata ayıklayıcı eklenir:</span><span class="sxs-lookup"><span data-stu-id="5bf47-159">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="5bf47-160">ASP.NET Core yapılandırma komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden `--console` anahtar için bağımsız değişkenler geçirilmeden önce kaldırılır [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="5bf47-160">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="5bf47-161">`isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="5bf47-161">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="5bf47-162">Durdurma ve başlatma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="5bf47-162">Handle stopping and starting events</span></span>

<span data-ttu-id="5bf47-163">İşlenecek [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), ve [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) olayları, aşağıdaki ek değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="5bf47-163">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="5bf47-164">Türetilen bir sınıf oluşturmanız [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="5bf47-164">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="5bf47-165">Bir genişletme yöntemi için oluşturma [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) özel Geçiren `WebHostService` için [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="5bf47-165">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="5bf47-166">İçinde `Program.Main`, yeni bir uzantı metodu çağırma `RunAsCustomService`, yerine [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="5bf47-166">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="5bf47-167">`isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="5bf47-167">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="5bf47-168">Varsa özel `WebHostService` kod bağımlılık ekleme (örneğin, bir Günlükçü) hizmetinden gerektirir,'ndan elde [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) özelliği:</span><span class="sxs-lookup"><span data-stu-id="5bf47-168">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="5bf47-169">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="5bf47-169">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="5bf47-170">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="5bf47-170">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="5bf47-171">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="5bf47-171">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="5bf47-172">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5bf47-172">Configure HTTPS</span></span>

<span data-ttu-id="5bf47-173">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="5bf47-173">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="5bf47-174">Geçerli dizin ve içerik kök</span><span class="sxs-lookup"><span data-stu-id="5bf47-174">Current directory and content root</span></span>

<span data-ttu-id="5bf47-175">Geçerli çalışma dizini çağırarak döndürülen `Directory.GetCurrentDirectory()` bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör.</span><span class="sxs-lookup"><span data-stu-id="5bf47-175">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="5bf47-176">*System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum.</span><span class="sxs-lookup"><span data-stu-id="5bf47-176">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="5bf47-177">Korumak ve bir hizmetin varlıklar ve ayar dosyaları ile erişmek için aşağıdaki yaklaşımlardan birini kullanın [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) kullanırken bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="5bf47-177">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="5bf47-178">İçerik kök yolu kullanın.</span><span class="sxs-lookup"><span data-stu-id="5bf47-178">Use the content root path.</span></span> <span data-ttu-id="5bf47-179">`IHostingEnvironment.ContentRootPath` Sağlanan aynı yol `binPath` hizmeti oluşturulduğunda bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="5bf47-179">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="5bf47-180">Yerine `Directory.GetCurrentDirectory()` ayarları dosyalara olan yolları oluşturmak için içerik kök yolu kullanın ve uygulamanın içerik kökünde dosyaların bakımını yapar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-180">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="5bf47-181">Disk üzerinde uygun bir konumda dosya Store.</span><span class="sxs-lookup"><span data-stu-id="5bf47-181">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="5bf47-182">Mutlak bir yol belirtin `SetBasePath` dosyaları içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="5bf47-182">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5bf47-183">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5bf47-183">Additional resources</span></span>

* <span data-ttu-id="5bf47-184">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="5bf47-184">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
