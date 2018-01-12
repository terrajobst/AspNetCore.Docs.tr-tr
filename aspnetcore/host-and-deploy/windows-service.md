---
title: Bir Windows hizmeti ana bilgisayar
author: tdykstra
description: "Bir ASP.NET Core uygulamayı bir Windows hizmetinde barındırma öğrenin."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 03/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4bf64f54dd9c6d2a1706bd405b0d6d75d7573a7a
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="d4070-103">Konak bir Windows hizmetinde bir ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="d4070-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="d4070-104">tarafından [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d4070-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d4070-105">IIS kullanarak çalıştırmak için olmayan bir ASP.NET Core uygulama Windows üzerinde barındırmak için önerilen yöntem bir [Windows hizmeti](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="d4070-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="d4070-106">Bu şekilde otomatik olarak yeniden başlatmalar ve çökme (Crash), sonra oturum birine beklemeden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4070-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="d4070-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d4070-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d4070-108">Bkz: [sonraki adımlar](#next-steps) bölümü çalıştırmak yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="d4070-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4070-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d4070-109">Prerequisites</span></span>

* <span data-ttu-id="d4070-110">Uygulama, .NET Framework çalışma zamanı modülü çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d4070-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="d4070-111">İçinde *.csproj* dosyası, uygun değerlerini belirtin [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) ve [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="d4070-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="d4070-112">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="d4070-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="d4070-113">Visual Studio'da bir proje oluşturma sırasında kullanmak **ASP.NET Core uygulama (.NET Framework)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="d4070-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="d4070-114">Uygulama (yalnızca bir iç ağ üzerinden) Internet'ten gelen istekleri alırsa, kullanmalısınız [WebListener](xref:fundamentals/servers/weblistener) web sunucusu yerine [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d4070-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="d4070-115">Kestrel IIS ile kenar dağıtımlar için kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d4070-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="d4070-116">Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d4070-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="d4070-117">Başlarken</span><span class="sxs-lookup"><span data-stu-id="d4070-117">Getting started</span></span>

<span data-ttu-id="d4070-118">Bu bölümde, mevcut bir ASP.NET Core projeyi bir hizmet olarak çalıştırmak için gerekli minimum değişiklikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d4070-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="d4070-119">NuGet paket yüklemesi [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="d4070-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="d4070-120">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="d4070-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="d4070-121">Çağrı `host.RunAsService` yerine `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="d4070-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="d4070-122">Kod çağırırsa `UseContentRoot`, yerine yayımlama konumu için bir yol kullanın`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="d4070-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="d4070-123">Bir klasöre uygulama yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="d4070-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="d4070-124">Kullanım [dotnet yayımlama](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profili](xref:host-and-deploy/visual-studio-publish-profiles) bir klasöre yayımlar.</span><span class="sxs-lookup"><span data-stu-id="d4070-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="d4070-125">Test oluşturma ve hizmeti başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="d4070-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="d4070-126">Kullanmak için bir yönetici komut istemi penceresi açın [sc.exe](https://technet.microsoft.com/library/bb490995) oluşturmak ve bir hizmeti başlatmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="d4070-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="d4070-127">Hizmet Hizmetim adlı, uygulaması'na yayımlamak `c:\svc`ve uygulama AspNetCoreService adlı, komutları şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="d4070-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="d4070-128">`binPath` Yürütülebilir filename de dahil olmak üzere uygulamanın yürütülebilir dosya yolu bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="d4070-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![Konsol penceresi oluşturma ve örnek başlatma](windows-service/_static/create-start.png)

  <span data-ttu-id="d4070-130">Bu komutlar bitirdikten sonra aynı yol bir konsol uygulaması çalıştırırken göz atın (varsayılan olarak, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="d4070-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![Bir hizmet olarak çalışıyor](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="d4070-132">Hizmet dışında çalıştırmak için bir yol sağlama</span><span class="sxs-lookup"><span data-stu-id="d4070-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="d4070-133">Test ve çağıran kodu eklemek için her zamanki olacak şekilde bir hizmet dışında çalıştırılırken hata ayıklamak daha kolay `host.RunAsService` yalnızca belirli koşullar altında.</span><span class="sxs-lookup"><span data-stu-id="d4070-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="d4070-134">Örneğin, uygulama ile bir konsol uygulaması olarak çalıştırabilirsiniz bir `--console` hata ayıklayıcı bağlı değilse veya komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d4070-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="d4070-135">Durdurma ve başlatma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="d4070-135">Handle stopping and starting events</span></span>

<span data-ttu-id="d4070-136">İşlenecek `OnStarting`, `OnStarted`, ve `OnStopping` olayları, aşağıdaki ek değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="d4070-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="d4070-137">Türeyen bir sınıf oluşturun `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="d4070-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="d4070-138">Create genişletme yöntemi için `IWebHost` özel geçen `WebHostService` için `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="d4070-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="d4070-139">İçinde `Program.Main` değişiklik yerine yeni uzantı metodu çağırma `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="d4070-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="d4070-140">Varsa özel `WebHostService` kod gerekir (örneğin, bir Günlükçü) bağımlılık ekleme alanından bir hizmet almak için elde `Services` özelliği `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="d4070-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="d4070-141">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d4070-141">Next steps</span></span>

<span data-ttu-id="d4070-142">[Örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) bu eşlik makaledir kod örnekleri önceki gösterildiği gibi değiştirilmiş basit bir MVC web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d4070-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="d4070-143">Bunu bir hizmet olarak çalıştırmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="d4070-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="d4070-144">Yayımla *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="d4070-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="d4070-145">Bir yönetici penceresini açın.</span><span class="sxs-lookup"><span data-stu-id="d4070-145">Open an administrator window.</span></span>

* <span data-ttu-id="d4070-146">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="d4070-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="d4070-147">Bir tarayıcıda çalıştığını doğrulamak için http://localhost: 5000 için gidin.</span><span class="sxs-lookup"><span data-stu-id="d4070-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="d4070-148">Uygulamayı bir hizmet olarak çalıştırırken istendiği şekilde başlamazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="d4070-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="d4070-149">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d4070-149">Acknowledgments</span></span>

<span data-ttu-id="d4070-150">Bu makalede önceden yayımlanan kaynakları yardımıyla yazıldı.</span><span class="sxs-lookup"><span data-stu-id="d4070-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="d4070-151">Erken ve bunların en yararlı bunlar olan:</span><span class="sxs-lookup"><span data-stu-id="d4070-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="d4070-152">ASP.NET Core Windows hizmeti olarak barındırma</span><span class="sxs-lookup"><span data-stu-id="d4070-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="d4070-153">Bir Windows hizmetinde ASP.NET çekirdek barındırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="d4070-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
