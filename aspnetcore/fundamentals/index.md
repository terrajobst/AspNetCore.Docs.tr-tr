---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamalar oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 7533242140c31a937f32cc9082d760103347ce25
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219187"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="7ddcb-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="7ddcb-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7ddcb-104">Bu makale, ASP.NET Core uygulamaları geliştirmeyi anlamak için önemli konulara genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7ddcb-105">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="7ddcb-105">The Startup class</span></span>

<span data-ttu-id="7ddcb-106">`Startup` sınıfı şu konumda:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="7ddcb-107">Uygulamanın gerektirdiği hizmetler yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="7ddcb-108">İstek işleme işlem hattı tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7ddcb-109">*Hizmetler* , uygulama tarafından kullanılan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7ddcb-110">Örneğin, bir günlük bileşeni bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-110">For example, a logging component is a service.</span></span> <span data-ttu-id="7ddcb-111">Hizmetleri yapılandırmak (veya *kaydettirmek*) için kod `Startup.ConfigureServices` yöntemine eklenir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7ddcb-112">İstek işleme işlem hattı, bir dizi *Ara yazılım* bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7ddcb-113">Örneğin, bir ara yazılım statik dosyalar için istekleri işleyebilir veya HTTP isteklerini HTTPS 'ye yeniden yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7ddcb-114">Her bir ara yazılım bir `HttpContext` zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır ya da isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7ddcb-115">İstek işleme işlem hattını yapılandırma kodu `Startup.Configure` yöntemine eklenir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7ddcb-116">Örnek bir `Startup` sınıfı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="7ddcb-117">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7ddcb-118">Bağımlılık ekleme (hizmetler)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-118">Dependency injection (services)</span></span>

<span data-ttu-id="7ddcb-119">ASP.NET Core, yapılandırılmış hizmetlerin bir uygulamanın sınıfları tarafından kullanılabilmesini sağlayan yerleşik bir bağımlılık ekleme (dı) çerçevesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7ddcb-120">Bir sınıftaki hizmetin bir örneğini almanın bir yolu, gerekli türde bir parametreye sahip bir Oluşturucu oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7ddcb-121">Parametresi hizmet türü veya arabirim olabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7ddcb-122">Dı sistemi, çalışma zamanında hizmeti sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7ddcb-123">İşte bir Entity Framework Core bağlam nesnesi almak için DI kullanan bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7ddcb-124">Vurgulanan çizgi bir Oluşturucu Ekleme örneğidir:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7ddcb-125">Dı yerleşik olarak kullanılıyorsa, isterseniz Control (IOC) kapsayıcısının bir üçüncü taraf Inversion öğesini eklemenize olanak sağlamak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7ddcb-126">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7ddcb-127">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="7ddcb-127">Middleware</span></span>

<span data-ttu-id="7ddcb-128">İstek işleme işlem hattı, bir dizi ara yazılım bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7ddcb-129">Her bileşen bir `HttpContext` zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır ya da isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7ddcb-130">Kurala göre, `Startup.Configure` yönteminde `Use...` uzantısı yöntemini çağırarak işlem hattına bir ara yazılım bileşeni eklenir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7ddcb-131">Örneğin, statik dosyaların işlenmesini etkinleştirmek için `UseStaticFiles`çağırın.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7ddcb-132">Aşağıdaki örnekteki vurgulanan kod, istek işleme işlem hattını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="7ddcb-133">ASP.NET Core, zengin bir yerleşik ara yazılım kümesi içerir ve özel ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7ddcb-134">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7ddcb-135">Host</span><span class="sxs-lookup"><span data-stu-id="7ddcb-135">Host</span></span>

<span data-ttu-id="7ddcb-136">ASP.NET Core bir uygulama, başlangıçta bir *konak* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7ddcb-137">Ana bilgisayar, uygulamanın tüm kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7ddcb-138">Bir HTTP sunucusu uygulama</span><span class="sxs-lookup"><span data-stu-id="7ddcb-138">An HTTP server implementation</span></span>
* <span data-ttu-id="7ddcb-139">Ara yazılım bileşenleri</span><span class="sxs-lookup"><span data-stu-id="7ddcb-139">Middleware components</span></span>
* <span data-ttu-id="7ddcb-140">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-140">Logging</span></span>
* <span data-ttu-id="7ddcb-141">IÇERIK</span><span class="sxs-lookup"><span data-stu-id="7ddcb-141">DI</span></span>
* <span data-ttu-id="7ddcb-142">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7ddcb-142">Configuration</span></span>

<span data-ttu-id="7ddcb-143">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7ddcb-144">İki konak mevcuttur: genel ana bilgisayar ve Web ana bilgisayarı.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="7ddcb-145">Genel ana bilgisayar önerilir ve Web konağı yalnızca geriye dönük uyumluluk için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="7ddcb-146">Bir konak oluşturma kodu `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="7ddcb-147">`CreateDefaultBuilder` ve `ConfigureWebHostDefaults` yöntemleri, aşağıdaki gibi yaygın olarak kullanılan seçeneklerle bir konak yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7ddcb-148">Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7ddcb-149">*AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7ddcb-150">Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7ddcb-151">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7ddcb-152">Web dışı senaryolar</span><span class="sxs-lookup"><span data-stu-id="7ddcb-152">Non-web scenarios</span></span>

<span data-ttu-id="7ddcb-153">Genel ana bilgisayar, diğer uygulama türlerinin günlüğe kaydetme, bağımlılık ekleme (dı), yapılandırma ve uygulama ömür yönetimi gibi çapraz kesme çerçevesi uzantıları kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-153">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7ddcb-154">Daha fazla bilgi için <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/hosted-services> bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-154">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7ddcb-155">Sunucular</span><span class="sxs-lookup"><span data-stu-id="7ddcb-155">Servers</span></span>

<span data-ttu-id="7ddcb-156">Bir ASP.NET Core uygulaması HTTP isteklerini dinlemek için HTTP sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-156">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7ddcb-157">Sunucu, bir `HttpContext`oluşan [istek özellikleri](xref:fundamentals/request-features) kümesi olarak uygulamaya istekleri ister.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-157">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="7ddcb-158">Windows</span><span class="sxs-lookup"><span data-stu-id="7ddcb-158">Windows</span></span>](#tab/windows)

<span data-ttu-id="7ddcb-159">ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-159">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7ddcb-160">*Kestrel* , platformlar arası bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-160">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7ddcb-161">Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-161">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7ddcb-162">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-162">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7ddcb-163">*IIS HTTP sunucusu* , IIS kullanan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-163">*IIS HTTP Server* is a server for Windows that uses IIS.</span></span> <span data-ttu-id="7ddcb-164">Bu sunucu ile, ASP.NET Core uygulaması ve IIS aynı işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-164">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7ddcb-165">*Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-165">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7ddcb-166">macOS</span><span class="sxs-lookup"><span data-stu-id="7ddcb-166">macOS</span></span>](#tab/macos)

<span data-ttu-id="7ddcb-167">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-167">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ddcb-168">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-168">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ddcb-169">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-169">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7ddcb-170">Linux</span><span class="sxs-lookup"><span data-stu-id="7ddcb-170">Linux</span></span>](#tab/linux)

<span data-ttu-id="7ddcb-171">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-171">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ddcb-172">ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-172">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ddcb-173">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-173">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="7ddcb-174">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-174">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7ddcb-175">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7ddcb-175">Configuration</span></span>

<span data-ttu-id="7ddcb-176">ASP.NET Core, ayarları sıralı bir yapılandırma sağlayıcıları kümesinden ad-değer çiftleri olarak alan bir yapılandırma çerçevesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-176">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7ddcb-177">*. JSON* dosyaları, *. xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri gibi çeşitli kaynaklar için yerleşik yapılandırma sağlayıcıları vardır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-177">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7ddcb-178">Ayrıca, özel yapılandırma sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-178">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7ddcb-179">Örneğin, yapılandırmanın *appSettings. JSON* ve ortam değişkenlerinden geldiğini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-179">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7ddcb-180">Ardından, *ConnectionString* değeri istendiğinde, Framework ilk olarak *appSettings. JSON* dosyasına bakar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-180">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7ddcb-181">Değer aynı zamanda bir ortam değişkeninde bulunursa, ortam değişkeninin değeri öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-181">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7ddcb-182">Parolalar gibi gizli yapılandırma verilerini yönetmek için ASP.NET Core bir [gizli dizi Yöneticisi aracı](xref:security/app-secrets)sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-182">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7ddcb-183">Üretim gizli dizileri için [Azure Key Vault](xref:security/key-vault-configuration)önerilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-183">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7ddcb-184">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7ddcb-185">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="7ddcb-185">Options</span></span>

<span data-ttu-id="7ddcb-186">Mümkün olduğunda yapılandırma değerlerini depolamak ve almak için *Seçenekler deseninin* ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-186">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7ddcb-187">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-187">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7ddcb-188">Örneğin, aşağıdaki kod WebSockets seçeneklerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-188">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="7ddcb-189">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-189">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7ddcb-190">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="7ddcb-190">Environments</span></span>

<span data-ttu-id="7ddcb-191">*Geliştirme*, *hazırlık*ve *üretim*gibi yürütme ortamları, ASP.NET Core birinci sınıf kavramlardır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-191">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7ddcb-192">`ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlayarak bir uygulamanın üzerinde çalıştığı ortamı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-192">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7ddcb-193">ASP.NET Core, uygulamanın başlangıcında bu ortam değişkenini okur ve değeri bir `IHostingEnvironment` uygulamasında depolar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-193">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7ddcb-194">Ortam nesnesi, uygulama tarafından her yerde DI aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-194">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7ddcb-195">`Startup` sınıfından aşağıdaki örnek kod, uygulamayı yalnızca geliştirmede çalıştırıldığında ayrıntılı hata bilgilerini sunacak şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-195">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="7ddcb-196">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-196">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7ddcb-197">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-197">Logging</span></span>

<span data-ttu-id="7ddcb-198">ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-198">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7ddcb-199">Kullanılabilir sağlayıcılar şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-199">Available providers include the following:</span></span>

* <span data-ttu-id="7ddcb-200">Konsol</span><span class="sxs-lookup"><span data-stu-id="7ddcb-200">Console</span></span>
* <span data-ttu-id="7ddcb-201">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="7ddcb-201">Debug</span></span>
* <span data-ttu-id="7ddcb-202">Windows üzerinde olay Izleme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-202">Event Tracing on Windows</span></span>
* <span data-ttu-id="7ddcb-203">Windows olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="7ddcb-203">Windows Event Log</span></span>
* <span data-ttu-id="7ddcb-204">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7ddcb-204">TraceSource</span></span>
* <span data-ttu-id="7ddcb-205">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7ddcb-205">Azure App Service</span></span>
* <span data-ttu-id="7ddcb-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7ddcb-206">Azure Application Insights</span></span>

<span data-ttu-id="7ddcb-207">Dı ve çağrı günlüğü yöntemlerinden `ILogger` nesne alarak uygulamanın kodundaki her yerden günlükleri yazın.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-207">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7ddcb-208">Oluşturucu Ekleme ve günlük yöntemi çağrılarını vurgulanmış bir `ILogger` nesnesi kullanan örnek kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-208">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7ddcb-209">`ILogger` arabirimi, günlük sağlayıcısına istediğiniz sayıda alanı geçirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-209">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7ddcb-210">Alanlar genellikle bir ileti dizesi oluşturmak için kullanılır, ancak sağlayıcı bunları bir veri deposuna ayrı alanlar olarak da gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-210">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7ddcb-211">Bu özellik, günlük sağlayıcılarının [yapılandırılmış günlük olarak da bilinen anlam günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-211">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7ddcb-212">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7ddcb-213">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-213">Routing</span></span>

<span data-ttu-id="7ddcb-214">*Yol* , bir işleyiciye EŞLENMIŞ bir URL örüncidir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-214">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7ddcb-215">İşleyici genellikle bir Razor sayfası, MVC denetleyicisindeki bir eylem yöntemi veya bir ara yazılım olur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-215">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7ddcb-216">ASP.NET Core yönlendirme, uygulamanız tarafından kullanılan URL 'Ler üzerinde denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-216">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7ddcb-217">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7ddcb-218">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-218">Error handling</span></span>

<span data-ttu-id="7ddcb-219">ASP.NET Core, hataları işlemeye yönelik yerleşik özellikler içerir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-219">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7ddcb-220">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="7ddcb-220">A developer exception page</span></span>
* <span data-ttu-id="7ddcb-221">Özel hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="7ddcb-221">Custom error pages</span></span>
* <span data-ttu-id="7ddcb-222">Statik durum kodu sayfaları</span><span class="sxs-lookup"><span data-stu-id="7ddcb-222">Static status code pages</span></span>
* <span data-ttu-id="7ddcb-223">Başlatma özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-223">Startup exception handling</span></span>

<span data-ttu-id="7ddcb-224">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-224">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7ddcb-225">HTTP isteğinde bulunma</span><span class="sxs-lookup"><span data-stu-id="7ddcb-225">Make HTTP requests</span></span>

<span data-ttu-id="7ddcb-226">`IHttpClientFactory` bir uygulama `HttpClient` örnekleri oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-226">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7ddcb-227">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-227">The factory:</span></span>

* <span data-ttu-id="7ddcb-228">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-228">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7ddcb-229">Örneğin, *GitHub istemcisi kayıtlı ve GitHub 'a* erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-229">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7ddcb-230">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-230">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7ddcb-231">, Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok temsilci seçme işleyicisinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-231">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7ddcb-232">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-232">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7ddcb-233">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-233">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7ddcb-234">Geçici hata işleme için popüler bir üçüncü taraf kitaplığı olan *Polly*ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-234">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7ddcb-235">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-235">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7ddcb-236">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-236">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7ddcb-237">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-237">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7ddcb-238">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="7ddcb-238">Content root</span></span>

<span data-ttu-id="7ddcb-239">İçerik kökü, için temel yoldur:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-239">The content root is the base path to the:</span></span>

* <span data-ttu-id="7ddcb-240">Uygulamayı barındıran yürütülebilir dosya ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="7ddcb-240">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7ddcb-241">Uygulamayı oluşturan derlenmiş derlemeler ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="7ddcb-241">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7ddcb-242">Uygulama tarafından kullanılan kod olmayan içerik dosyaları, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-242">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="7ddcb-243">Razor dosyaları ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-243">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7ddcb-244">Yapılandırma dosyaları ( *. JSON*, *. xml*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-244">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7ddcb-245">Veri dosyaları ( *. db*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-245">Data files (*.db*)</span></span>
* <span data-ttu-id="7ddcb-246">[Web kökü](#web-root), genellikle yayınlanan *Wwwroot* klasörü.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-246">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="7ddcb-247">Geliştirme sırasında:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-247">During development:</span></span>

* <span data-ttu-id="7ddcb-248">İçerik kökü, projenin kök dizinini varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-248">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="7ddcb-249">Projenin kök dizini şunu oluşturmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-249">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="7ddcb-250">Uygulamanın, projenin kök dizinindeki kod olmayan içerik dosyalarının yolu.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-250">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="7ddcb-251">[Web kökü](#web-root), genellikle projenin kök dizinindeki *Wwwroot* klasörü.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-251">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="7ddcb-252">[Konak oluşturulurken](#host)alternatif bir içerik kök yolu belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-252">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="7ddcb-253">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-253">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

## <a name="web-root"></a><span data-ttu-id="7ddcb-254">Web kökü</span><span class="sxs-lookup"><span data-stu-id="7ddcb-254">Web root</span></span>

<span data-ttu-id="7ddcb-255">Web kökü, genel, kod olmayan statik kaynak dosyalarının temel yoludur, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-255">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="7ddcb-256">Stil sayfaları ( *. css*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-256">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7ddcb-257">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-257">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7ddcb-258">Görüntüler ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-258">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7ddcb-259">Statik dosyalar yalnızca Web kök dizininden (ve alt dizinlerde) varsayılan olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-259">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="7ddcb-260">Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir Web kökü belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-260">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="7ddcb-261">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-261">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

<span data-ttu-id="7ddcb-262">Proje dosyasındaki [\<içerik > proje öğesi](/visualstudio/msbuild/common-msbuild-project-items#content) ile *Wwwroot* 'da dosya yayımlamayı önleyin.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-262">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7ddcb-263">Aşağıdaki örnek, *Wwwroot/yerel* dizin ve alt dizinlerde içerik yayımlamayı engeller:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-263">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7ddcb-264">Statik kimlik varlıklarının Web köküne yayımlanmasını engellemek için bkz. <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-264">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

<span data-ttu-id="7ddcb-265">Razor ( *. cshtml*) dosyalarında, tilde işareti (`~/`) Web köküne işaret eder.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-265">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7ddcb-266">`~/` başlayan bir yol, *sanal yol*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-266">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7ddcb-267">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-267">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7ddcb-268">Bu makale, ASP.NET Core uygulamaları geliştirmeyi anlamak için önemli konulara genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-268">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7ddcb-269">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="7ddcb-269">The Startup class</span></span>

<span data-ttu-id="7ddcb-270">`Startup` sınıfı şu konumda:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-270">The `Startup` class is where:</span></span>

* <span data-ttu-id="7ddcb-271">Uygulamanın gerektirdiği hizmetler yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-271">Services required by the app are configured.</span></span>
* <span data-ttu-id="7ddcb-272">İstek işleme işlem hattı tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-272">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7ddcb-273">*Hizmetler* , uygulama tarafından kullanılan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-273">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7ddcb-274">Örneğin, bir günlük bileşeni bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-274">For example, a logging component is a service.</span></span> <span data-ttu-id="7ddcb-275">Hizmetleri yapılandırmak (veya *kaydettirmek*) için kod `Startup.ConfigureServices` yöntemine eklenir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-275">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7ddcb-276">İstek işleme işlem hattı, bir dizi *Ara yazılım* bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-276">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7ddcb-277">Örneğin, bir ara yazılım statik dosyalar için istekleri işleyebilir veya HTTP isteklerini HTTPS 'ye yeniden yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-277">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7ddcb-278">Her bir ara yazılım bir `HttpContext` zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır ya da isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-278">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7ddcb-279">İstek işleme işlem hattını yapılandırma kodu `Startup.Configure` yöntemine eklenir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-279">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7ddcb-280">Örnek bir `Startup` sınıfı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-280">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="7ddcb-281">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7ddcb-282">Bağımlılık ekleme (hizmetler)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-282">Dependency injection (services)</span></span>

<span data-ttu-id="7ddcb-283">ASP.NET Core, yapılandırılmış hizmetlerin bir uygulamanın sınıfları tarafından kullanılabilmesini sağlayan yerleşik bir bağımlılık ekleme (dı) çerçevesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-283">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7ddcb-284">Bir sınıftaki hizmetin bir örneğini almanın bir yolu, gerekli türde bir parametreye sahip bir Oluşturucu oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-284">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7ddcb-285">Parametresi hizmet türü veya arabirim olabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-285">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7ddcb-286">Dı sistemi, çalışma zamanında hizmeti sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-286">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7ddcb-287">İşte bir Entity Framework Core bağlam nesnesi almak için DI kullanan bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-287">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7ddcb-288">Vurgulanan çizgi bir Oluşturucu Ekleme örneğidir:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-288">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7ddcb-289">Dı yerleşik olarak kullanılıyorsa, isterseniz Control (IOC) kapsayıcısının bir üçüncü taraf Inversion öğesini eklemenize olanak sağlamak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-289">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7ddcb-290">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-290">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7ddcb-291">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="7ddcb-291">Middleware</span></span>

<span data-ttu-id="7ddcb-292">İstek işleme işlem hattı, bir dizi ara yazılım bileşeni olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-292">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7ddcb-293">Her bileşen bir `HttpContext` zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır ya da isteği sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-293">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7ddcb-294">Kurala göre, `Startup.Configure` yönteminde `Use...` uzantısı yöntemini çağırarak işlem hattına bir ara yazılım bileşeni eklenir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-294">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7ddcb-295">Örneğin, statik dosyaların işlenmesini etkinleştirmek için `UseStaticFiles`çağırın.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-295">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7ddcb-296">Aşağıdaki örnekteki vurgulanan kod, istek işleme işlem hattını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-296">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="7ddcb-297">ASP.NET Core, zengin bir yerleşik ara yazılım kümesi içerir ve özel ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-297">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7ddcb-298">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-298">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7ddcb-299">Host</span><span class="sxs-lookup"><span data-stu-id="7ddcb-299">Host</span></span>

<span data-ttu-id="7ddcb-300">ASP.NET Core bir uygulama, başlangıçta bir *konak* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-300">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7ddcb-301">Ana bilgisayar, uygulamanın tüm kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-301">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7ddcb-302">Bir HTTP sunucusu uygulama</span><span class="sxs-lookup"><span data-stu-id="7ddcb-302">An HTTP server implementation</span></span>
* <span data-ttu-id="7ddcb-303">Ara yazılım bileşenleri</span><span class="sxs-lookup"><span data-stu-id="7ddcb-303">Middleware components</span></span>
* <span data-ttu-id="7ddcb-304">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-304">Logging</span></span>
* <span data-ttu-id="7ddcb-305">IÇERIK</span><span class="sxs-lookup"><span data-stu-id="7ddcb-305">DI</span></span>
* <span data-ttu-id="7ddcb-306">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7ddcb-306">Configuration</span></span>

<span data-ttu-id="7ddcb-307">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-307">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7ddcb-308">İki konak mevcuttur: Web Konağı ve genel ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-308">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="7ddcb-309">ASP.NET Core 2. x içinde, genel konak yalnızca Web dışı senaryolar içindir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-309">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="7ddcb-310">Bir konak oluşturma kodu `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-310">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="7ddcb-311">`CreateDefaultBuilder` yöntemi, aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir konak yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-311">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7ddcb-312">Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-312">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7ddcb-313">*AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-313">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7ddcb-314">Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-314">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7ddcb-315">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-315">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7ddcb-316">Web dışı senaryolar</span><span class="sxs-lookup"><span data-stu-id="7ddcb-316">Non-web scenarios</span></span>

<span data-ttu-id="7ddcb-317">Genel ana bilgisayar, diğer uygulama türlerinin günlüğe kaydetme, bağımlılık ekleme (dı), yapılandırma ve uygulama ömür yönetimi gibi çapraz kesme çerçevesi uzantıları kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-317">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7ddcb-318">Daha fazla bilgi için <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/hosted-services> bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-318">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7ddcb-319">Sunucular</span><span class="sxs-lookup"><span data-stu-id="7ddcb-319">Servers</span></span>

<span data-ttu-id="7ddcb-320">Bir ASP.NET Core uygulaması HTTP isteklerini dinlemek için HTTP sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-320">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7ddcb-321">Sunucu, bir `HttpContext`oluşan [istek özellikleri](xref:fundamentals/request-features) kümesi olarak uygulamaya istekleri ister.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-321">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7ddcb-322">Windows</span><span class="sxs-lookup"><span data-stu-id="7ddcb-322">Windows</span></span>](#tab/windows)

<span data-ttu-id="7ddcb-323">ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-323">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7ddcb-324">*Kestrel* , platformlar arası bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-324">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7ddcb-325">Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-325">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7ddcb-326">Kestrel, doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-326">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7ddcb-327">*IIS HTTP sunucusu* , IIS kullanan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-327">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="7ddcb-328">Bu sunucu ile, ASP.NET Core uygulaması ve IIS aynı işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-328">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7ddcb-329">*Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-329">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7ddcb-330">macOS</span><span class="sxs-lookup"><span data-stu-id="7ddcb-330">macOS</span></span>](#tab/macos)

<span data-ttu-id="7ddcb-331">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-331">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ddcb-332">Kestrel, doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-332">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ddcb-333">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-333">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7ddcb-334">Linux</span><span class="sxs-lookup"><span data-stu-id="7ddcb-334">Linux</span></span>](#tab/linux)

<span data-ttu-id="7ddcb-335">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-335">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ddcb-336">Kestrel, doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-336">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ddcb-337">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-337">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7ddcb-338">Windows</span><span class="sxs-lookup"><span data-stu-id="7ddcb-338">Windows</span></span>](#tab/windows)

<span data-ttu-id="7ddcb-339">ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-339">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7ddcb-340">*Kestrel* , platformlar arası bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-340">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7ddcb-341">Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-341">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7ddcb-342">Kestrel, doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-342">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7ddcb-343">*Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-343">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7ddcb-344">macOS</span><span class="sxs-lookup"><span data-stu-id="7ddcb-344">macOS</span></span>](#tab/macos)

<span data-ttu-id="7ddcb-345">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-345">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ddcb-346">Kestrel, doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-346">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ddcb-347">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-347">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7ddcb-348">Linux</span><span class="sxs-lookup"><span data-stu-id="7ddcb-348">Linux</span></span>](#tab/linux)

<span data-ttu-id="7ddcb-349">ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-349">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ddcb-350">Kestrel, doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-350">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ddcb-351">Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-351">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7ddcb-352">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-352">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7ddcb-353">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7ddcb-353">Configuration</span></span>

<span data-ttu-id="7ddcb-354">ASP.NET Core, ayarları sıralı bir yapılandırma sağlayıcıları kümesinden ad-değer çiftleri olarak alan bir yapılandırma çerçevesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-354">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7ddcb-355">*. JSON* dosyaları, *. xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri gibi çeşitli kaynaklar için yerleşik yapılandırma sağlayıcıları vardır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-355">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7ddcb-356">Ayrıca, özel yapılandırma sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-356">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7ddcb-357">Örneğin, yapılandırmanın *appSettings. JSON* ve ortam değişkenlerinden geldiğini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-357">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7ddcb-358">Ardından, *ConnectionString* değeri istendiğinde, Framework ilk olarak *appSettings. JSON* dosyasına bakar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-358">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7ddcb-359">Değer aynı zamanda bir ortam değişkeninde bulunursa, ortam değişkeninin değeri öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-359">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7ddcb-360">Parolalar gibi gizli yapılandırma verilerini yönetmek için ASP.NET Core bir [gizli dizi Yöneticisi aracı](xref:security/app-secrets)sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-360">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7ddcb-361">Üretim gizli dizileri için [Azure Key Vault](xref:security/key-vault-configuration)önerilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-361">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7ddcb-362">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-362">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7ddcb-363">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="7ddcb-363">Options</span></span>

<span data-ttu-id="7ddcb-364">Mümkün olduğunda yapılandırma değerlerini depolamak ve almak için *Seçenekler deseninin* ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-364">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7ddcb-365">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-365">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7ddcb-366">Örneğin, aşağıdaki kod WebSockets seçeneklerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-366">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="7ddcb-367">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-367">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7ddcb-368">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="7ddcb-368">Environments</span></span>

<span data-ttu-id="7ddcb-369">*Geliştirme*, *hazırlık*ve *üretim*gibi yürütme ortamları, ASP.NET Core birinci sınıf kavramlardır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-369">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7ddcb-370">`ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlayarak bir uygulamanın üzerinde çalıştığı ortamı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-370">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7ddcb-371">ASP.NET Core, uygulamanın başlangıcında bu ortam değişkenini okur ve değeri bir `IHostingEnvironment` uygulamasında depolar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-371">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7ddcb-372">Ortam nesnesi, uygulama tarafından her yerde DI aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-372">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7ddcb-373">`Startup` sınıfından aşağıdaki örnek kod, uygulamayı yalnızca geliştirmede çalıştırıldığında ayrıntılı hata bilgilerini sunacak şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-373">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="7ddcb-374">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-374">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7ddcb-375">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-375">Logging</span></span>

<span data-ttu-id="7ddcb-376">ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-376">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7ddcb-377">Kullanılabilir sağlayıcılar şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-377">Available providers include the following:</span></span>

* <span data-ttu-id="7ddcb-378">Konsol</span><span class="sxs-lookup"><span data-stu-id="7ddcb-378">Console</span></span>
* <span data-ttu-id="7ddcb-379">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="7ddcb-379">Debug</span></span>
* <span data-ttu-id="7ddcb-380">Windows üzerinde olay Izleme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-380">Event Tracing on Windows</span></span>
* <span data-ttu-id="7ddcb-381">Windows olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="7ddcb-381">Windows Event Log</span></span>
* <span data-ttu-id="7ddcb-382">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7ddcb-382">TraceSource</span></span>
* <span data-ttu-id="7ddcb-383">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7ddcb-383">Azure App Service</span></span>
* <span data-ttu-id="7ddcb-384">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7ddcb-384">Azure Application Insights</span></span>

<span data-ttu-id="7ddcb-385">Dı ve çağrı günlüğü yöntemlerinden `ILogger` nesne alarak uygulamanın kodundaki her yerden günlükleri yazın.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-385">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7ddcb-386">Oluşturucu Ekleme ve günlük yöntemi çağrılarını vurgulanmış bir `ILogger` nesnesi kullanan örnek kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-386">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7ddcb-387">`ILogger` arabirimi, günlük sağlayıcısına istediğiniz sayıda alanı geçirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-387">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7ddcb-388">Alanlar genellikle bir ileti dizesi oluşturmak için kullanılır, ancak sağlayıcı bunları bir veri deposuna ayrı alanlar olarak da gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-388">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7ddcb-389">Bu özellik, günlük sağlayıcılarının [yapılandırılmış günlük olarak da bilinen anlam günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-389">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7ddcb-390">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-390">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7ddcb-391">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-391">Routing</span></span>

<span data-ttu-id="7ddcb-392">*Yol* , bir işleyiciye EŞLENMIŞ bir URL örüncidir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-392">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7ddcb-393">İşleyici genellikle bir Razor sayfası, MVC denetleyicisindeki bir eylem yöntemi veya bir ara yazılım olur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-393">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7ddcb-394">ASP.NET Core yönlendirme, uygulamanız tarafından kullanılan URL 'Ler üzerinde denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-394">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7ddcb-395">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-395">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7ddcb-396">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-396">Error handling</span></span>

<span data-ttu-id="7ddcb-397">ASP.NET Core, hataları işlemeye yönelik yerleşik özellikler içerir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-397">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7ddcb-398">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="7ddcb-398">A developer exception page</span></span>
* <span data-ttu-id="7ddcb-399">Özel hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="7ddcb-399">Custom error pages</span></span>
* <span data-ttu-id="7ddcb-400">Statik durum kodu sayfaları</span><span class="sxs-lookup"><span data-stu-id="7ddcb-400">Static status code pages</span></span>
* <span data-ttu-id="7ddcb-401">Başlatma özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="7ddcb-401">Startup exception handling</span></span>

<span data-ttu-id="7ddcb-402">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-402">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7ddcb-403">HTTP isteğinde bulunma</span><span class="sxs-lookup"><span data-stu-id="7ddcb-403">Make HTTP requests</span></span>

<span data-ttu-id="7ddcb-404">`IHttpClientFactory` bir uygulama `HttpClient` örnekleri oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-404">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7ddcb-405">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-405">The factory:</span></span>

* <span data-ttu-id="7ddcb-406">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-406">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7ddcb-407">Örneğin, *GitHub istemcisi kayıtlı ve GitHub 'a* erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-407">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7ddcb-408">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-408">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7ddcb-409">, Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok temsilci seçme işleyicisinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-409">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7ddcb-410">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-410">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7ddcb-411">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-411">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7ddcb-412">Geçici hata işleme için popüler bir üçüncü taraf kitaplığı olan *Polly*ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-412">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7ddcb-413">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-413">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7ddcb-414">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-414">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7ddcb-415">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-415">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7ddcb-416">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="7ddcb-416">Content root</span></span>

<span data-ttu-id="7ddcb-417">İçerik kökü, için temel yoldur:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-417">The content root is the base path to the:</span></span>

* <span data-ttu-id="7ddcb-418">Uygulamayı barındıran yürütülebilir dosya ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="7ddcb-418">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7ddcb-419">Uygulamayı oluşturan derlenmiş derlemeler ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="7ddcb-419">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7ddcb-420">Uygulama tarafından kullanılan kod olmayan içerik dosyaları, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-420">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="7ddcb-421">Razor dosyaları ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-421">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7ddcb-422">Yapılandırma dosyaları ( *. JSON*, *. xml*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-422">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7ddcb-423">Veri dosyaları ( *. db*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-423">Data files (*.db*)</span></span>
* <span data-ttu-id="7ddcb-424">[Web kökü](#web-root), genellikle yayınlanan *Wwwroot* klasörü.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-424">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="7ddcb-425">Geliştirme sırasında:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-425">During development:</span></span>

* <span data-ttu-id="7ddcb-426">İçerik kökü, projenin kök dizinini varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-426">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="7ddcb-427">Projenin kök dizini şunu oluşturmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-427">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="7ddcb-428">Uygulamanın, projenin kök dizinindeki kod olmayan içerik dosyalarının yolu.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-428">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="7ddcb-429">[Web kökü](#web-root), genellikle projenin kök dizinindeki *Wwwroot* klasörü.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-429">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="7ddcb-430">[Konak oluşturulurken](#host)alternatif bir içerik kök yolu belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-430">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="7ddcb-431">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-431">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="7ddcb-432">Web kökü</span><span class="sxs-lookup"><span data-stu-id="7ddcb-432">Web root</span></span>

<span data-ttu-id="7ddcb-433">Web kökü, genel, kod olmayan statik kaynak dosyalarının temel yoludur, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-433">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="7ddcb-434">Stil sayfaları ( *. css*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-434">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7ddcb-435">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-435">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7ddcb-436">Görüntüler ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="7ddcb-436">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7ddcb-437">Statik dosyalar yalnızca Web kök dizininden (ve alt dizinlerde) varsayılan olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-437">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="7ddcb-438">Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir Web kökü belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-438">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="7ddcb-439">Daha fazla bilgi için bkz. [Web root](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="7ddcb-439">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="7ddcb-440">Proje dosyasındaki [\<içerik > proje öğesi](/visualstudio/msbuild/common-msbuild-project-items#content) ile *Wwwroot* 'da dosya yayımlamayı önleyin.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-440">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7ddcb-441">Aşağıdaki örnek, *Wwwroot/yerel* dizin ve alt dizinlerde içerik yayımlamayı engeller:</span><span class="sxs-lookup"><span data-stu-id="7ddcb-441">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7ddcb-442">Razor ( *. cshtml*) dosyalarında, tilde işareti (`~/`) Web köküne işaret eder.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-442">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7ddcb-443">`~/` başlayan bir yol, *sanal yol*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-443">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7ddcb-444">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7ddcb-444">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end
