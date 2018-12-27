---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmaya yönelik temel kavramları keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/index
ms.openlocfilehash: 11dc6336ae7667038983c967f28232bef325f5bb
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637776"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="4db99-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="4db99-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="4db99-104">ASP.NET Core uygulaması bir web sunucusunu oluşturan bir konsol uygulaması olan kendi `Program.Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4db99-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="4db99-105">`Main` Yöntemdir uygulamanın *yönetilen giriş noktasını*:</span><span class="sxs-lookup"><span data-stu-id="4db99-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="4db99-106">.NET Core ana bilgisayarı:</span><span class="sxs-lookup"><span data-stu-id="4db99-106">The .NET Core Host:</span></span>

* <span data-ttu-id="4db99-107">Yükleri [.NET Core çalışma zamanı](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="4db99-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="4db99-108">İlk komut satırı bağımsız değişkeni olarak giriş noktasını içeren yönetilen ikili dosya yolunu kullanır (`Main`) ve kod yürütmeyi başlatır.</span><span class="sxs-lookup"><span data-stu-id="4db99-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="4db99-109">`Main` Yöntemini çağıran [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web ana bilgisayarı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="4db99-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="4db99-110">Oluşturucu bir web sunucusu tanımlayan yöntemleri vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="4db99-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="4db99-111">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır.</span><span class="sxs-lookup"><span data-stu-id="4db99-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="4db99-112">ASP.NET Core'nın web ana bilgisayarı denemeleri çalıştırmak [Internet Information Services (IIS)](https://www.iis.net/)varsa.</span><span class="sxs-lookup"><span data-stu-id="4db99-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="4db99-113">Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="4db99-114">`UseStartup` açıklanan daha ayrıntılı olarak [başlangıç](#startup) bölümü.</span><span class="sxs-lookup"><span data-stu-id="4db99-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="4db99-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, dönüş türünü `WebHost.CreateDefaultBuilder` çağrısı birçok isteğe bağlı yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="4db99-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="4db99-116">Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırmak ve <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> kök içerik dizini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="4db99-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="4db99-117"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> Ve <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> yöntemleri yapı <xref:Microsoft.AspNetCore.Hosting.IWebHost> nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.</span><span class="sxs-lookup"><span data-stu-id="4db99-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="4db99-118">.NET Core ana bilgisayarı:</span><span class="sxs-lookup"><span data-stu-id="4db99-118">The .NET Core Host:</span></span>

* <span data-ttu-id="4db99-119">Yükleri [.NET Core çalışma zamanı](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="4db99-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="4db99-120">İlk komut satırı bağımsız değişkeni olarak giriş noktasını içeren yönetilen ikili dosya yolunu kullanır (`Main`) ve kod yürütmeyi başlatır.</span><span class="sxs-lookup"><span data-stu-id="4db99-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="4db99-121">`Main` Yöntemi kullanan <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web uygulama ana bilgisayarı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="4db99-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="4db99-122">Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="4db99-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="4db99-123">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db99-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="4db99-124">Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-124">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="4db99-125">`UseStartup` açıklanan daha ayrıntılı olarak [başlangıç](#startup) bölümü.</span><span class="sxs-lookup"><span data-stu-id="4db99-125">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="4db99-126">`WebHostBuilder` dahil olmak üzere birçok isteğe bağlı yöntemler sağlar <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> IIS ve IIS Express barındırmak ve <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> kök içerik dizini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="4db99-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="4db99-127"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> Ve <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> yöntemleri yapı <xref:Microsoft.AspNetCore.Hosting.IWebHost> nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.</span><span class="sxs-lookup"><span data-stu-id="4db99-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="4db99-128">Başlangıç</span><span class="sxs-lookup"><span data-stu-id="4db99-128">Startup</span></span>

<span data-ttu-id="4db99-129">`UseStartup` Metodunda `WebHostBuilder` belirtir `Startup` uygulamanız için sınıf:</span><span class="sxs-lookup"><span data-stu-id="4db99-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="4db99-130">`Startup` Sınıfıdır istek işleme ardışık düzen tanımladığınız ve uygulama tarafından gereken diğer hizmetler yapılandırıldığı.</span><span class="sxs-lookup"><span data-stu-id="4db99-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="4db99-131">`Startup` Sınıfı genel olmalıdır ve aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="4db99-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="4db99-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Core kimlik) tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db99-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="4db99-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> tanımlar [ara yazılım](xref:fundamentals/middleware/index) isteği işlem hattında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4db99-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="4db99-134">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="4db99-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="4db99-135">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="4db99-135">Content root</span></span>

<span data-ttu-id="4db99-136">Temel yol gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfaları](xref:razor-pages/index), MVC görünümleri ve statik varlıklar.</span><span class="sxs-lookup"><span data-stu-id="4db99-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="4db99-137">Varsayılan olarak, uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynı konumda içerik kök dizinidir.</span><span class="sxs-lookup"><span data-stu-id="4db99-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="4db99-138">Web kökü (webroot)</span><span class="sxs-lookup"><span data-stu-id="4db99-138">Web root (webroot)</span></span>

<span data-ttu-id="4db99-139">Bir uygulamanın webroot CSS, JavaScript ve görüntü dosyaları gibi genel, statik kaynakları içeren proje dizinindedir.</span><span class="sxs-lookup"><span data-stu-id="4db99-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="4db99-140">Varsayılan olarak, *wwwroot* webroot olduğu.</span><span class="sxs-lookup"><span data-stu-id="4db99-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="4db99-141">Razor için (*.cshtml*) dosyaları, eğik çizgi tilde `~/` webroot için işaret eder.</span><span class="sxs-lookup"><span data-stu-id="4db99-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="4db99-142">İle başlayan yollar `~/` sanal yol adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4db99-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="4db99-143">Bağımlılık ekleme (Hizmetler)</span><span class="sxs-lookup"><span data-stu-id="4db99-143">Dependency injection (services)</span></span>

<span data-ttu-id="4db99-144">A *hizmet* bir uygulamada ortak tüketim için hazırlanmış bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="4db99-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="4db99-145">Hizmetleri aracılığıyla yapılan [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4db99-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4db99-146">ASP.NET Core içeren destekleyen yerel bir denetimi tersine çevirme (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="4db99-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="4db99-147">İsterseniz, varsayılan kapsayıcı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4db99-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="4db99-148">Ek olarak kendi [kaybolmasını avantajı eşlenmesiyle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI Hizmetleri gibi yapar [günlüğü](xref:fundamentals/logging/index), uygulamanız genelinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="4db99-149">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="4db99-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="4db99-150">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="4db99-150">Middleware</span></span>

<span data-ttu-id="4db99-151">ASP.NET Core, istek kullanarak işlem hattı oluşturma [ara yazılım](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="4db99-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="4db99-152">ASP.NET Core ara yazılım üzerinde zaman uyumsuz işlemler gerçekleştiren bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="4db99-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="4db99-153">Kural gereği, çağırarak "XYZ" adlı bir ara yazılım bileşeni ardışık düzenine eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4db99-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="4db99-154">ASP.NET Core içeren zengin bir yerleşik ara yazılım ve kendi özel bir ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4db99-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="4db99-155">[.NET (OWIN) için açık Web arabirimi](xref:fundamentals/owin), web sunucuları, ölçeklendirilebilmeleri web apps sağlayan ASP.NET Core uygulamalarında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4db99-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="4db99-156">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index> ve <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="4db99-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="4db99-157">HTTP isteklerini başlatma</span><span class="sxs-lookup"><span data-stu-id="4db99-157">Initiate HTTP requests</span></span>

<span data-ttu-id="4db99-158"><xref:System.Net.Http.IHttpClientFactory> erişmek kullanılabilir <xref:System.Net.Http.HttpClient> HTTP isteğinde bulunmak için örnekleri.</span><span class="sxs-lookup"><span data-stu-id="4db99-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="4db99-159">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="4db99-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="4db99-160">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="4db99-160">Environments</span></span>

<span data-ttu-id="4db99-161">Ortamları gibi *geliştirme* ve *üretim*, ASP.NET Core, birinci sınıf bir kavram olan ve bir ortam değişkeni, ayarlar dosyası ve komut satırı bağımsız değişkeni kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="4db99-162">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="4db99-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="4db99-163">Barındırma</span><span class="sxs-lookup"><span data-stu-id="4db99-163">Hosting</span></span>

<span data-ttu-id="4db99-164">ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*, uygulama başlatma ve ömür yönetimi için sorumlu olduğu.</span><span class="sxs-lookup"><span data-stu-id="4db99-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="4db99-165">Daha fazla bilgi için bkz. <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="4db99-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="4db99-166">Sunucular</span><span class="sxs-lookup"><span data-stu-id="4db99-166">Servers</span></span>

<span data-ttu-id="4db99-167">Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez.</span><span class="sxs-lookup"><span data-stu-id="4db99-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="4db99-168">Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db99-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="4db99-169">Windows</span><span class="sxs-lookup"><span data-stu-id="4db99-169">Windows</span></span>](#tab/windows)

<span data-ttu-id="4db99-170">ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="4db99-170">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="4db99-171">[Kestrel'i](xref:fundamentals/servers/kestrel) yönetilen, platformlar arası web sunucusu sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-171">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="4db99-172">Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="4db99-172">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="4db99-173">Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-173">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="4db99-174">IIS HTTP sunucusu (`IISHttpServer`) olan bir [işlem sunucusu](xref:fundamentals/servers/index#in-process-hosting-model) IIS için.</span><span class="sxs-lookup"><span data-stu-id="4db99-174">IIS HTTP Server (`IISHttpServer`) is an [in-process server](xref:fundamentals/servers/index#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="4db99-175">[HTTP.sys](xref:fundamentals/servers/httpsys) sunucusu, Windows üzerinde ASP.NET Core bir web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-175">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="4db99-176">macOS</span><span class="sxs-lookup"><span data-stu-id="4db99-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="4db99-177">ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4db99-177">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="4db99-178">Kestrel'i yönetilen, platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-178">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="4db99-179">Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-179">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4db99-180">Linux</span><span class="sxs-lookup"><span data-stu-id="4db99-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="4db99-181">ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4db99-181">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="4db99-182">Kestrel'i yönetilen, platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-182">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="4db99-183">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](http://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="4db99-183">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="4db99-184">Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-184">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="4db99-185">Windows</span><span class="sxs-lookup"><span data-stu-id="4db99-185">Windows</span></span>](#tab/windows)

<span data-ttu-id="4db99-186">ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="4db99-186">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="4db99-187">[Kestrel'i](xref:fundamentals/servers/kestrel) yönetilen, platformlar arası web sunucusu sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-187">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="4db99-188">Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="4db99-188">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="4db99-189">Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-189">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="4db99-190">[HTTP.sys](xref:fundamentals/servers/httpsys) sunucusu, Windows üzerinde ASP.NET Core bir web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-190">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="4db99-191">macOS</span><span class="sxs-lookup"><span data-stu-id="4db99-191">macOS</span></span>](#tab/macos)

<span data-ttu-id="4db99-192">ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4db99-192">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="4db99-193">Kestrel'i yönetilen, platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-193">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="4db99-194">Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-194">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4db99-195">Linux</span><span class="sxs-lookup"><span data-stu-id="4db99-195">Linux</span></span>](#tab/linux)

<span data-ttu-id="4db99-196">ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4db99-196">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="4db99-197">Kestrel'i yönetilen, platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db99-197">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="4db99-198">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](http://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="4db99-198">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="4db99-199">Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-199">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="4db99-200">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="4db99-200">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="4db99-201">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4db99-201">Configuration</span></span>

<span data-ttu-id="4db99-202">ASP.NET Core ad-değer çiftlerine göre bir yapılandırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db99-202">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="4db99-203">Yapılandırma modeli temel almayan <xref:System.Configuration> veya *web.config*. Yapılandırma ayarları sıralı bir dizi yapılandırma sağlayıcıları alır.</span><span class="sxs-lookup"><span data-stu-id="4db99-203">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="4db99-204">Yerleşik yapılandırma sağlayıcıları, çeşitli dosya biçimleri (XML, JSON, INI), ortam değişkenleri ve komut satırı bağımsız değişkenleri destekler.</span><span class="sxs-lookup"><span data-stu-id="4db99-204">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="4db99-205">Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4db99-205">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="4db99-206">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="4db99-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="4db99-207">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="4db99-207">Logging</span></span>

<span data-ttu-id="4db99-208">ASP.NET Core bir çeşitli günlük sağlayıcılar ile çalışan API'si günlük kaydını destekler.</span><span class="sxs-lookup"><span data-stu-id="4db99-208">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="4db99-209">Yerleşik sağlayıcılar, bir veya daha fazla hedefe gönderen günlükleri destekler.</span><span class="sxs-lookup"><span data-stu-id="4db99-209">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="4db99-210">Üçüncü taraf günlük altyapılarına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-210">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="4db99-211">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="4db99-211">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="4db99-212">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="4db99-212">Error handling</span></span>

<span data-ttu-id="4db99-213">ASP.NET Core uygulamalarında Geliştirici özel durum sayfasında, özel hata sayfaları, statik durumu kod sayfaları ve başlangıç özel durum işleme dahil olmak üzere, hataları işleme için yerleşik senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="4db99-213">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="4db99-214">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="4db99-214">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="4db99-215">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4db99-215">Routing</span></span>

<span data-ttu-id="4db99-216">ASP.NET Core, yol işleyicisi uygulama isteklerinin Yönlendirme senaryoları sunar.</span><span class="sxs-lookup"><span data-stu-id="4db99-216">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="4db99-217">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="4db99-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="4db99-218">Arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="4db99-218">Background tasks</span></span>

<span data-ttu-id="4db99-219">Arka plan görevleri olarak gerçekleştirilen *barındırılan hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="4db99-219">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="4db99-220">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="4db99-220">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="4db99-221">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="4db99-221">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="4db99-222">Erişim HttpContext</span><span class="sxs-lookup"><span data-stu-id="4db99-222">Access HttpContext</span></span>

<span data-ttu-id="4db99-223">`HttpContext` Razor sayfaları ve MVC isteklerini işleme sırasında otomatik olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db99-223">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="4db99-224">Durumlarda burada `HttpContext` değil kullanıma hazır erişebileceğiniz `HttpContext` aracılığıyla <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> arabirimi ve kendi varsayılan uygulama <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="4db99-224">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="4db99-225">Daha fazla bilgi için bkz. <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="4db99-225">For more information, see <xref:fundamentals/httpcontext>.</span></span>
