---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: b156cd0755d7918d5f8433fcbe5c870ad04ac13e
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396227"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="d3d89-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="d3d89-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="d3d89-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d3d89-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d3d89-105">ASP.NET Core uygulaması olarak IIS kullanmadan Windows üzerinde barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="d3d89-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="d3d89-106">Bir Windows hizmeti olarak barındırılan uygulamayı otomatik olarak yapabilirsiniz başladıktan sonra bilgisayarı yeniden başlatır ve kullanıcı müdahalesine gerek kalmadan kilitleniyor.</span><span class="sxs-lookup"><span data-stu-id="d3d89-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="d3d89-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3d89-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="d3d89-108">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="d3d89-108">Get started</span></span>

<span data-ttu-id="d3d89-109">Bir hizmet olarak çalıştırmak için mevcut bir ASP.NET Core projesini ayarlarsınız ayarlamak için aşağıdaki en düşük değişiklikleri gerekir:</span><span class="sxs-lookup"><span data-stu-id="d3d89-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="d3d89-110">Proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="d3d89-110">In the project file:</span></span>

   1. <span data-ttu-id="d3d89-111">Çalışma zamanı tanımlayıcısı varlığını onaylamak veya ekleyin  **\<PropertyGroup >** , hedef Framework'ü içerir:</span><span class="sxs-lookup"><span data-stu-id="d3d89-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

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

   1. <span data-ttu-id="d3d89-112">İçin bir paket başvurusu ekleme [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="d3d89-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="d3d89-113">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="d3d89-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="d3d89-114">Çağrı [ana bilgisayar. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) yerine `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="d3d89-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="d3d89-115">Çağrı [UseContentRoot](xref:fundamentals/host/web-host#content-root) ve uygulama için bir yol yerine konumu yayımlanan `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="d3d89-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="d3d89-116">Uygulamayı yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="d3d89-116">Publish the app.</span></span> <span data-ttu-id="d3d89-117">Kullanım [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profilini](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="d3d89-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="d3d89-118">Visual Studio kullanırken **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="d3d89-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="d3d89-119">Komut satırından örnek uygulamayı yayımlamak için proje klasöründen konsol penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3d89-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="d3d89-120">Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="d3d89-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="d3d89-121">`binPath` Değerdir yürütülebilir dosya adını içeren uygulamanın yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="d3d89-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="d3d89-122">**Eşittir işareti ve tırnak karakteri yolunun başında arasındaki boşluk gereklidir.**</span><span class="sxs-lookup"><span data-stu-id="d3d89-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="d3d89-123">Proje klasöründe yayımlanan bir hizmet için yolunu kullanın *yayımlama* hizmet oluşturmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="d3d89-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="d3d89-124">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d3d89-124">In the following example:</span></span>

   * <span data-ttu-id="d3d89-125">Projenin bulunduğu `c:\my_services\AspNetCoreService` klasör.</span><span class="sxs-lookup"><span data-stu-id="d3d89-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="d3d89-126">Proje yayınlanan `Release` yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="d3d89-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="d3d89-127">Hedef Çerçeve adı (TFM) olan `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="d3d89-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="d3d89-128">Çalışma zamanı tanımlayıcı (RID) olan `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="d3d89-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="d3d89-129">Uygulama yürütülebilir dosyası adlı *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="d3d89-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="d3d89-130">Adlı hizmetin **MyService**.</span><span class="sxs-lookup"><span data-stu-id="d3d89-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="d3d89-131">Örnek:</span><span class="sxs-lookup"><span data-stu-id="d3d89-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="d3d89-132">Alanı arasında mevcut olduğundan emin olun `binPath=` bağımsız değişkeni ve değeri.</span><span class="sxs-lookup"><span data-stu-id="d3d89-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="d3d89-133">Yayımlama ve farklı bir klasör hizmeti başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="d3d89-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="d3d89-134">Kullanım [--çıktı &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) seçeneğini `dotnet publish` komutu.</span><span class="sxs-lookup"><span data-stu-id="d3d89-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="d3d89-135">Visual Studio kullanıyorsanız, yapılandırma **hedef konum** içinde **FolderProfile** özellik sayfasında seçmeden önce yayımlama **Yayımla** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d3d89-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="d3d89-136">Hizmetle `sc.exe` çıkış klasör yolunu kullanarak komutu.</span><span class="sxs-lookup"><span data-stu-id="d3d89-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="d3d89-137">Sağlanan yol hizmetin yürütülebilir dosya adı dahil `binPath`.</span><span class="sxs-lookup"><span data-stu-id="d3d89-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="d3d89-138">Hizmetle başlar `sc start <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="d3d89-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="d3d89-139">Örnek uygulama hizmeti başlatmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="d3d89-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="d3d89-140">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="d3d89-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="d3d89-141">`sc query <SERVICE_NAME>` Komutu, durumu belirlemek için hizmet durumunu denetlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d3d89-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="d3d89-142">Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="d3d89-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="d3d89-143">Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama, bir yola göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendiren `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi ara yazılım](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="d3d89-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="d3d89-144">Örnek app service için uygulamaya Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d3d89-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="d3d89-145">Hizmetle Durdur `sc stop <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="d3d89-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="d3d89-146">Aşağıdaki komut örnek uygulama hizmetini durdurur:</span><span class="sxs-lookup"><span data-stu-id="d3d89-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="d3d89-147">Hizmeti ile bir hizmeti durdurmak için bir kısa bir gecikmeyle kaldırmanız `sc delete <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="d3d89-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="d3d89-148">Örnek uygulama hizmeti durumunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="d3d89-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="d3d89-149">Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmeti kaldırmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="d3d89-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="d3d89-150">Dışında bir hizmeti çalıştırmanın bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="d3d89-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="d3d89-151">Çağıran kod eklemek için her zamanki şekilde bir hizmet dışında çalışırken hata ayıklama ve test etmek daha kolay `RunAsService` yalnızca belirli koşullar altında.</span><span class="sxs-lookup"><span data-stu-id="d3d89-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="d3d89-152">Örneğin, bir konsol uygulaması olarak uygulama çalıştırabilirsiniz bir `--console` komut satırı bağımsız değişkeni veya hata ayıklayıcı eklenir:</span><span class="sxs-lookup"><span data-stu-id="d3d89-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="d3d89-153">ASP.NET Core yapılandırma komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden `--console` anahtar için bağımsız değişkenler geçirilmeden önce kaldırılır [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="d3d89-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="d3d89-154">`isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="d3d89-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="d3d89-155">Durdurma ve başlatma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="d3d89-155">Handle stopping and starting events</span></span>

<span data-ttu-id="d3d89-156">İşlenecek [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), ve [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) olayları, aşağıdaki ek değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="d3d89-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="d3d89-157">Türetilen bir sınıf oluşturmanız [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="d3d89-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="d3d89-158">Bir genişletme yöntemi için oluşturma [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) özel Geçiren `WebHostService` için [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="d3d89-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="d3d89-159">İçinde `Program.Main`, yeni bir uzantı metodu çağırma `RunAsCustomService`, yerine [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="d3d89-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="d3d89-160">`isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="d3d89-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="d3d89-161">Varsa özel `WebHostService` kod bağımlılık ekleme (örneğin, bir Günlükçü) hizmetinden gerektirir,'ndan elde [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) özelliği:</span><span class="sxs-lookup"><span data-stu-id="d3d89-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d3d89-162">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="d3d89-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d3d89-163">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="d3d89-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="d3d89-164">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="d3d89-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="d3d89-165">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d3d89-165">Configure HTTPS</span></span>

<span data-ttu-id="d3d89-166">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="d3d89-166">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3d89-167">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d3d89-167">Additional resources</span></span>

* <span data-ttu-id="d3d89-168">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="d3d89-168">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
