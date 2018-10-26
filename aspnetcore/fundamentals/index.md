---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmaya yönelik temel kavramları keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: 56344315acc59003248ffaf1e61455b94a93a545
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090725"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="2e5dd-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="2e5dd-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="2e5dd-104">ASP.NET Core uygulaması bir web sunucusunu oluşturan bir konsol uygulaması olan kendi `Program.Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="2e5dd-105">`Main` Yöntemdir uygulamanın *yönetilen giriş noktasını*:</span><span class="sxs-lookup"><span data-stu-id="2e5dd-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="2e5dd-106">.NET Core ana bilgisayarı:</span><span class="sxs-lookup"><span data-stu-id="2e5dd-106">The .NET Core Host:</span></span>

* <span data-ttu-id="2e5dd-107">Yükleri [.NET Core çalışma zamanı](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="2e5dd-108">İlk komut satırı bağımsız değişkeni olarak giriş noktasını içeren yönetilen ikili dosya yolunu kullanır (`Main`) ve kod yürütmeyi başlatır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="2e5dd-109">`Main` Yöntemini çağıran [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web ana bilgisayarı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="2e5dd-110">Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-110">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="2e5dd-111">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="2e5dd-112">IIS üzerinde çalıştırmak ASP.NET Core'nın web ana bilgisayarı varsa çalışır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-112">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="2e5dd-113">Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="2e5dd-114">`UseStartup` açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-114">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="2e5dd-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, dönüş türünü `WebHost.CreateDefaultBuilder` çağrısı birçok isteğe bağlı yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="2e5dd-116">Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırmak ve <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> kök içerik dizini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="2e5dd-117"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> Ve <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> yöntemleri yapı <xref:Microsoft.AspNetCore.Hosting.IWebHost> nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="2e5dd-118">.NET Core ana bilgisayarı:</span><span class="sxs-lookup"><span data-stu-id="2e5dd-118">The .NET Core Host:</span></span>

* <span data-ttu-id="2e5dd-119">Yükleri [.NET Core çalışma zamanı](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="2e5dd-120">İlk komut satırı bağımsız değişkeni olarak giriş noktasını içeren yönetilen ikili dosya yolunu kullanır (`Main`) ve kod yürütmeyi başlatır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="2e5dd-121">`Main` Yöntemi kullanan <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web uygulama ana bilgisayarı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="2e5dd-122">Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="2e5dd-123">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="2e5dd-124">Diğer web sunucuları gibi [WebListener](xref:fundamentals/servers/weblistener), uygun bir genişletme yöntemi çağrılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="2e5dd-125">`UseStartup` açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="2e5dd-126">`WebHostBuilder` dahil olmak üzere birçok isteğe bağlı yöntemler sağlar <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> IIS ve IIS Express barındırmak ve <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> kök içerik dizini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="2e5dd-127"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> Ve <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> yöntemleri yapı <xref:Microsoft.AspNetCore.Hosting.IWebHost> nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="2e5dd-128">Başlangıç</span><span class="sxs-lookup"><span data-stu-id="2e5dd-128">Startup</span></span>

<span data-ttu-id="2e5dd-129">`UseStartup` Metodunda `WebHostBuilder` belirtir `Startup` uygulamanız için sınıf:</span><span class="sxs-lookup"><span data-stu-id="2e5dd-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="2e5dd-130">`Startup` Sınıfıdır istek işleme ardışık düzen tanımladığınız ve uygulama tarafından gereken diğer hizmetler yapılandırıldığı.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="2e5dd-131">`Startup` Sınıfı genel olmalıdır ve aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="2e5dd-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="2e5dd-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Core kimlik) tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="2e5dd-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> tanımlar [ara yazılım](xref:fundamentals/middleware/index) isteği işlem hattında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="2e5dd-134">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="2e5dd-135">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="2e5dd-135">Content root</span></span>

<span data-ttu-id="2e5dd-136">Temel yol gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfaları](xref:razor-pages/index), MVC görünümleri ve statik varlıklar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="2e5dd-137">Varsayılan olarak, uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynı konumda içerik kök dizinidir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="2e5dd-138">Web kökü (webroot)</span><span class="sxs-lookup"><span data-stu-id="2e5dd-138">Web root (webroot)</span></span>

<span data-ttu-id="2e5dd-139">Bir uygulamanın webroot CSS, JavaScript ve görüntü dosyaları gibi genel, statik kaynakları içeren proje dizinindedir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="2e5dd-140">Varsayılan olarak, *wwwroot* webroot olduğu.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="2e5dd-141">Razor için (*.cshtml*) dosyaları, eğik çizgi tilde `~/` webroot için işaret eder.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="2e5dd-142">İle başlayan yollar `~/` sanal yol adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="2e5dd-143">Bağımlılık ekleme (Hizmetler)</span><span class="sxs-lookup"><span data-stu-id="2e5dd-143">Dependency injection (services)</span></span>

<span data-ttu-id="2e5dd-144">A *hizmet* bir uygulamada ortak tüketim için hazırlanmış bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="2e5dd-145">Hizmetleri aracılığıyla yapılan [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2e5dd-146">ASP.NET Core içeren destekleyen yerel bir denetimi tersine çevirme (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="2e5dd-147">İsterseniz, varsayılan kapsayıcı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="2e5dd-148">Ek olarak kendi [kaybolmasını avantajı eşlenmesiyle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI Hizmetleri gibi yapar [günlüğü](xref:fundamentals/logging/index), uygulamanız genelinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="2e5dd-149">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="2e5dd-150">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="2e5dd-150">Middleware</span></span>

<span data-ttu-id="2e5dd-151">ASP.NET Core, istek kullanarak işlem hattı oluşturma [ara yazılım](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="2e5dd-152">ASP.NET Core ara yazılım üzerinde zaman uyumsuz işlemler gerçekleştiren bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="2e5dd-153">Kural gereği, çağırarak "XYZ" adlı bir ara yazılım bileşeni ardışık düzenine eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="2e5dd-154">ASP.NET Core içeren zengin bir yerleşik ara yazılım ve kendi özel bir ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="2e5dd-155">[.NET (OWIN) için açık Web arabirimi](xref:fundamentals/owin), web sunucuları, ölçeklendirilebilmeleri web apps sağlayan ASP.NET Core uygulamalarında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="2e5dd-156">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index> ve <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="2e5dd-157">HTTP isteklerini başlatma</span><span class="sxs-lookup"><span data-stu-id="2e5dd-157">Initiate HTTP requests</span></span>

<span data-ttu-id="2e5dd-158"><xref:System.Net.Http.IHttpClientFactory> erişmek kullanılabilir <xref:System.Net.Http.HttpClient> HTTP isteğinde bulunmak için örnekleri.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="2e5dd-159">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="2e5dd-160">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="2e5dd-160">Environments</span></span>

<span data-ttu-id="2e5dd-161">Ortamları gibi *geliştirme* ve *üretim*, ASP.NET Core, birinci sınıf bir kavram olan ve bir ortam değişkeni, ayarlar dosyası ve komut satırı bağımsız değişkeni kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="2e5dd-162">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="2e5dd-163">Barındırma</span><span class="sxs-lookup"><span data-stu-id="2e5dd-163">Hosting</span></span>

<span data-ttu-id="2e5dd-164">ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*, uygulama başlatma ve ömür yönetimi için sorumlu olduğu.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="2e5dd-165">Daha fazla bilgi için bkz. <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="2e5dd-166">Sunucular</span><span class="sxs-lookup"><span data-stu-id="2e5dd-166">Servers</span></span>

<span data-ttu-id="2e5dd-167">Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="2e5dd-168">Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="2e5dd-169">İletilen istek, arabirimler aracılığıyla erişilebilen bir özellik nesne olarak paketlenir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-169">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="2e5dd-170">ASP.NET Core içeren olarak adlandırılan bir yönetilen, platformlar arası web sunucusuna [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-170">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="2e5dd-171">Kestrel'i yaygın olarak çalıştığı bir üretim web sunucusu gibi [IIS](https://www.iis.net/) veya [Ngınx](http://nginx.org) ters Ara sunucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-171">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="2e5dd-172">Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-172">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="2e5dd-173">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-173">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="2e5dd-174">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2e5dd-174">Configuration</span></span>

<span data-ttu-id="2e5dd-175">ASP.NET Core ad-değer çiftlerine göre bir yapılandırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-175">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="2e5dd-176">Yapılandırma modeli temel almayan <xref:System.Configuration> veya *web.config*. Yapılandırma ayarları sıralı bir dizi yapılandırma sağlayıcıları alır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-176">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="2e5dd-177">Yerleşik yapılandırma sağlayıcıları, çeşitli dosya biçimleri (XML, JSON, INI), ortam değişkenleri ve komut satırı bağımsız değişkenleri destekler.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-177">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="2e5dd-178">Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-178">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="2e5dd-179">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="2e5dd-180">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="2e5dd-180">Logging</span></span>

<span data-ttu-id="2e5dd-181">ASP.NET Core bir çeşitli günlük sağlayıcılar ile çalışan API'si günlük kaydını destekler.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-181">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="2e5dd-182">Yerleşik sağlayıcılar, bir veya daha fazla hedefe gönderen günlükleri destekler.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-182">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="2e5dd-183">Üçüncü taraf günlük altyapılarına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-183">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="2e5dd-184">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-184">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="2e5dd-185">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="2e5dd-185">Error handling</span></span>

<span data-ttu-id="2e5dd-186">ASP.NET Core uygulamalarında Geliştirici özel durum sayfasında, özel hata sayfaları, statik durumu kod sayfaları ve başlangıç özel durum işleme dahil olmak üzere, hataları işleme için yerleşik senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-186">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="2e5dd-187">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-187">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="2e5dd-188">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="2e5dd-188">Routing</span></span>

<span data-ttu-id="2e5dd-189">ASP.NET Core, yol işleyicisi uygulama isteklerinin Yönlendirme senaryoları sunar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-189">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="2e5dd-190">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-190">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="2e5dd-191">Dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="2e5dd-191">File Providers</span></span>

<span data-ttu-id="2e5dd-192">ASP.NET Core, dosya sistemi erişimini kullanarak platformlar arasında dosyaları ile çalışma için ortak bir arabirim sunan dosya sağlayıcıları soyutlar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-192">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="2e5dd-193">Daha fazla bilgi için bkz. <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-193">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="2e5dd-194">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="2e5dd-194">Static files</span></span>

<span data-ttu-id="2e5dd-195">Statik dosya ara yazılımı, HTML, CSS, görüntü ve JavaScript dosyaları gibi statik dosyalar işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-195">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="2e5dd-196">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-196">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="2e5dd-197">Oturum ve uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="2e5dd-197">Session and app state</span></span>

<span data-ttu-id="2e5dd-198">ASP.NET Core, bir kullanıcı bir web uygulaması gözatar sırada oturum ve uygulama durumu korumak için çeşitli yaklaşımlar sunar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-198">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="2e5dd-199">Daha fazla bilgi için bkz. <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-199">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="2e5dd-200">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="2e5dd-200">Globalization and localization</span></span>

<span data-ttu-id="2e5dd-201">ASP.NET Core ile çok dilli bir Web sitesi oluşturma sitenizin daha geniş kitlelere ulaşmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="2e5dd-202">ASP.NET Core, içeriği yerelleştirmek için farklı dillere ve kültürlere Hizmetleri ve ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-202">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="2e5dd-203">Daha fazla bilgi için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-203">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="2e5dd-204">İstek özellikleri</span><span class="sxs-lookup"><span data-stu-id="2e5dd-204">Request features</span></span>

<span data-ttu-id="2e5dd-205">Web sunucusu uygulaması ayrıntıları HTTP istekleriyle ilgili ve yanıtları arabirimlerde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="2e5dd-206">Bu arabirimler, uygulamanın barındırma işlem hattı oluşturup için sunucu uygulamaları ve ara yazılım tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="2e5dd-207">Daha fazla bilgi için bkz. <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-207">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="2e5dd-208">Arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="2e5dd-208">Background tasks</span></span>

<span data-ttu-id="2e5dd-209">Arka plan görevleri olarak gerçekleştirilen *barındırılan hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-209">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="2e5dd-210">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-210">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="2e5dd-211">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-211">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="2e5dd-212">Erişim HttpContext</span><span class="sxs-lookup"><span data-stu-id="2e5dd-212">Access HttpContext</span></span>

<span data-ttu-id="2e5dd-213">`HttpContext` Razor sayfaları ve MVC isteklerini işleme sırasında otomatik olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-213">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="2e5dd-214">Durumlarda burada `HttpContext` değil kullanıma hazır erişebileceğiniz `HttpContext` aracılığıyla <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> arabirimi ve kendi varsayılan uygulama <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-214">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="2e5dd-215">Daha fazla bilgi için bkz. <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-215">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="2e5dd-216">WebSockets</span><span class="sxs-lookup"><span data-stu-id="2e5dd-216">WebSockets</span></span>

<span data-ttu-id="2e5dd-217">[WebSocket](https://wikipedia.org/wiki/WebSocket) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-217">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="2e5dd-218">Sohbet, yürütebilmektedir oyunlar gibi uygulamalar için kullanılır ve gerçek zamanlı bir web uygulaması işlevindeki istediğiniz herhangi bir yerde.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-218">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="2e5dd-219">ASP.NET Core web yuvası senaryolarını destekler.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-219">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="2e5dd-220">Daha fazla bilgi için bkz. <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-220">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="2e5dd-221">Microsoft.AspNetCore.App metapackage</span><span class="sxs-lookup"><span data-stu-id="2e5dd-221">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="2e5dd-222">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage paket Yönetimi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-222">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="2e5dd-223">Daha fazla bilgi için bkz. <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-223">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="2e5dd-224">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="2e5dd-224">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="2e5dd-225">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:</span><span class="sxs-lookup"><span data-stu-id="2e5dd-225">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="2e5dd-226">Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-226">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="2e5dd-227">Tüm paketleri Entity Framework Core tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-227">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="2e5dd-228">ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-228">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="2e5dd-229">Daha fazla bilgi için bkz. <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-229">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="2e5dd-230">.NET core ve .NET Framework çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="2e5dd-230">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="2e5dd-231">ASP.NET Core uygulaması, .NET Core veya .NET Framework çalışma zamanı hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-231">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="2e5dd-232">Daha fazla bilgi için [.NET Core ve .NET Framework arasında seçim yapma](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="2e5dd-232">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="2e5dd-233">ASP.NET Core ile ASP.NET arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="2e5dd-233">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="2e5dd-234">ASP.NET Core ile ASP.NET arasında seçim yapma hakkında daha fazla bilgi için bkz: <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="2e5dd-234">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
