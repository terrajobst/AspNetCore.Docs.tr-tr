---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamalar oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: fundamentals/index
ms.openlocfilehash: cff2afd62ed60648dc689d408dde56ecda18c261
ms.sourcegitcommit: 2d4c1732c4866ed26b83da35f7bc2ad021a9c701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70815646"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="abedd-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="abedd-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="abedd-104">Bu makale, ASP.NET Core uygulamaları geliştirmeyi anlamak için önemli konulara genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="abedd-105">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="abedd-105">The Startup class</span></span>

<span data-ttu-id="abedd-106">`Startup` Sınıfı şu konumda:</span><span class="sxs-lookup"><span data-stu-id="abedd-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="abedd-107">Uygulamanın gerektirdiği hizmetler yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="abedd-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="abedd-108">İstek işleme işlem hattı tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="abedd-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="abedd-109">*Hizmetler* , uygulama tarafından kullanılan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="abedd-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="abedd-110">Örneğin, bir günlük bileşeni bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="abedd-110">For example, a logging component is a service.</span></span> <span data-ttu-id="abedd-111">Hizmetleri yapılandırmak (veya *kaydettirmek*) için kod `Startup.ConfigureServices` yöntemine eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="abedd-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="abedd-112">İstek işleme işlem hattı, bir dizi *Ara yazılım* bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="abedd-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="abedd-113">Örneğin, bir ara yazılım statik dosyalar için istekleri işleyebilir veya HTTP isteklerini HTTPS 'ye yeniden yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="abedd-114">Her bir ara yazılım bir `HttpContext` üzerinde zaman uyumsuz işlemler gerçekleştirir ve ardından işlem hattında sonraki ara yazılımı çağırır veya isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="abedd-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="abedd-115">İstek işleme işlem hattını yapılandırma kodu `Startup.Configure` yöntemine eklenir.</span><span class="sxs-lookup"><span data-stu-id="abedd-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="abedd-116">Örnek `Startup` bir sınıf aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="abedd-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="abedd-117">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="abedd-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="abedd-118">Bağımlılık ekleme (hizmetler)</span><span class="sxs-lookup"><span data-stu-id="abedd-118">Dependency injection (services)</span></span>

<span data-ttu-id="abedd-119">ASP.NET Core, yapılandırılmış hizmetlerin bir uygulamanın sınıfları tarafından kullanılabilmesini sağlayan yerleşik bir bağımlılık ekleme (dı) çerçevesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="abedd-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="abedd-120">Bir sınıftaki hizmetin bir örneğini almanın bir yolu, gerekli türde bir parametreye sahip bir Oluşturucu oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="abedd-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="abedd-121">Parametresi hizmet türü veya arabirim olabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="abedd-122">Dı sistemi, çalışma zamanında hizmeti sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="abedd-123">İşte bir Entity Framework Core bağlam nesnesi almak için DI kullanan bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="abedd-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="abedd-124">Vurgulanan çizgi bir Oluşturucu Ekleme örneğidir:</span><span class="sxs-lookup"><span data-stu-id="abedd-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="abedd-125">Dı yerleşik olarak kullanılıyorsa, isterseniz Control (IOC) kapsayıcısının bir üçüncü taraf Inversion öğesini eklemenize olanak sağlamak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="abedd-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="abedd-126">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="abedd-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="abedd-127">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="abedd-127">Middleware</span></span>

<span data-ttu-id="abedd-128">İstek işleme işlem hattı, bir dizi ara yazılım bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="abedd-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="abedd-129">Her bileşen bir `HttpContext` üzerinde zaman uyumsuz işlemler gerçekleştirir ve ardından işlem hattında sonraki ara yazılımı çağırır veya isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="abedd-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="abedd-130">Kurala göre, `Use...` `Startup.Configure` yöntemi içindeki genişletme yöntemi çağrılarak işlem hattına bir ara yazılım bileşeni eklenir.</span><span class="sxs-lookup"><span data-stu-id="abedd-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="abedd-131">Örneğin, statik dosyaları işlemeyi etkinleştirmek için çağırın `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="abedd-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="abedd-132">Aşağıdaki örnekteki vurgulanan kod, istek işleme işlem hattını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="abedd-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="abedd-133">ASP.NET Core, zengin bir yerleşik ara yazılım kümesi içerir ve özel ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abedd-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="abedd-134">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="abedd-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="abedd-135">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="abedd-135">Host</span></span>

<span data-ttu-id="abedd-136">ASP.NET Core bir uygulama, başlangıçta bir *konak* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="abedd-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="abedd-137">Ana bilgisayar, uygulamanın tüm kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="abedd-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="abedd-138">Bir HTTP sunucusu uygulama</span><span class="sxs-lookup"><span data-stu-id="abedd-138">An HTTP server implementation</span></span>
* <span data-ttu-id="abedd-139">Ara yazılım bileşenleri</span><span class="sxs-lookup"><span data-stu-id="abedd-139">Middleware components</span></span>
* <span data-ttu-id="abedd-140">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="abedd-140">Logging</span></span>
* <span data-ttu-id="abedd-141">IÇERIK</span><span class="sxs-lookup"><span data-stu-id="abedd-141">DI</span></span>
* <span data-ttu-id="abedd-142">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="abedd-142">Configuration</span></span>

<span data-ttu-id="abedd-143">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="abedd-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="abedd-144">İki konak mevcuttur: genel ana bilgisayar ve Web ana bilgisayarı.</span><span class="sxs-lookup"><span data-stu-id="abedd-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="abedd-145">Genel ana bilgisayar önerilir ve Web konağı yalnızca geriye dönük uyumluluk için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="abedd-146">Bir konak `Program.Main`oluşturma kodu:</span><span class="sxs-lookup"><span data-stu-id="abedd-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="abedd-147">`CreateDefaultBuilder` Ve`ConfigureWebHostDefaults` yöntemleri, aşağıdaki gibi yaygın olarak kullanılan seçeneklerle bir ana bilgisayar yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="abedd-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="abedd-148">Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="abedd-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="abedd-149">*AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.</span><span class="sxs-lookup"><span data-stu-id="abedd-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="abedd-150">Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="abedd-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="abedd-151">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="abedd-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="abedd-152">İki konak mevcuttur: Web Konağı ve genel ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="abedd-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="abedd-153">ASP.NET Core 2. x içinde, genel konak yalnızca Web dışı senaryolar içindir.</span><span class="sxs-lookup"><span data-stu-id="abedd-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="abedd-154">Bir konak `Program.Main`oluşturma kodu:</span><span class="sxs-lookup"><span data-stu-id="abedd-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="abedd-155">`CreateDefaultBuilder` Yöntemi, aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir ana bilgisayar yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="abedd-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="abedd-156">Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="abedd-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="abedd-157">*AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.</span><span class="sxs-lookup"><span data-stu-id="abedd-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="abedd-158">Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="abedd-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="abedd-159">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="abedd-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="abedd-160">Web dışı senaryolar</span><span class="sxs-lookup"><span data-stu-id="abedd-160">Non-web scenarios</span></span>

<span data-ttu-id="abedd-161">Genel ana bilgisayar, diğer uygulama türlerinin günlüğe kaydetme, bağımlılık ekleme (dı), yapılandırma ve uygulama ömür yönetimi gibi çapraz kesme çerçevesi uzantıları kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="abedd-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="abedd-162">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="abedd-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="abedd-163">Sunucular</span><span class="sxs-lookup"><span data-stu-id="abedd-163">Servers</span></span>

<span data-ttu-id="abedd-164">Bir ASP.NET Core uygulaması HTTP isteklerini dinlemek için HTTP sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="abedd-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="abedd-165">Sunucu, bir `HttpContext`öğesine oluşturulan [istek özellikleri](xref:fundamentals/request-features) kümesi olarak uygulamaya istekleri ister.</span><span class="sxs-lookup"><span data-stu-id="abedd-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="abedd-166">Windows</span><span class="sxs-lookup"><span data-stu-id="abedd-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="abedd-167">ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="abedd-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="abedd-168">*Kestrel* , platformlar arası bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="abedd-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="abedd-169">Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="abedd-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="abedd-170">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="abedd-171">*IIS HTTP sunucusu* , IIS kullanan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="abedd-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="abedd-172">Bu sunucu ile, ASP.NET Core uygulaması ve IIS aynı işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="abedd-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="abedd-173">*Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="abedd-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="abedd-174">macOS</span><span class="sxs-lookup"><span data-stu-id="abedd-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="abedd-175">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="abedd-176">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="abedd-177">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="abedd-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="abedd-178">Linux</span><span class="sxs-lookup"><span data-stu-id="abedd-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="abedd-179">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="abedd-180">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="abedd-181">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="abedd-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="abedd-182">Windows</span><span class="sxs-lookup"><span data-stu-id="abedd-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="abedd-183">ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="abedd-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="abedd-184">*Kestrel* , platformlar arası bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="abedd-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="abedd-185">Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="abedd-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="abedd-186">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="abedd-187">*Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="abedd-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="abedd-188">macOS</span><span class="sxs-lookup"><span data-stu-id="abedd-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="abedd-189">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="abedd-190">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="abedd-191">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="abedd-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="abedd-192">Linux</span><span class="sxs-lookup"><span data-stu-id="abedd-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="abedd-193">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="abedd-194">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="abedd-195">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="abedd-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="abedd-196">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="abedd-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="abedd-197">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="abedd-197">Configuration</span></span>

<span data-ttu-id="abedd-198">ASP.NET Core, ayarları sıralı bir yapılandırma sağlayıcıları kümesinden ad-değer çiftleri olarak alan bir yapılandırma çerçevesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="abedd-199">*. JSON* dosyaları, *. xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri gibi çeşitli kaynaklar için yerleşik yapılandırma sağlayıcıları vardır.</span><span class="sxs-lookup"><span data-stu-id="abedd-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="abedd-200">Ayrıca, özel yapılandırma sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abedd-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="abedd-201">Örneğin, yapılandırmanın *appSettings. JSON* ve ortam değişkenlerinden geldiğini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abedd-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="abedd-202">Ardından, *ConnectionString* değeri istendiğinde, Framework ilk olarak *appSettings. JSON* dosyasına bakar.</span><span class="sxs-lookup"><span data-stu-id="abedd-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="abedd-203">Değer aynı zamanda bir ortam değişkeninde bulunursa, ortam değişkeninin değeri öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="abedd-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="abedd-204">Parolalar gibi gizli yapılandırma verilerini yönetmek için ASP.NET Core bir [gizli dizi Yöneticisi aracı](xref:security/app-secrets)sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="abedd-205">Üretim gizli dizileri için [Azure Key Vault](xref:security/key-vault-configuration)önerilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="abedd-206">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="abedd-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="abedd-207">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="abedd-207">Options</span></span>

<span data-ttu-id="abedd-208">Mümkün olduğunda yapılandırma değerlerini depolamak ve almak için *Seçenekler deseninin* ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="abedd-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="abedd-209">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="abedd-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="abedd-210">Örneğin, aşağıdaki kod WebSockets seçeneklerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="abedd-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="abedd-211">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="abedd-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="abedd-212">Lý</span><span class="sxs-lookup"><span data-stu-id="abedd-212">Environments</span></span>

<span data-ttu-id="abedd-213">*Geliştirme*, *hazırlık*ve *üretim*gibi yürütme ortamları, ASP.NET Core birinci sınıf kavramlardır.</span><span class="sxs-lookup"><span data-stu-id="abedd-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="abedd-214">`ASPNETCORE_ENVIRONMENT` Ortam değişkenini ayarlayarak bir uygulamanın çalıştığı ortamı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abedd-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="abedd-215">ASP.NET Core, uygulamanın başlangıcında bu ortam değişkenini okur ve değeri bir `IHostingEnvironment` uygulamada depolar.</span><span class="sxs-lookup"><span data-stu-id="abedd-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="abedd-216">Ortam nesnesi, uygulama tarafından her yerde DI aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="abedd-217">`Startup` Sınıfından aşağıdaki örnek kod, uygulamayı yalnızca geliştirmede çalıştırıldığında ayrıntılı hata bilgilerini sunacak şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="abedd-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="abedd-218">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="abedd-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="abedd-219">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="abedd-219">Logging</span></span>

<span data-ttu-id="abedd-220">ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler.</span><span class="sxs-lookup"><span data-stu-id="abedd-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="abedd-221">Kullanılabilir sağlayıcılar şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="abedd-221">Available providers include the following:</span></span>

* <span data-ttu-id="abedd-222">Konsol</span><span class="sxs-lookup"><span data-stu-id="abedd-222">Console</span></span>
* <span data-ttu-id="abedd-223">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="abedd-223">Debug</span></span>
* <span data-ttu-id="abedd-224">Windows üzerinde olay Izleme</span><span class="sxs-lookup"><span data-stu-id="abedd-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="abedd-225">Windows olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="abedd-225">Windows Event Log</span></span>
* <span data-ttu-id="abedd-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="abedd-226">TraceSource</span></span>
* <span data-ttu-id="abedd-227">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="abedd-227">Azure App Service</span></span>
* <span data-ttu-id="abedd-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="abedd-228">Azure Application Insights</span></span>

<span data-ttu-id="abedd-229">Dı ve çağrı günlüğü yöntemlerinden bir `ILogger` nesne alarak uygulamanın kodundaki her yerden günlükleri yazın.</span><span class="sxs-lookup"><span data-stu-id="abedd-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="abedd-230">Oluşturucu Ekleme ve günlük yöntemi çağrılarını vurgulanmış `ILogger` bir nesne kullanan örnek kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="abedd-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="abedd-231">`ILogger` Arabirim, günlük sağlayıcısına istediğiniz sayıda alanı geçirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="abedd-232">Alanlar genellikle bir ileti dizesi oluşturmak için kullanılır, ancak sağlayıcı bunları bir veri deposuna ayrı alanlar olarak da gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="abedd-233">Bu özellik, günlük sağlayıcılarının [yapılandırılmış günlük olarak da bilinen anlam günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="abedd-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="abedd-234">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="abedd-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="abedd-235">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="abedd-235">Routing</span></span>

<span data-ttu-id="abedd-236">*Yol* , bir işleyiciye EŞLENMIŞ bir URL örüncidir.</span><span class="sxs-lookup"><span data-stu-id="abedd-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="abedd-237">İşleyici genellikle bir Razor sayfası, MVC denetleyicisindeki bir eylem yöntemi veya bir ara yazılım olur.</span><span class="sxs-lookup"><span data-stu-id="abedd-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="abedd-238">ASP.NET Core yönlendirme, uygulamanız tarafından kullanılan URL 'Ler üzerinde denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="abedd-239">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="abedd-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="abedd-240">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="abedd-240">Error handling</span></span>

<span data-ttu-id="abedd-241">ASP.NET Core, hataları işlemeye yönelik yerleşik özellikler içerir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="abedd-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="abedd-242">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="abedd-242">A developer exception page</span></span>
* <span data-ttu-id="abedd-243">Özel hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="abedd-243">Custom error pages</span></span>
* <span data-ttu-id="abedd-244">Statik durum kodu sayfaları</span><span class="sxs-lookup"><span data-stu-id="abedd-244">Static status code pages</span></span>
* <span data-ttu-id="abedd-245">Başlatma özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="abedd-245">Startup exception handling</span></span>

<span data-ttu-id="abedd-246">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="abedd-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="abedd-247">HTTP isteğinde bulunma</span><span class="sxs-lookup"><span data-stu-id="abedd-247">Make HTTP requests</span></span>

<span data-ttu-id="abedd-248">Uygulamasının bir uygulamasına `IHttpClientFactory` örnek oluşturmak `HttpClient` için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="abedd-249">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="abedd-249">The factory:</span></span>

* <span data-ttu-id="abedd-250">, Mantıksal `HttpClient` örnekleri adlandırmak ve yapılandırmak için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="abedd-251">Örneğin, *GitHub istemcisi kayıtlı ve GitHub 'a* erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="abedd-252">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="abedd-253">, Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok temsilci seçme işleyicisinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="abedd-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="abedd-254">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="abedd-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="abedd-255">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="abedd-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="abedd-256">Geçici hata işleme için popüler bir üçüncü taraf kitaplığı olan *Polly*ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="abedd-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="abedd-257">Yaşam sürelerini el ile yönetirken `HttpClientMessageHandler` `HttpClient` gerçekleşen yaygın DNS sorunlarından kaçınmak için temeldeki örneklerin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="abedd-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="abedd-258">Fabrika tarafından oluşturulan istemciler aracılığıyla gönderilen tüm `ILogger`istekler için yapılandırılabilir bir günlüğe kaydetme deneyimi ekler (aracılığıyla).</span><span class="sxs-lookup"><span data-stu-id="abedd-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="abedd-259">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="abedd-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="abedd-260">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="abedd-260">Content root</span></span>

<span data-ttu-id="abedd-261">İçerik kökü, uygulama tarafından kullanılan Razor dosyaları gibi özel içeriklerin temel yoludur.</span><span class="sxs-lookup"><span data-stu-id="abedd-261">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="abedd-262">Varsayılan olarak, içerik kökü, uygulamayı barındıran yürütülebilir dosyanın temel yoludur.</span><span class="sxs-lookup"><span data-stu-id="abedd-262">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="abedd-263">[Konak oluşturulurken](#host)alternatif bir konum belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-263">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="abedd-264">Daha fazla bilgi için bkz. [içerik kökü](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="abedd-264">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="abedd-265">Daha fazla bilgi için bkz. [içerik kökü](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="abedd-265">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="abedd-266">Web kökü</span><span class="sxs-lookup"><span data-stu-id="abedd-266">Web root</span></span>

<span data-ttu-id="abedd-267">Web kökü ( *Webroot*olarak da bilinir), genel, STATIK kaynakların CSS, JavaScript ve resim dosyaları gibi temel yoludur.</span><span class="sxs-lookup"><span data-stu-id="abedd-267">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="abedd-268">Statik dosyalar ara yazılımı, dosyaları yalnızca Web kök dizininden (ve alt dizinlerden) varsayılan olarak sunar.</span><span class="sxs-lookup"><span data-stu-id="abedd-268">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="abedd-269">Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir konum belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="abedd-269">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="abedd-270">Daha fazla bilgi için bkz. [Webroot](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#webroot)</span><span class="sxs-lookup"><span data-stu-id="abedd-270">For more information, see [WebRoot](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#webroot)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="abedd-271">Daha fazla bilgi için bkz. [Web root](/aspnet/core/fundamentals/host/web-host#webroot).</span><span class="sxs-lookup"><span data-stu-id="abedd-271">For more information, see [Web root](/aspnet/core/fundamentals/host/web-host#webroot).</span></span>

::: moniker-end

<span data-ttu-id="abedd-272">Razor ( *. cshtml*) dosyalarında, tilde işareti `~/` Web köküne işaret eder.</span><span class="sxs-lookup"><span data-stu-id="abedd-272">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="abedd-273">İle `~/` başlayan yollar sanal yollar olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="abedd-273">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="abedd-274">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="abedd-274">For more information, see <xref:fundamentals/static-files>.</span></span>
