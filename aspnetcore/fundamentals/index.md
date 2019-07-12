---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: a6c848987c97103864fd5410922346e85a68c353
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856229"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="a067a-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="a067a-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="a067a-104">Bu makalede, ASP.NET Core uygulamaları geliştirmek nasıl anlamaya yönelik önemli konular bir genel bakıştır.</span><span class="sxs-lookup"><span data-stu-id="a067a-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="a067a-105">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="a067a-105">The Startup class</span></span>

<span data-ttu-id="a067a-106">`Startup` Sınıftır yeri:</span><span class="sxs-lookup"><span data-stu-id="a067a-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="a067a-107">Uygulama tarafından gereken hizmetleri yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a067a-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="a067a-108">Ardışık Düzen işleme isteği tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="a067a-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="a067a-109">*Hizmetleri* uygulama tarafından kullanılan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="a067a-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="a067a-110">Örneğin, bir günlük bileşeni bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="a067a-110">For example, a logging component is a service.</span></span> <span data-ttu-id="a067a-111">Kod yapılandırmak için (veya *kaydetme*) Hizmetleri eklenir `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a067a-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="a067a-112">Ardışık Düzen işleme isteği oluşan bir dizi olarak *ara yazılım* bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="a067a-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="a067a-113">Örneğin, bir ara yazılım bir statik dosyaları için istekleri işleyecek veya HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="a067a-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="a067a-114">Zaman uyumsuz işlemler gerçekleştirir her bir ara yazılım bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a067a-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="a067a-115">Ardışık Düzen işleme isteği yapılandırmak için kod eklenir `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a067a-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="a067a-116">Bir örneği aşağıdadır `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a067a-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="a067a-117">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a067a-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="a067a-118">Bağımlılık ekleme (hizmetler)</span><span class="sxs-lookup"><span data-stu-id="a067a-118">Dependency injection (services)</span></span>

<span data-ttu-id="a067a-119">ASP.NET Core yerleşik bağımlılık ekleme (dı) framework uygulamanın sınıfları için kullanılabilir yapar yapılandırılmış hizmet vardır.</span><span class="sxs-lookup"><span data-stu-id="a067a-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="a067a-120">Bir sınıf içinde bir hizmet örneği yapmanın bir yolu, gerekli türünde bir parametre ile bir oluşturucu oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="a067a-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="a067a-121">Parametresi, hizmet türü veya arabirim olabilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="a067a-122">DI sistemi hizmeti zamanında sağlar.</span><span class="sxs-lookup"><span data-stu-id="a067a-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="a067a-123">Bir Entity Framework Core bağlam nesnesini almak için DI kullanan bir sınıf aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a067a-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="a067a-124">Vurgulanan satırı Oluşturucu ekleme örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a067a-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="a067a-125">DI oluşturulmuş olsa da tercih ederseniz üçüncü taraf bir denetimi tersine çevirme (IOC) kapsayıcı takın izin verecek şekilde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a067a-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="a067a-126">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="a067a-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="a067a-127">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="a067a-127">Middleware</span></span>

<span data-ttu-id="a067a-128">Ardışık Düzen işleme isteği ara yazılımı bileşenleri bir dizi olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="a067a-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="a067a-129">Zaman uyumsuz işlemler gerçekleştirir her bileşen bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a067a-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="a067a-130">Kural gereği, harekete geçirerek bir ara yazılım bileşeni ardışık düzenine eklenen kendi `Use...` uzantı yönteminde `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a067a-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="a067a-131">Örneğin, statik dosyaları işleme olanağı çağrı `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="a067a-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="a067a-132">Aşağıdaki örnekte vurgulanmış kodu ardışık düzen işleme isteği yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="a067a-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="a067a-133">ASP.NET Core içeren zengin bir yerleşik ara yazılım ve özel bir ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a067a-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="a067a-134">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="a067a-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="a067a-135">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="a067a-135">Host</span></span>

<span data-ttu-id="a067a-136">ASP.NET Core uygulaması derleme bir *konak* başlangıç.</span><span class="sxs-lookup"><span data-stu-id="a067a-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="a067a-137">Ana bilgisayar gibi tüm uygulamanın kaynakları yalıtan bir nesne şöyledir:</span><span class="sxs-lookup"><span data-stu-id="a067a-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="a067a-138">Bir HTTP sunucusu uygulamasını</span><span class="sxs-lookup"><span data-stu-id="a067a-138">An HTTP server implementation</span></span>
* <span data-ttu-id="a067a-139">Ara yazılım bileşenleri</span><span class="sxs-lookup"><span data-stu-id="a067a-139">Middleware components</span></span>
* <span data-ttu-id="a067a-140">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="a067a-140">Logging</span></span>
* <span data-ttu-id="a067a-141">DI</span><span class="sxs-lookup"><span data-stu-id="a067a-141">DI</span></span>
* <span data-ttu-id="a067a-142">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a067a-142">Configuration</span></span>

<span data-ttu-id="a067a-143">Tüm bağımlı kaynakları uygulamanın bir nesnesinde de dahil olmak üzere ana nedeni ömrü yönetimi,: uygulama başlatma ve normal şekilde kapatılmasını üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="a067a-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a067a-144">İki ana kullanılabilir: Genel yönetici ve Web ana bilgisayarı.</span><span class="sxs-lookup"><span data-stu-id="a067a-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="a067a-145">Genel konak önerilir ve Web ana bilgisayarı geriye yalnızca kullanılabilir uyumluluk.</span><span class="sxs-lookup"><span data-stu-id="a067a-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="a067a-146">Bir ana bilgisayar oluşturmak için kod `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a067a-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="a067a-147">`CreateDefaultBuilder` Ve `ConfigureWebHostDefaults` yöntemler aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir konak yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="a067a-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="a067a-148">Kullanım [Kestrel](#servers) web sunucusu ve etkin IIS tümleştirme olarak.</span><span class="sxs-lookup"><span data-stu-id="a067a-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="a067a-149">Yük yapılandırmasından *appsettings.json*, *appsettings. { Ortam adı} .json*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynaklarını.</span><span class="sxs-lookup"><span data-stu-id="a067a-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="a067a-150">Günlük çıktısı, konsol ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="a067a-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="a067a-151">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="a067a-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a067a-152">İki ana kullanılabilir: Web ana bilgisayarı ve genel ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="a067a-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="a067a-153">ASP.NET core'da 2.x genel konağın olduğundan yalnızca web olmayan senaryolar için.</span><span class="sxs-lookup"><span data-stu-id="a067a-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="a067a-154">Bir ana bilgisayar oluşturmak için kod `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a067a-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="a067a-155">`CreateDefaultBuilder` Yöntemi aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir konak yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="a067a-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="a067a-156">Kullanım [Kestrel](#servers) web sunucusu ve etkin IIS tümleştirme olarak.</span><span class="sxs-lookup"><span data-stu-id="a067a-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="a067a-157">Yük yapılandırmasından *appsettings.json*, *appsettings. { Ortam adı} .json*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynaklarını.</span><span class="sxs-lookup"><span data-stu-id="a067a-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="a067a-158">Günlük çıktısı, konsol ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="a067a-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="a067a-159">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="a067a-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="a067a-160">Olmayan web senaryoları</span><span class="sxs-lookup"><span data-stu-id="a067a-160">Non-web scenarios</span></span>

<span data-ttu-id="a067a-161">Genel konak günlüğe kaydetme, bağımlılık ekleme (dı), yapılandırma ve uygulama ömrü yönetimi gibi çapraz kesme framework uzantıları kullanmak için uygulama diğer türleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a067a-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="a067a-162">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="a067a-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="a067a-163">Sunucular</span><span class="sxs-lookup"><span data-stu-id="a067a-163">Servers</span></span>

<span data-ttu-id="a067a-164">ASP.NET Core uygulaması, HTTP isteklerini dinlemek için bir HTTP sunucusu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="a067a-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="a067a-165">Uygulamaya bir dizi olarak sunucu yüzeyleri istekleri [istek özellikleri](xref:fundamentals/request-features) içine oluşan bir `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a067a-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="a067a-166">Windows</span><span class="sxs-lookup"><span data-stu-id="a067a-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="a067a-167">ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="a067a-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="a067a-168">*Kestrel'i* bir platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="a067a-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="a067a-169">Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="a067a-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="a067a-170">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="a067a-171">*IIS HTTP sunucusu* IIS kullanan bir windows Server'de olduğu.</span><span class="sxs-lookup"><span data-stu-id="a067a-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="a067a-172">IIS ve ASP.NET Core uygulaması, bu sunucu ile aynı işlem içinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a067a-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="a067a-173">*HTTP.sys* , IIS ile kullanılmayan Windows için sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="a067a-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="a067a-174">macOS</span><span class="sxs-lookup"><span data-stu-id="a067a-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="a067a-175">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="a067a-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a067a-176">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a067a-177">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a067a-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="a067a-178">Linux</span><span class="sxs-lookup"><span data-stu-id="a067a-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="a067a-179">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="a067a-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a067a-180">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a067a-181">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a067a-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="a067a-182">Windows</span><span class="sxs-lookup"><span data-stu-id="a067a-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="a067a-183">ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="a067a-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="a067a-184">*Kestrel'i* bir platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="a067a-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="a067a-185">Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="a067a-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="a067a-186">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="a067a-187">*HTTP.sys* , IIS ile kullanılmayan Windows için sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="a067a-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="a067a-188">macOS</span><span class="sxs-lookup"><span data-stu-id="a067a-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="a067a-189">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="a067a-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a067a-190">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a067a-191">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a067a-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="a067a-192">Linux</span><span class="sxs-lookup"><span data-stu-id="a067a-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="a067a-193">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="a067a-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a067a-194">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a067a-195">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a067a-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="a067a-196">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="a067a-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="a067a-197">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a067a-197">Configuration</span></span>

<span data-ttu-id="a067a-198">ASP.NET Core ayarları ad-değer çiftleri olarak sıralı bir dizi yapılandırma sağlayıcıları alır bir yapılandırma çerçevesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a067a-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="a067a-199">Yerleşik yapılandırma sağlayıcıları vardır, kaynakları çeşitli gibi *.json* dosyaları *.xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a067a-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="a067a-200">Özel yapılandırma sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a067a-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="a067a-201">Örneğin, bu yapılandırma geldiği belirtebilirsiniz *appsettings.json* ve ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a067a-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="a067a-202">Sonra ne zaman değerini *ConnectionString* istenildiğinde, framework ilk olarak görünür *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a067a-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="a067a-203">Değer var. bulunursa, aynı zamanda bir ortam değişkeni, ortam değişkeninin değerini öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="a067a-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="a067a-204">ASP.NET Core sağlar parolalar gibi gizli yapılandırma verilerini yönetmek için bir [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a067a-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="a067a-205">Üretim gizli öğeleri için öneririz [Azure anahtar kasası](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="a067a-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="a067a-206">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a067a-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="a067a-207">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="a067a-207">Options</span></span>

<span data-ttu-id="a067a-208">Mümkünse, ASP.NET Core aşağıdaki *seçenekleri deseni* depolamak ve yapılandırma değerleri almak için.</span><span class="sxs-lookup"><span data-stu-id="a067a-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="a067a-209">Seçenekleri deseni sınıfları, ilgili ayar gruplarını temsil etmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="a067a-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="a067a-210">Örneğin, aşağıdaki kodu WebSockets seçeneklerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="a067a-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="a067a-211">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a067a-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="a067a-212">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="a067a-212">Environments</span></span>

<span data-ttu-id="a067a-213">Yürütme ortamlarını gibi *geliştirme*, *hazırlama*, ve *üretim*, ASP.NET Core, birinci sınıf bir kavram olan.</span><span class="sxs-lookup"><span data-stu-id="a067a-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="a067a-214">Bir uygulama ortamı çalışır ayarlayarak belirtebilirsiniz `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="a067a-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a067a-215">ASP.NET Core uygulaması başlatma sırasında bu ortam değişkenini okur ve değerini depolayan bir `IHostingEnvironment` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="a067a-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="a067a-216">Ortam nesnesi herhangi bir uygulamayı DI aracılığıyla kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="a067a-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="a067a-217">Aşağıdaki örnek, koddan `Startup` sınıfı yalnızca geliştirme çalıştığında, ayrıntılı hata bilgileri sağlamak için uygulama yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="a067a-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="a067a-218">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a067a-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="a067a-219">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="a067a-219">Logging</span></span>

<span data-ttu-id="a067a-220">ASP.NET Core çeşitli günlük yerleşik ve üçüncü taraf sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler.</span><span class="sxs-lookup"><span data-stu-id="a067a-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="a067a-221">Kullanılabilir sağlayıcılar şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="a067a-221">Available providers include the following:</span></span>

* <span data-ttu-id="a067a-222">Konsol</span><span class="sxs-lookup"><span data-stu-id="a067a-222">Console</span></span>
* <span data-ttu-id="a067a-223">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a067a-223">Debug</span></span>
* <span data-ttu-id="a067a-224">Windows olay izleme</span><span class="sxs-lookup"><span data-stu-id="a067a-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="a067a-225">Windows olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="a067a-225">Windows Event Log</span></span>
* <span data-ttu-id="a067a-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="a067a-226">TraceSource</span></span>
* <span data-ttu-id="a067a-227">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="a067a-227">Azure App Service</span></span>
* <span data-ttu-id="a067a-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="a067a-228">Azure Application Insights</span></span>

<span data-ttu-id="a067a-229">Yazma günlükleri öğesinden herhangi bir uygulamanın kodu alarak bir `ILogger` DI ve günlük metotları çağırma nesnesi.</span><span class="sxs-lookup"><span data-stu-id="a067a-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="a067a-230">İşte kullanan örnek kodu bir `ILogger` Oluşturucu ekleme ve vurgulanmış günlük yöntem çağrılarını içeren bir nesne.</span><span class="sxs-lookup"><span data-stu-id="a067a-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="a067a-231">`ILogger` Arabirimi herhangi bir sayıda alanlar için oturum açma sağlayıcısı geçirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a067a-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="a067a-232">Alanları bir ileti dizesi oluşturmak için yaygın olarak kullanılır, ancak sağlayıcısı Ayrıca bunları ayrı alanlar olarak bir veri deposuna gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="a067a-233">Bu özelliği uygulamak günlük sağlayıcıları için mümkün kılar [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="a067a-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="a067a-234">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="a067a-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="a067a-235">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a067a-235">Routing</span></span>

<span data-ttu-id="a067a-236">A *rota* bir işleyici eşlenmiş bir URL deseni şudur.</span><span class="sxs-lookup"><span data-stu-id="a067a-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="a067a-237">İşleyici, bir Razor sayfası, bir MVC denetleyicisi veya bir ara yazılım bir eylem yöntemi genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="a067a-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="a067a-238">Uygulamanız tarafından kullanılan URL'leri üzerinde denetim ASP.NET Core yönlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="a067a-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="a067a-239">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="a067a-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="a067a-240">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="a067a-240">Error handling</span></span>

<span data-ttu-id="a067a-241">ASP.NET Core gibi hataları işlemek için yerleşik özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a067a-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="a067a-242">Bir geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="a067a-242">A developer exception page</span></span>
* <span data-ttu-id="a067a-243">Özel hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="a067a-243">Custom error pages</span></span>
* <span data-ttu-id="a067a-244">Statik durumu kod sayfaları</span><span class="sxs-lookup"><span data-stu-id="a067a-244">Static status code pages</span></span>
* <span data-ttu-id="a067a-245">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="a067a-245">Startup exception handling</span></span>

<span data-ttu-id="a067a-246">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="a067a-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="a067a-247">HTTP isteğinde bulunma</span><span class="sxs-lookup"><span data-stu-id="a067a-247">Make HTTP requests</span></span>

<span data-ttu-id="a067a-248">Uygulanışı `IHttpClientFactory` oluşturmak için kullanılabilir `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="a067a-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="a067a-249">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="a067a-249">The factory:</span></span>

* <span data-ttu-id="a067a-250">Adlandırma ve mantıksal yapılandırmak için merkezi bir konum sağlar `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="a067a-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="a067a-251">Örneğin, bir *github* istemci kayıtlı ve GitHub erişim sağlamak için yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="a067a-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="a067a-252">Varsayılan istemci diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="a067a-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="a067a-253">Kayıt ve giden bir istek ara yazılım ardışık düzenini oluşturmak için birden fazla temsilci işleyicileri zincirleme destekler.</span><span class="sxs-lookup"><span data-stu-id="a067a-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="a067a-254">Bu düzen, ASP.NET Core gelen ara yazılım ardışık benzerdir.</span><span class="sxs-lookup"><span data-stu-id="a067a-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="a067a-255">Desen etrafında HTTP isteklerini, önbelleğe alma, hata, seri hale getirme, işleme ve günlüğe kaydetme gibi çapraz kesme konuları yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="a067a-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="a067a-256">Tümleşik şekilde çalışarak *Polly*, geçici hata işlemeye yönelik popüler bir üçüncü taraf kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="a067a-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="a067a-257">Havuzu ve arka plandaki, yaşam süresini yöneten `HttpClientMessageHandler` el ile yönetilmesi sırasında oluşan Genel DNS sorunları önlemek için örnekleri `HttpClient` yaşam süresi yok.</span><span class="sxs-lookup"><span data-stu-id="a067a-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="a067a-258">Yapılandırılabilir günlük deneyimi ekler (aracılığıyla `ILogger`) fabrikası tarafından oluşturulan istemcileri aracılığıyla gönderilen tüm istekler için.</span><span class="sxs-lookup"><span data-stu-id="a067a-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="a067a-259">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="a067a-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="a067a-260">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="a067a-260">Content root</span></span>

<span data-ttu-id="a067a-261">Temel yol, Razor dosyaları gibi uygulama tarafından kullanılan herhangi bir özel içerik için içerik köküdür.</span><span class="sxs-lookup"><span data-stu-id="a067a-261">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="a067a-262">Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosya için taban yoludur.</span><span class="sxs-lookup"><span data-stu-id="a067a-262">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="a067a-263">Alternatif bir konuma olabilir belirtilen [konak oluşturmaya](#host).</span><span class="sxs-lookup"><span data-stu-id="a067a-263">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a067a-264">Daha fazla bilgi için [içerik kök](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="a067a-264">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a067a-265">Daha fazla bilgi için [içerik kök](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="a067a-265">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="a067a-266">Web kökü</span><span class="sxs-lookup"><span data-stu-id="a067a-266">Web root</span></span>

<span data-ttu-id="a067a-267">Web kökü (diğer adıyla *webroot*) genel, statik kaynakları, CSS, JavaScript ve görüntü dosyaları gibi temel yolu.</span><span class="sxs-lookup"><span data-stu-id="a067a-267">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="a067a-268">Statik dosya ara yazılımı, varsayılan olarak yalnızca bir hizmet web kök dizinine (ve alt dizinleri) dosyalarından verecektir.</span><span class="sxs-lookup"><span data-stu-id="a067a-268">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="a067a-269">Varsayılan olarak web kök yolu *{içerik kök} / wwwroot*, ancak farklı bir konuma A'da belirtildiği zaman [konak oluşturmaya](#host).</span><span class="sxs-lookup"><span data-stu-id="a067a-269">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="a067a-270">Razor'daki ( *.cshtml*) dosyaları, eğik çizgi tilde `~/` web kök dizinine işaret eder.</span><span class="sxs-lookup"><span data-stu-id="a067a-270">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="a067a-271">İle başlayan yollar `~/` sanal yol adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a067a-271">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="a067a-272">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="a067a-272">For more information, see <xref:fundamentals/static-files>.</span></span>
