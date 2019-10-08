---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamalar oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/index
ms.openlocfilehash: a70d6aa05a2c92d19076b8d6e4ea24d7554368b6
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007123"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="827a7-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="827a7-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="827a7-104">Bu makale, ASP.NET Core uygulamaları geliştirmeyi anlamak için önemli konulara genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="827a7-105">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="827a7-105">The Startup class</span></span>

<span data-ttu-id="827a7-106">@No__t-0 sınıfı şu konumda:</span><span class="sxs-lookup"><span data-stu-id="827a7-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="827a7-107">Uygulamanın gerektirdiği hizmetler yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="827a7-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="827a7-108">İstek işleme işlem hattı tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="827a7-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="827a7-109">*Hizmetler* , uygulama tarafından kullanılan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="827a7-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="827a7-110">Örneğin, bir günlük bileşeni bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="827a7-110">For example, a logging component is a service.</span></span> <span data-ttu-id="827a7-111">Hizmetleri yapılandırmak (veya *kaydettirmek*) için kod `Startup.ConfigureServices` yöntemine eklenir.</span><span class="sxs-lookup"><span data-stu-id="827a7-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="827a7-112">İstek işleme işlem hattı, bir dizi *Ara yazılım* bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="827a7-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="827a7-113">Örneğin, bir ara yazılım statik dosyalar için istekleri işleyebilir veya HTTP isteklerini HTTPS 'ye yeniden yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="827a7-114">Her bir ara yazılım `HttpContext` ' da zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır veya isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="827a7-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="827a7-115">İstek işleme işlem hattını yapılandırma kodu `Startup.Configure` yöntemine eklenir.</span><span class="sxs-lookup"><span data-stu-id="827a7-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="827a7-116">Örnek bir `Startup` sınıfı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="827a7-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="827a7-117">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="827a7-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="827a7-118">Bağımlılık ekleme (hizmetler)</span><span class="sxs-lookup"><span data-stu-id="827a7-118">Dependency injection (services)</span></span>

<span data-ttu-id="827a7-119">ASP.NET Core, yapılandırılmış hizmetlerin bir uygulamanın sınıfları tarafından kullanılabilmesini sağlayan yerleşik bir bağımlılık ekleme (dı) çerçevesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="827a7-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="827a7-120">Bir sınıftaki hizmetin bir örneğini almanın bir yolu, gerekli türde bir parametreye sahip bir Oluşturucu oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="827a7-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="827a7-121">Parametresi hizmet türü veya arabirim olabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="827a7-122">Dı sistemi, çalışma zamanında hizmeti sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="827a7-123">İşte bir Entity Framework Core bağlam nesnesi almak için DI kullanan bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="827a7-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="827a7-124">Vurgulanan çizgi bir Oluşturucu Ekleme örneğidir:</span><span class="sxs-lookup"><span data-stu-id="827a7-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="827a7-125">Dı yerleşik olarak kullanılıyorsa, isterseniz Control (IOC) kapsayıcısının bir üçüncü taraf Inversion öğesini eklemenize olanak sağlamak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="827a7-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="827a7-126">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="827a7-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="827a7-127">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="827a7-127">Middleware</span></span>

<span data-ttu-id="827a7-128">İstek işleme işlem hattı, bir dizi ara yazılım bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="827a7-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="827a7-129">Her bileşen `HttpContext` ' da zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır veya isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="827a7-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="827a7-130">Kurala göre, `Startup.Configure` yönteminde `Use...` genişletme yöntemini çağırarak işlem hattına bir ara yazılım bileşeni eklenir.</span><span class="sxs-lookup"><span data-stu-id="827a7-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="827a7-131">Örneğin, statik dosyaların işlenmesini etkinleştirmek için `UseStaticFiles` ' ı çağırın.</span><span class="sxs-lookup"><span data-stu-id="827a7-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="827a7-132">Aşağıdaki örnekteki vurgulanan kod, istek işleme işlem hattını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="827a7-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="827a7-133">ASP.NET Core, zengin bir yerleşik ara yazılım kümesi içerir ve özel ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="827a7-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="827a7-134">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="827a7-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="827a7-135">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="827a7-135">Host</span></span>

<span data-ttu-id="827a7-136">ASP.NET Core bir uygulama, başlangıçta bir *konak* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="827a7-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="827a7-137">Ana bilgisayar, uygulamanın tüm kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="827a7-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="827a7-138">Bir HTTP sunucusu uygulama</span><span class="sxs-lookup"><span data-stu-id="827a7-138">An HTTP server implementation</span></span>
* <span data-ttu-id="827a7-139">Ara yazılım bileşenleri</span><span class="sxs-lookup"><span data-stu-id="827a7-139">Middleware components</span></span>
* <span data-ttu-id="827a7-140">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="827a7-140">Logging</span></span>
* <span data-ttu-id="827a7-141">IÇERIK</span><span class="sxs-lookup"><span data-stu-id="827a7-141">DI</span></span>
* <span data-ttu-id="827a7-142">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="827a7-142">Configuration</span></span>

<span data-ttu-id="827a7-143">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="827a7-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="827a7-144">İki konak mevcuttur: genel ana bilgisayar ve Web ana bilgisayarı.</span><span class="sxs-lookup"><span data-stu-id="827a7-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="827a7-145">Genel ana bilgisayar önerilir ve Web konağı yalnızca geriye dönük uyumluluk için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="827a7-146">Bir konak oluşturma kodu `Program.Main` ' dır:</span><span class="sxs-lookup"><span data-stu-id="827a7-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="827a7-147">@No__t-0 ve `ConfigureWebHostDefaults` yöntemleri, aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir konak yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="827a7-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="827a7-148">Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="827a7-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="827a7-149">*AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.</span><span class="sxs-lookup"><span data-stu-id="827a7-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="827a7-150">Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="827a7-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="827a7-151">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="827a7-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="827a7-152">İki konak mevcuttur: Web Konağı ve genel ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="827a7-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="827a7-153">ASP.NET Core 2. x içinde, genel konak yalnızca Web dışı senaryolar içindir.</span><span class="sxs-lookup"><span data-stu-id="827a7-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="827a7-154">Bir konak oluşturma kodu `Program.Main` ' dır:</span><span class="sxs-lookup"><span data-stu-id="827a7-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="827a7-155">@No__t-0 yöntemi, aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir konak yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="827a7-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="827a7-156">Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="827a7-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="827a7-157">*AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.</span><span class="sxs-lookup"><span data-stu-id="827a7-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="827a7-158">Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="827a7-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="827a7-159">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="827a7-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="827a7-160">Web dışı senaryolar</span><span class="sxs-lookup"><span data-stu-id="827a7-160">Non-web scenarios</span></span>

<span data-ttu-id="827a7-161">Genel ana bilgisayar, diğer uygulama türlerinin günlüğe kaydetme, bağımlılık ekleme (dı), yapılandırma ve uygulama ömür yönetimi gibi çapraz kesme çerçevesi uzantıları kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="827a7-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="827a7-162">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="827a7-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="827a7-163">Sunucular</span><span class="sxs-lookup"><span data-stu-id="827a7-163">Servers</span></span>

<span data-ttu-id="827a7-164">Bir ASP.NET Core uygulaması HTTP isteklerini dinlemek için HTTP sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="827a7-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="827a7-165">Sunucu, `HttpContext` ' e oluşturulan [istek özellikleri](xref:fundamentals/request-features) kümesi olarak uygulamaya ister.</span><span class="sxs-lookup"><span data-stu-id="827a7-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="827a7-166">Windows</span><span class="sxs-lookup"><span data-stu-id="827a7-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="827a7-167">ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="827a7-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="827a7-168">*Kestrel* , platformlar arası bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="827a7-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="827a7-169">Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="827a7-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="827a7-170">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="827a7-171">*IIS HTTP sunucusu* , IIS kullanan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="827a7-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="827a7-172">Bu sunucu ile, ASP.NET Core uygulaması ve IIS aynı işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="827a7-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="827a7-173">*Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="827a7-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="827a7-174">macOS</span><span class="sxs-lookup"><span data-stu-id="827a7-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="827a7-175">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="827a7-176">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="827a7-177">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="827a7-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="827a7-178">Linux</span><span class="sxs-lookup"><span data-stu-id="827a7-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="827a7-179">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="827a7-180">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="827a7-181">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="827a7-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="827a7-182">Windows</span><span class="sxs-lookup"><span data-stu-id="827a7-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="827a7-183">ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="827a7-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="827a7-184">*Kestrel* , platformlar arası bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="827a7-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="827a7-185">Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="827a7-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="827a7-186">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="827a7-187">*Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="827a7-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="827a7-188">macOS</span><span class="sxs-lookup"><span data-stu-id="827a7-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="827a7-189">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="827a7-190">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="827a7-191">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="827a7-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="827a7-192">Linux</span><span class="sxs-lookup"><span data-stu-id="827a7-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="827a7-193">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="827a7-194">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="827a7-195">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="827a7-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="827a7-196">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="827a7-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="827a7-197">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="827a7-197">Configuration</span></span>

<span data-ttu-id="827a7-198">ASP.NET Core, ayarları sıralı bir yapılandırma sağlayıcıları kümesinden ad-değer çiftleri olarak alan bir yapılandırma çerçevesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="827a7-199">*. JSON* dosyaları, *. xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri gibi çeşitli kaynaklar için yerleşik yapılandırma sağlayıcıları vardır.</span><span class="sxs-lookup"><span data-stu-id="827a7-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="827a7-200">Ayrıca, özel yapılandırma sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="827a7-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="827a7-201">Örneğin, yapılandırmanın *appSettings. JSON* ve ortam değişkenlerinden geldiğini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="827a7-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="827a7-202">Ardından, *ConnectionString* değeri istendiğinde, Framework ilk olarak *appSettings. JSON* dosyasına bakar.</span><span class="sxs-lookup"><span data-stu-id="827a7-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="827a7-203">Değer aynı zamanda bir ortam değişkeninde bulunursa, ortam değişkeninin değeri öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="827a7-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="827a7-204">Parolalar gibi gizli yapılandırma verilerini yönetmek için ASP.NET Core bir [gizli dizi Yöneticisi aracı](xref:security/app-secrets)sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="827a7-205">Üretim gizli dizileri için [Azure Key Vault](xref:security/key-vault-configuration)önerilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="827a7-206">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="827a7-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="827a7-207">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="827a7-207">Options</span></span>

<span data-ttu-id="827a7-208">Mümkün olduğunda yapılandırma değerlerini depolamak ve almak için *Seçenekler deseninin* ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="827a7-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="827a7-209">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="827a7-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="827a7-210">Örneğin, aşağıdaki kod WebSockets seçeneklerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="827a7-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="827a7-211">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="827a7-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="827a7-212">Lý</span><span class="sxs-lookup"><span data-stu-id="827a7-212">Environments</span></span>

<span data-ttu-id="827a7-213">*Geliştirme*, *hazırlık*ve *üretim*gibi yürütme ortamları, ASP.NET Core birinci sınıf kavramlardır.</span><span class="sxs-lookup"><span data-stu-id="827a7-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="827a7-214">@No__t-0 ortam değişkenini ayarlayarak bir uygulamanın üzerinde çalıştığı ortamı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="827a7-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="827a7-215">ASP.NET Core, uygulamanın başlangıcında bu ortam değişkenini okur ve değeri bir `IHostingEnvironment` uygulamasında depolar.</span><span class="sxs-lookup"><span data-stu-id="827a7-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="827a7-216">Ortam nesnesi, uygulama tarafından her yerde DI aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="827a7-217">@No__t-0 sınıfından aşağıdaki örnek kod, uygulamayı yalnızca geliştirmede çalıştırıldığında ayrıntılı hata bilgilerini sunacak şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="827a7-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="827a7-218">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="827a7-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="827a7-219">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="827a7-219">Logging</span></span>

<span data-ttu-id="827a7-220">ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler.</span><span class="sxs-lookup"><span data-stu-id="827a7-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="827a7-221">Kullanılabilir sağlayıcılar şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="827a7-221">Available providers include the following:</span></span>

* <span data-ttu-id="827a7-222">Konsol</span><span class="sxs-lookup"><span data-stu-id="827a7-222">Console</span></span>
* <span data-ttu-id="827a7-223">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="827a7-223">Debug</span></span>
* <span data-ttu-id="827a7-224">Windows üzerinde olay Izleme</span><span class="sxs-lookup"><span data-stu-id="827a7-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="827a7-225">Windows olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="827a7-225">Windows Event Log</span></span>
* <span data-ttu-id="827a7-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="827a7-226">TraceSource</span></span>
* <span data-ttu-id="827a7-227">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="827a7-227">Azure App Service</span></span>
* <span data-ttu-id="827a7-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="827a7-228">Azure Application Insights</span></span>

<span data-ttu-id="827a7-229">Dı ve çağrı günlüğü yöntemlerinden `ILogger` nesnesi alarak uygulamanın kodundaki her yerden günlükleri yazın.</span><span class="sxs-lookup"><span data-stu-id="827a7-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="827a7-230">Oluşturucu Ekleme ve günlük yöntemi çağrılarını vurgulanmış bir `ILogger` nesnesi kullanan örnek kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="827a7-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="827a7-231">@No__t-0 arabirimi, günlük sağlayıcısına istediğiniz sayıda alanı geçirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="827a7-232">Alanlar genellikle bir ileti dizesi oluşturmak için kullanılır, ancak sağlayıcı bunları bir veri deposuna ayrı alanlar olarak da gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="827a7-233">Bu özellik, günlük sağlayıcılarının [yapılandırılmış günlük olarak da bilinen anlam günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="827a7-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="827a7-234">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="827a7-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="827a7-235">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="827a7-235">Routing</span></span>

<span data-ttu-id="827a7-236">*Yol* , bir işleyiciye EŞLENMIŞ bir URL örüncidir.</span><span class="sxs-lookup"><span data-stu-id="827a7-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="827a7-237">İşleyici genellikle bir Razor sayfası, MVC denetleyicisindeki bir eylem yöntemi veya bir ara yazılım olur.</span><span class="sxs-lookup"><span data-stu-id="827a7-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="827a7-238">ASP.NET Core yönlendirme, uygulamanız tarafından kullanılan URL 'Ler üzerinde denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="827a7-239">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="827a7-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="827a7-240">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="827a7-240">Error handling</span></span>

<span data-ttu-id="827a7-241">ASP.NET Core, hataları işlemeye yönelik yerleşik özellikler içerir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="827a7-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="827a7-242">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="827a7-242">A developer exception page</span></span>
* <span data-ttu-id="827a7-243">Özel hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="827a7-243">Custom error pages</span></span>
* <span data-ttu-id="827a7-244">Statik durum kodu sayfaları</span><span class="sxs-lookup"><span data-stu-id="827a7-244">Static status code pages</span></span>
* <span data-ttu-id="827a7-245">Başlatma özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="827a7-245">Startup exception handling</span></span>

<span data-ttu-id="827a7-246">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="827a7-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="827a7-247">HTTP isteğinde bulunma</span><span class="sxs-lookup"><span data-stu-id="827a7-247">Make HTTP requests</span></span>

<span data-ttu-id="827a7-248">@No__t-0 ' a bir uygulama `HttpClient` örnek oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="827a7-249">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="827a7-249">The factory:</span></span>

* <span data-ttu-id="827a7-250">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="827a7-251">Örneğin, *GitHub istemcisi kayıtlı ve GitHub 'a* erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="827a7-252">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="827a7-253">, Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok temsilci seçme işleyicisinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="827a7-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="827a7-254">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="827a7-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="827a7-255">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="827a7-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="827a7-256">Geçici hata işleme için popüler bir üçüncü taraf kitaplığı olan *Polly*ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="827a7-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="827a7-257">@No__t-1 yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temeldeki `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="827a7-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="827a7-258">Fabrika tarafından oluşturulan istemciler aracılığıyla gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger` aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="827a7-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="827a7-259">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="827a7-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="827a7-260">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="827a7-260">Content root</span></span>

<span data-ttu-id="827a7-261">İçerik kökü, için temel yoldur:</span><span class="sxs-lookup"><span data-stu-id="827a7-261">The content root is the base path to the:</span></span>

* <span data-ttu-id="827a7-262">Uygulamayı barındıran yürütülebilir dosya ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="827a7-262">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="827a7-263">Uygulamayı oluşturan derlenmiş derlemeler ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="827a7-263">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="827a7-264">Uygulama tarafından kullanılan kod olmayan içerik dosyaları, örneğin:</span><span class="sxs-lookup"><span data-stu-id="827a7-264">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="827a7-265">Razor dosyaları ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="827a7-265">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="827a7-266">Yapılandırma dosyaları ( *. JSON*, *. xml*)</span><span class="sxs-lookup"><span data-stu-id="827a7-266">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="827a7-267">Veri dosyaları ( *. db*)</span><span class="sxs-lookup"><span data-stu-id="827a7-267">Data files (*.db*)</span></span>
* <span data-ttu-id="827a7-268">[Web kökü](#web-root), genellikle yayınlanan *Wwwroot* klasörü.</span><span class="sxs-lookup"><span data-stu-id="827a7-268">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="827a7-269">Geliştirme sırasında:</span><span class="sxs-lookup"><span data-stu-id="827a7-269">During development:</span></span>

* <span data-ttu-id="827a7-270">İçerik kökü, projenin kök dizinini varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="827a7-270">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="827a7-271">Projenin kök dizini şunu oluşturmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="827a7-271">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="827a7-272">Uygulamanın, projenin kök dizinindeki kod olmayan içerik dosyalarının yolu.</span><span class="sxs-lookup"><span data-stu-id="827a7-272">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="827a7-273">[Web kökü](#web-root), genellikle projenin kök dizinindeki *Wwwroot* klasörü.</span><span class="sxs-lookup"><span data-stu-id="827a7-273">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="827a7-274">[Konak oluşturulurken](#host)alternatif bir içerik kök yolu belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-274">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="827a7-275">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="827a7-275">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="827a7-276">[Konak oluşturulurken](#host)alternatif bir içerik kök yolu belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-276">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="827a7-277">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="827a7-277">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="827a7-278">Web kökü</span><span class="sxs-lookup"><span data-stu-id="827a7-278">Web root</span></span>

<span data-ttu-id="827a7-279">Web kökü, genel, kod olmayan statik kaynak dosyalarının temel yoludur, örneğin:</span><span class="sxs-lookup"><span data-stu-id="827a7-279">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="827a7-280">Stil sayfaları ( *. css*)</span><span class="sxs-lookup"><span data-stu-id="827a7-280">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="827a7-281">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="827a7-281">JavaScript (*.js*)</span></span>
* <span data-ttu-id="827a7-282">Görüntüler ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="827a7-282">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="827a7-283">Statik dosyalar yalnızca Web kök dizininden (ve alt dizinlerde) varsayılan olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="827a7-283">Static files are only served by default from the web root directory (and sub-directories).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="827a7-284">Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir Web kökü belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-284">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="827a7-285">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="827a7-285">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="827a7-286">Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir Web kökü belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="827a7-286">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="827a7-287">Daha fazla bilgi için bkz. [Web root](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="827a7-287">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

::: moniker-end

<span data-ttu-id="827a7-288">Razor ( *. cshtml*) dosyalarında, tilde işareti (`~/`) Web köküne işaret eder.</span><span class="sxs-lookup"><span data-stu-id="827a7-288">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="827a7-289">@No__t-0 ' dan başlayan bir yol, *sanal yol*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="827a7-289">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="827a7-290">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="827a7-290">For more information, see <xref:fundamentals/static-files>.</span></span>
