---
title: Bir Windows hizmeti ana bilgisayar
author: tdykstra
description: "Bir ASP.NET Core uygulamayı bir Windows hizmetinde barındırma öğrenin."
keywords: "ASP.NET Core, Windows hizmeti barındırma"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: a6d1acf5ab8f40b0b4d487a6f34cd83d13907852
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="34759-104">Konak bir Windows hizmetinde bir ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="34759-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="34759-105">tarafından [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="34759-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="34759-106">Çalıştırmak için IIS kullanılmadığında Windows üzerinde bir ASP.NET Core uygulama barındırmak için önerilen yöntem olduğu bir [Windows hizmeti](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="34759-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="34759-107">Bu şekilde otomatik olarak yeniden başlatmalar ve çökme (Crash), sonra oturum birine beklemeden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34759-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="34759-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="34759-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="34759-109">Bkz: [sonraki adımlar](#next-steps) bölümü çalıştırmak yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="34759-109">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34759-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="34759-110">Prerequisites</span></span>

* <span data-ttu-id="34759-111">Uygulama, .NET Framework çalışma zamanı modülü çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="34759-111">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="34759-112">İçinde *.csproj* dosyası, uygun değerlerini belirtin [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) ve [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="34759-112">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="34759-113">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="34759-113">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="34759-114">Visual Studio'da bir proje oluşturma sırasında kullanmak **ASP.NET Core uygulama (.NET Framework)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="34759-114">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="34759-115">Uygulamasını (yalnızca bir iç ağ üzerinden) internet'ten isteklerini alacak, kullanmalısınız [WebListener](xref:fundamentals/servers/weblistener) web sunucusu yerine [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="34759-115">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="34759-116">Kestrel IIS ile kenar dağıtımlar için kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="34759-116">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="34759-117">Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="34759-117">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="34759-118">Başlarken</span><span class="sxs-lookup"><span data-stu-id="34759-118">Getting started</span></span>

<span data-ttu-id="34759-119">Bu bölümde, mevcut bir ASP.NET Core projeyi bir hizmet olarak çalıştırmak için gerekli minimum değişiklikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="34759-119">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="34759-120">NuGet paket yüklemesi [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="34759-120">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="34759-121">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="34759-121">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="34759-122">Çağrı `host.RunAsService` yerine `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="34759-122">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="34759-123">Kodunuzu çağırırsa `UseContentRoot`, yerine yayımlama konumu için bir yol kullanın`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="34759-123">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="34759-124">Bir klasöre uygulama yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="34759-124">Publish the application to a folder.</span></span>

  <span data-ttu-id="34759-125">Kullanım [dotnet yayımlama](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profili](xref:publishing/web-publishing-vs) bir klasöre yayımlar.</span><span class="sxs-lookup"><span data-stu-id="34759-125">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="34759-126">Test oluşturma ve hizmeti başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="34759-126">Test by creating and starting the service.</span></span>

  <span data-ttu-id="34759-127">Kullanmak için bir yönetici komut istemi penceresi açın [sc.exe](https://technet.microsoft.com/library/bb490995) oluşturmak ve bir hizmeti başlatmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="34759-127">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="34759-128">Hizmetinizi MyService ad kullanırsanız, uygulamanızın yayımlama `c:\svc`ve uygulama AspNetCoreService adlı, komutları şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="34759-128">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="34759-129">`binPath` Yürütülebilir filename de dahil olmak üzere, uygulamanızın yürütülebilir dosya yolu bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="34759-129">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Konsol penceresi oluşturma ve örnek başlatma](windows-service/_static/create-start.png)

  <span data-ttu-id="34759-131">Bu komutlar bitirdikten sonra bir konsol uygulaması çalıştırdığınızda olarak aynı yola göz atabilirsiniz (varsayılan olarak, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="34759-131">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![Bir hizmet olarak çalışıyor](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="34759-133">Hizmet dışında çalıştırmak için bir yol sağlama</span><span class="sxs-lookup"><span data-stu-id="34759-133">Provide a way to run outside of a service</span></span>

<span data-ttu-id="34759-134">Test ve çağıran kodu eklemek için her zamanki olacak şekilde bir hizmet dışında çalıştırırken hata ayıklamak daha kolay `host.RunAsService` yalnızca belirli koşullar altında.</span><span class="sxs-lookup"><span data-stu-id="34759-134">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="34759-135">Örneğin, bir konsol uygulaması gibi çalıştırabilir alırsanız bir `--console` hata ayıklayıcı bağlı değilse veya komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="34759-135">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="34759-136">Durdurma ve başlatma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="34759-136">Handle stopping and starting events</span></span>

<span data-ttu-id="34759-137">İşlemek istiyorsanız `OnStarting`, `OnStarted`, ve `OnStopping` olayları, aşağıdaki ek değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="34759-137">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="34759-138">Türeyen bir sınıf oluşturun `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="34759-138">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="34759-139">Create genişletme yöntemi için `IWebHost` özel geçen `WebHostService` için `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="34759-139">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="34759-140">İçinde `Program.Main` değişiklik yerine yeni uzantı metodu çağırma `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="34759-140">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="34759-141">Varsa özel `WebHostService` kodu hizmet bağımlılık ekleme (örneğin, bir Günlükçü) almanız gerekir, buradan edinebilirsiniz `Services` özelliği `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="34759-141">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="34759-142">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="34759-142">Next steps</span></span>

<span data-ttu-id="34759-143">[Örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) bu eşlik makaledir kod örnekleri önceki gösterildiği gibi değiştirilmiş basit bir MVC web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="34759-143">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="34759-144">Bunu bir hizmet olarak çalıştırmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="34759-144">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="34759-145">Yayımla *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="34759-145">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="34759-146">Bir yönetici penceresini açın.</span><span class="sxs-lookup"><span data-stu-id="34759-146">Open an administrator window.</span></span>

* <span data-ttu-id="34759-147">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="34759-147">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="34759-148">Bir tarayıcıda çalıştığını doğrulamak için http://localhost: 5000 için gidin.</span><span class="sxs-lookup"><span data-stu-id="34759-148">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="34759-149">Uygulamayı bir hizmet olarak çalıştırırken istendiği şekilde başlamazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="34759-149">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="34759-150">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="34759-150">Acknowledgments</span></span>

<span data-ttu-id="34759-151">Bu makalede önceden yayımlanan kaynakları yardımıyla yazıldı.</span><span class="sxs-lookup"><span data-stu-id="34759-151">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="34759-152">Erken ve bunların en yararlı bunlar olan:</span><span class="sxs-lookup"><span data-stu-id="34759-152">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="34759-153">ASP.NET Core Windows hizmeti olarak barındırma</span><span class="sxs-lookup"><span data-stu-id="34759-153">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="34759-154">Bir Windows hizmetinde ASP.NET çekirdek barındırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="34759-154">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
