---
title: Bir Windows hizmetinde konak ASP.NET Çekirdeği
author: guardrex
description: Bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 718cc83bb29c0cff323853d22c107e00616b1dd1
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126241"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="dc167-103">Bir Windows hizmetinde konak ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="dc167-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="dc167-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="dc167-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="dc167-105">Bir ASP.NET Core uygulama IIS olarak kullanmadan Windows üzerinde barındırılabilir bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="dc167-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="dc167-106">Bir Windows hizmeti olarak barındırıldığında, uygulama otomatik olarak şunları yapabilecek başladıktan sonra bilgisayarı yeniden başlatır ve insan etkileşimi gerektirmeden çöküyor.</span><span class="sxs-lookup"><span data-stu-id="dc167-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="dc167-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dc167-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="dc167-108">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="dc167-108">Get started</span></span>

<span data-ttu-id="dc167-109">Bir hizmet olarak çalıştırmak üzere mevcut bir ASP.NET Core projeyi ayarlamak için aşağıdaki en düşük değişiklikleri gerekir:</span><span class="sxs-lookup"><span data-stu-id="dc167-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="dc167-110">Proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="dc167-110">In the project file:</span></span>

   1. <span data-ttu-id="dc167-111">Çalışma zamanı tanımlayıcı varlığını onaylamak veya ekleyin  **\<PropertyGroup >** hedef Framework'ü içerir:</span><span class="sxs-lookup"><span data-stu-id="dc167-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="dc167-112">Bir paket başvurusunu ekleyin [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="dc167-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="dc167-113">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="dc167-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="dc167-114">Çağrı [ana bilgisayar. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) yerine `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="dc167-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="dc167-115">Kod çağırırsa `UseContentRoot`, uygulama için bir yol yerine konumu yayımlanan kullanım `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="dc167-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="dc167-116">Uygulamayı yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="dc167-116">Publish the app.</span></span> <span data-ttu-id="dc167-117">Kullanım [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profili](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="dc167-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="dc167-118">Komut satırından örnek uygulamayı yayımlamak için proje klasöründen konsol penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="dc167-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="dc167-119">Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="dc167-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="dc167-120">`binPath` Yürütülebilir dosya adı içeren uygulamanın yürütülebilir dosya yolu bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="dc167-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="dc167-121">**Eşittir işareti ve yolun başlangıcında tırnak karakteri arasındaki boşluğu gereklidir.**</span><span class="sxs-lookup"><span data-stu-id="dc167-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="dc167-122">Proje klasöründe yayımlanan bir hizmet için yolunu kullanın *yayımlama* hizmeti oluşturmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="dc167-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="dc167-123">Aşağıdaki örnekte hizmetidir:</span><span class="sxs-lookup"><span data-stu-id="dc167-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="dc167-124">Adlı **MyService**.</span><span class="sxs-lookup"><span data-stu-id="dc167-124">Named **MyService**.</span></span>
   * <span data-ttu-id="dc167-125">Yayımlanan *c:\\my_services\\AspNetCoreService\\bin\\sürüm\\&lt;TARGET_FRAMEWORK&gt;\\Yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="dc167-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="dc167-126">Adlı bir uygulama tarafından yürütülebilir temsil *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="dc167-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="dc167-127">Yönetici ayrıcalıklarıyla bir komut kabuğu'nu açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="dc167-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="dc167-128">Alanı arasında mevcut olduğundan emin olun `binPath=` bağımsız değişkeni ve değeri.</span><span class="sxs-lookup"><span data-stu-id="dc167-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="dc167-129">Yayımlama ve farklı bir klasör hizmetini başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="dc167-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="dc167-130">Kullanım [--çıkış &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) seçeneği `dotnet publish` komutu.</span><span class="sxs-lookup"><span data-stu-id="dc167-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="dc167-131">İle hizmet oluşturma `sc.exe` çıkış klasörü yolu kullanarak komutu.</span><span class="sxs-lookup"><span data-stu-id="dc167-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="dc167-132">Sağlanan yol hizmetin yürütülebilir dosyanın adını dahil `binPath`.</span><span class="sxs-lookup"><span data-stu-id="dc167-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="dc167-133">Hizmetle başlar `sc start <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="dc167-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="dc167-134">Örnek uygulama hizmetini başlatmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="dc167-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="dc167-135">Komut hizmetini başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="dc167-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="dc167-136">`sc query <SERVICE_NAME>` Komutu, durumu belirlemek için hizmet durumunu denetlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="dc167-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="dc167-137">Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="dc167-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="dc167-138">Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama kendi yolundaki göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendirir `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi Ara](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="dc167-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="dc167-139">Örnek uygulama hizmeti için uygulamaya Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dc167-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="dc167-140">Hizmet ile Durdur `sc stop <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="dc167-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="dc167-141">Aşağıdaki komut örnek uygulama hizmetini durdurur:</span><span class="sxs-lookup"><span data-stu-id="dc167-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="dc167-142">Hizmet ile bir hizmeti durdurmak için kısa bir gecikmeyle, kaldırma `sc delete <SERVICE_NAME>` komutu.</span><span class="sxs-lookup"><span data-stu-id="dc167-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="dc167-143">Örnek uygulama hizmetinin durumunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="dc167-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="dc167-144">Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmetini kaldırmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="dc167-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="dc167-145">Hizmet dışında çalıştırmak için bir yol sağlama</span><span class="sxs-lookup"><span data-stu-id="dc167-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="dc167-146">Test ve çağıran kodu eklemek için her zamanki olacak şekilde bir hizmet dışında çalıştırılırken hata ayıklamak daha kolay `RunAsService` yalnızca belirli koşullar altında.</span><span class="sxs-lookup"><span data-stu-id="dc167-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="dc167-147">Örneğin, uygulama ile bir konsol uygulaması olarak çalıştırabilirsiniz bir `--console` komut satırı bağımsız değişkeni ya da hata ayıklayıcısı ekli varsa:</span><span class="sxs-lookup"><span data-stu-id="dc167-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="dc167-148">ASP.NET Core yapılandırma komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden `--console` anahtar bağımsız değişkenler için geçirilmeden önce kaldırılır [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="dc167-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="dc167-149">Durdurma ve başlatma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="dc167-149">Handle stopping and starting events</span></span>

<span data-ttu-id="dc167-150">İşlenecek [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), ve [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) olayları, aşağıdaki ek değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="dc167-150">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="dc167-151">Türeyen bir sınıf oluşturun [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="dc167-151">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="dc167-152">Create genişletme yöntemi için [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) özel geçen `WebHostService` için [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="dc167-152">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="dc167-153">İçinde `Program.Main`, yeni uzantı metodu çağırma `RunAsCustomService`, yerine [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="dc167-153">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="dc167-154">Varsa özel `WebHostService` kod bağımlılık ekleme (örneğin, bir Günlükçü) hizmetinden gerektirir, buradan edinin [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) özelliği:</span><span class="sxs-lookup"><span data-stu-id="dc167-154">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="dc167-155">Proxy sunucusu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="dc167-155">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="dc167-156">Internet'ten veya bir şirket ağı isteklerle etkileşim ve bir proxy'nin arkasında veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="dc167-156">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="dc167-157">Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="dc167-157">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="dc167-158">Kestrel uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="dc167-158">Kestrel endpoint configuration</span></span>

<span data-ttu-id="dc167-159">HTTPS yapılandırma ve SNI destek dahil olmak üzere Kestrel uç noktasını yapılandırma hakkında bilgi için bkz: [Kestrel uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="dc167-159">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
