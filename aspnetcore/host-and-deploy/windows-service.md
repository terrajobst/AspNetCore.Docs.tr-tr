---
title: "Bir Windows hizmetinde konak ASP.NET Çekirdeği"
author: tdykstra
description: "Bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar öğrenin."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: f3455e47cfc06a4492dc4e34871b348184c6ecfb
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="336bd-103">Bir Windows hizmetinde konak ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="336bd-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="336bd-104">By [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="336bd-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="336bd-105">IIS kullanarak çalıştırmak için olmayan bir ASP.NET Core uygulama Windows üzerinde barındırmak için önerilen yöntem bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="336bd-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="336bd-106">Bir Windows hizmeti olarak barındırıldığında, uygulama otomatik olarak şunları yapabilecek başladıktan sonra bilgisayarı yeniden başlatır ve insan etkileşimi gerektirmeden çöküyor.</span><span class="sxs-lookup"><span data-stu-id="336bd-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="336bd-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="336bd-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="336bd-108">Örnek uygulamayı çalıştırmak yönergeler görmek için örnek 's *README.md* dosya.</span><span class="sxs-lookup"><span data-stu-id="336bd-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="336bd-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="336bd-109">Prerequisites</span></span>

* <span data-ttu-id="336bd-110">Uygulama, .NET Framework çalışma zamanı modülü çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="336bd-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="336bd-111">İçinde *.csproj* dosyası, uygun değerlerini belirtin [TargetFramework](/nuget/schema/target-frameworks) ve [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="336bd-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="336bd-112">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="336bd-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="336bd-113">Visual Studio'da bir proje oluşturma sırasında kullanmak **ASP.NET Core uygulama (.NET Framework)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="336bd-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="336bd-114">Uygulama (yalnızca bir iç ağ üzerinden) Internet'ten gelen istekleri alırsa, kullanmalısınız [HTTP.sys](xref:fundamentals/servers/httpsys) web sunucusu (önceki adıyla bilinen [WebListener](xref:fundamentals/servers/weblistener) ASP.NET Core 1.x uygulamalar için) yerine[Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="336bd-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="336bd-115">IIS bir ters proxy sunucusu olarak kullanmak için Kestrel ile kenar dağıtımları için önerilir.</span><span class="sxs-lookup"><span data-stu-id="336bd-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="336bd-116">Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="336bd-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="336bd-117">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="336bd-117">Get started</span></span>

<span data-ttu-id="336bd-118">Bu bölümde, mevcut bir ASP.NET Core projeyi bir hizmet olarak çalıştırmak için gerekli minimum değişiklikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="336bd-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="336bd-119">NuGet paket yüklemesi [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="336bd-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="336bd-120">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="336bd-120">Make the following changes in `Program.Main`:</span></span>
  
   * <span data-ttu-id="336bd-121">Çağrı `host.RunAsService` yerine `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="336bd-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
   * <span data-ttu-id="336bd-122">Kod çağırırsa `UseContentRoot`, yerine yayımlama konumu için bir yol kullanmak `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="336bd-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="336bd-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="336bd-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="336bd-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="336bd-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

1. <span data-ttu-id="336bd-125">Uygulamayı bir klasöre yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="336bd-125">Publish the app to a folder.</span></span> <span data-ttu-id="336bd-126">Kullanım [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profili](xref:host-and-deploy/visual-studio-publish-profiles) bir klasöre yayımlar.</span><span class="sxs-lookup"><span data-stu-id="336bd-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

1. <span data-ttu-id="336bd-127">Test oluşturma ve hizmeti başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="336bd-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="336bd-128">Kullanmak için yönetici ayrıcalıklarıyla bir komut kabuğu'nu açın [sc.exe](https://technet.microsoft.com/library/bb490995) oluşturmak ve bir hizmeti başlatmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="336bd-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="336bd-129">Hizmet Hizmetim adlı, yayımlanan `c:\svc`, ve AspNetCoreService adlı, komutlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="336bd-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="336bd-130">`binPath` Yürütülebilir dosya adı içeren uygulamanın yürütülebilir dosya yolu bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="336bd-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Konsol penceresi oluşturma ve örnek başlatma](windows-service/_static/create-start.png)

   <span data-ttu-id="336bd-132">Bu komutlar bitirdikten sonra aynı yol bir konsol uygulaması çalıştırırken göz atın (varsayılan olarak, `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="336bd-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Bir hizmet olarak çalışıyor](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="336bd-134">Hizmet dışında çalıştırmak için bir yol sağlama</span><span class="sxs-lookup"><span data-stu-id="336bd-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="336bd-135">Test ve çağıran kodu eklemek için her zamanki olacak şekilde bir hizmet dışında çalıştırılırken hata ayıklamak daha kolay `RunAsService` yalnızca belirli koşullar altında.</span><span class="sxs-lookup"><span data-stu-id="336bd-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="336bd-136">Örneğin, uygulama ile bir konsol uygulaması olarak çalıştırabilirsiniz bir `--console` komut satırı bağımsız değişkeni ya da hata ayıklayıcısı ekli varsa:</span><span class="sxs-lookup"><span data-stu-id="336bd-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="336bd-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="336bd-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="336bd-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="336bd-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="336bd-139">Durdurma ve başlatma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="336bd-139">Handle stopping and starting events</span></span>

<span data-ttu-id="336bd-140">İşlenecek `OnStarting`, `OnStarted`, ve `OnStopping` olayları, aşağıdaki ek değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="336bd-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="336bd-141">Türeyen bir sınıf oluşturun `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="336bd-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

1. <span data-ttu-id="336bd-142">Create genişletme yöntemi için `IWebHost` özel geçen `WebHostService` için `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="336bd-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

1. <span data-ttu-id="336bd-143">İçinde `Program.Main`, yeni uzantı metodu çağırma `RunAsCustomService`, yerine `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="336bd-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="336bd-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="336bd-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="336bd-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="336bd-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="336bd-146">Varsa özel `WebHostService` kod bağımlılık ekleme (örneğin, bir Günlükçü) hizmetinden gerektirir, buradan edinin `Services` özelliği `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="336bd-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="acknowledgments"></a><span data-ttu-id="336bd-147">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="336bd-147">Acknowledgments</span></span>

<span data-ttu-id="336bd-148">Bu makale, yayımlanan kaynaklarının yardımıyla yazılmıştır:</span><span class="sxs-lookup"><span data-stu-id="336bd-148">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="336bd-149">ASP.NET Core Windows hizmeti olarak barındırma</span><span class="sxs-lookup"><span data-stu-id="336bd-149">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="336bd-150">Bir Windows hizmetinde ASP.NET çekirdek barındırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="336bd-150">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
