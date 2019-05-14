---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: 9c7bc25d813ad17825ef03f5176882993cc2dd63
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610322"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="d1e2c-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="d1e2c-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="d1e2c-104">Bu makalede, ASP.NET Core uygulamaları geliştirmek nasıl anlamaya yönelik önemli konular bir genel bakıştır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="d1e2c-105">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="d1e2c-105">The Startup class</span></span>

<span data-ttu-id="d1e2c-106">`Startup` Sınıftır yeri:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="d1e2c-107">Uygulama tarafından gereken diğer hizmetler yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="d1e2c-108">Ardışık Düzen işleme isteği tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="d1e2c-109">Kod yapılandırmak için (veya *kaydetme*) Hizmetleri eklenir `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d1e2c-110">*Hizmetleri* uygulama tarafından kullanılan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="d1e2c-111">Örneğin, bir Entity Framework Core bağlam nesnesi bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="d1e2c-112">Ardışık Düzen işleme isteği yapılandırmak için kod eklenir `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="d1e2c-113">İşlem hattı, bir dizi olarak oluşan *ara yazılım* bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="d1e2c-114">Örneğin, bir ara yazılım bir statik dosyaları için istekleri işleyecek veya HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="d1e2c-115">Zaman uyumsuz işlemler gerçekleştirir her bir ara yazılım bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="d1e2c-116">Bir örneği aşağıdadır `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="d1e2c-117">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="d1e2c-118">Bağımlılık ekleme (hizmetler)</span><span class="sxs-lookup"><span data-stu-id="d1e2c-118">Dependency injection (services)</span></span>

<span data-ttu-id="d1e2c-119">ASP.NET Core yerleşik bağımlılık ekleme (dı) framework uygulamanın sınıfları için kullanılabilir yapar yapılandırılmış hizmet vardır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="d1e2c-120">Bir sınıf içinde bir hizmet örneği yapmanın bir yolu, gerekli türünde bir parametre ile bir oluşturucu oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="d1e2c-121">Parametresi, hizmet türü veya arabirim olabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="d1e2c-122">DI sistemi hizmeti zamanında sağlar.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="d1e2c-123">Bir Entity Framework Core bağlam nesnesini almak için DI kullanan bir sınıf aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="d1e2c-124">Vurgulanan satırı Oluşturucu ekleme örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="d1e2c-125">DI oluşturulmuş olsa da tercih ederseniz üçüncü taraf bir denetimi tersine çevirme (IOC) kapsayıcı takın izin verecek şekilde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="d1e2c-126">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="d1e2c-127">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="d1e2c-127">Middleware</span></span>

<span data-ttu-id="d1e2c-128">Ardışık Düzen işleme isteği ara yazılımı bileşenleri bir dizi olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="d1e2c-129">Zaman uyumsuz işlemler gerçekleştirir her bileşen bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="d1e2c-130">Kural gereği, harekete geçirerek bir ara yazılım bileşeni ardışık düzenine eklenen kendi `Use...` uzantı yönteminde `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="d1e2c-131">Örneğin, statik dosyaları işleme olanağı çağrı `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="d1e2c-132">Aşağıdaki örnekte vurgulanmış kodu ardışık düzen işleme isteği yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="d1e2c-133">ASP.NET Core içeren zengin bir yerleşik ara yazılım ve özel bir ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="d1e2c-134">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="d1e2c-135">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="d1e2c-135">The host</span></span>

<span data-ttu-id="d1e2c-136">ASP.NET Core uygulaması derleme bir *konak* başlangıç.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="d1e2c-137">Ana bilgisayar gibi tüm uygulamanın kaynakları yalıtan bir nesne şöyledir:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="d1e2c-138">Bir HTTP sunucusu uygulamasını</span><span class="sxs-lookup"><span data-stu-id="d1e2c-138">An HTTP server implementation</span></span>
* <span data-ttu-id="d1e2c-139">Ara yazılım bileşenleri</span><span class="sxs-lookup"><span data-stu-id="d1e2c-139">Middleware components</span></span>
* <span data-ttu-id="d1e2c-140">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d1e2c-140">Logging</span></span>
* <span data-ttu-id="d1e2c-141">DI</span><span class="sxs-lookup"><span data-stu-id="d1e2c-141">DI</span></span>
* <span data-ttu-id="d1e2c-142">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d1e2c-142">Configuration</span></span>

<span data-ttu-id="d1e2c-143">Tüm bağımlı kaynakları uygulamanın bir nesnesinde de dahil olmak üzere ana nedeni ömrü yönetimi,: uygulama başlatma ve normal şekilde kapatılmasını üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="d1e2c-144">Bir ana bilgisayar oluşturmak için kod `Program.Main` ve izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="d1e2c-145">Konak bir parçası olan her bir kaynak yapılandırmak için yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="d1e2c-146">Bu istek için bir oluşturucu yöntemi çağrılır hep birlikte ve konak nesnesi örneği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d1e2c-147">`CreateHostBuilder` Oluşturucu dış bileşenleri gibi aşırı yükleme yöntemini tanımlayan özel adı [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-147">`CreateHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="d1e2c-148">ASP.NET Core 3.0 veya üstü, genel ana bilgisayar (`Host` sınıfı) veya Web ana bilgisayarı (`WebHost` sınıfı) bir web uygulamasında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-148">In ASP.NET Core 3.0 or later, Generic Host (`Host` class) or Web Host (`WebHost` class) can be used in a web app.</span></span> <span data-ttu-id="d1e2c-149">Genel konak önerilir ve Web ana bilgisayarı, kullanılabilir geriye dönük uyumluluk.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-149">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="d1e2c-150">Bir çerçeve sağlar `CreateDefaultBuilder` ve `ConfigureWebHostDefaults` kullanılan aşağıdaki gibi seçeneklere sahip bir konak için yaygın olarak ayarlamak için yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-150">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="d1e2c-151">Kullanım [Kestrel](#servers) web sunucusu ve etkin IIS tümleştirme olarak.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-151">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="d1e2c-152">Yük yapılandırmasından *appsettings.json*, *appsettings. { Ortam adı} .json*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynaklarını.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-152">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="d1e2c-153">Günlük çıktısı, konsol ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-153">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="d1e2c-154">Bir konak yapılar örnek kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-154">Here's sample code that builds a host.</span></span> <span data-ttu-id="d1e2c-155">Yaygın olarak kullanılan seçenekler konakla ayarlama yöntemleri vurgulanmıştır:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-155">The methods that set up the host with commonly used options are highlighted:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="d1e2c-156">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-156">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d1e2c-157">`CreateWebHostBuilder` Oluşturucu dış bileşenleri gibi aşırı yükleme yöntemini tanımlayan özel adı [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-157">`CreateWebHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="d1e2c-158">ASP.NET Core 2.x kullanan Web ana bilgisayarı (`WebHost` sınıfı) web uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-158">ASP.NET Core 2.x uses Web Host (`WebHost` class) for web apps.</span></span> <span data-ttu-id="d1e2c-159">Bir çerçeve sağlar `CreateDefaultBuilder` seçenekleri, aşağıdakiler gibi yaygın olarak konakla ayarlamak için kullanılan:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-159">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="d1e2c-160">Kullanım [Kestrel](#servers) web sunucusu ve etkin IIS tümleştirme olarak.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-160">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="d1e2c-161">Yük yapılandırmasından *appsettings.json*, *appsettings. { Ortam adı} .json*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynaklarını.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-161">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="d1e2c-162">Günlük çıktısı, konsol ve hata ayıklama sağlayıcılarına gönderin.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-162">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="d1e2c-163">Bir konak yapılar örnek kod aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-163">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="d1e2c-164">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="d1e2c-165">Gelişmiş Ana senaryoları</span><span class="sxs-lookup"><span data-stu-id="d1e2c-165">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d1e2c-166">Genel konak kullanmak .NET Core için herhangi bir uygulama kullanılabilir&mdash;yalnızca ASP.NET Core uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-166">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="d1e2c-167">Genel ana bilgisayar (`Host` sınıfı) günlüğe kaydetme, DI, yapılandırma ve uygulama ömrü yönetimi gibi çapraz kesme framework uzantıları kullanmak için uygulama diğer türleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-167">Generic Host (`Host` class) allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="d1e2c-168">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d1e2c-169">Web ana bilgisayarı, hangi .NET uygulamalarının diğer türleri için gerekli olmayan bir HTTP sunucusu uygulamasını dahil etmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-169">Web Host is designed to include an HTTP server implementation, which isn't required for other kinds of .NET apps.</span></span> <span data-ttu-id="d1e2c-170">ASP.NET Core 2.1 genel konak başlatılıyor (`Host` sınıfı) bir .NET Core uygulaması kullanmak kullanılabilir&mdash;yalnızca ASP.NET Core uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-170">Starting in ASP.NET Core 2.1, the Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="d1e2c-171">Genel konak günlüğe kaydetme, DI, yapılandırma ve uygulama ömrü yönetimi gibi çapraz kesme framework uzantıları kullanmak için uygulama diğer türleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-171">Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="d1e2c-172">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-172">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

<span data-ttu-id="d1e2c-173">Konak, arka plan görevleri çalıştırmak için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-173">You can also use the host to run background tasks.</span></span> <span data-ttu-id="d1e2c-174">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-174">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="d1e2c-175">Sunucular</span><span class="sxs-lookup"><span data-stu-id="d1e2c-175">Servers</span></span>

<span data-ttu-id="d1e2c-176">ASP.NET Core uygulaması, HTTP isteklerini dinlemek için bir HTTP sunucusu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-176">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="d1e2c-177">Uygulamaya bir dizi olarak sunucu yüzeyleri istekleri [istek özellikleri](xref:fundamentals/request-features) içine oluşan bir `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-177">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="d1e2c-178">Windows</span><span class="sxs-lookup"><span data-stu-id="d1e2c-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="d1e2c-179">ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-179">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="d1e2c-180">*Kestrel'i* bir platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-180">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="d1e2c-181">Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-181">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="d1e2c-182">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-182">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="d1e2c-183">*IIS HTTP sunucusu* IIS kullanan bir windows Server'de olduğu.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-183">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="d1e2c-184">IIS ve ASP.NET Core uygulaması, bu sunucu ile aynı işlem içinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-184">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="d1e2c-185">*HTTP.sys* , IIS ile kullanılmayan Windows için sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-185">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d1e2c-186">macOS</span><span class="sxs-lookup"><span data-stu-id="d1e2c-186">macOS</span></span>](#tab/macos)

<span data-ttu-id="d1e2c-187">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-187">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d1e2c-188">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-188">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d1e2c-189">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-189">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d1e2c-190">Linux</span><span class="sxs-lookup"><span data-stu-id="d1e2c-190">Linux</span></span>](#tab/linux)

<span data-ttu-id="d1e2c-191">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-191">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d1e2c-192">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-192">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d1e2c-193">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-193">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="d1e2c-194">Windows</span><span class="sxs-lookup"><span data-stu-id="d1e2c-194">Windows</span></span>](#tab/windows)

<span data-ttu-id="d1e2c-195">ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-195">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="d1e2c-196">*Kestrel'i* bir platformlar arası web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-196">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="d1e2c-197">Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-197">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="d1e2c-198">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-198">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="d1e2c-199">*HTTP.sys* , IIS ile kullanılmayan Windows için sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-199">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d1e2c-200">macOS</span><span class="sxs-lookup"><span data-stu-id="d1e2c-200">macOS</span></span>](#tab/macos)

<span data-ttu-id="d1e2c-201">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-201">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d1e2c-202">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-202">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d1e2c-203">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-203">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d1e2c-204">Linux</span><span class="sxs-lookup"><span data-stu-id="d1e2c-204">Linux</span></span>](#tab/linux)

<span data-ttu-id="d1e2c-205">ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-205">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d1e2c-206">ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-206">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d1e2c-207">Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](http://nginx.org) veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-207">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="d1e2c-208">Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-208">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="d1e2c-209">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d1e2c-209">Configuration</span></span>

<span data-ttu-id="d1e2c-210">ASP.NET Core ayarları ad-değer çiftleri olarak sıralı bir dizi yapılandırma sağlayıcıları alır bir yapılandırma çerçevesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-210">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="d1e2c-211">Yerleşik yapılandırma sağlayıcıları vardır, kaynakları çeşitli gibi *.json* dosyaları *.xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-211">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="d1e2c-212">Özel yapılandırma sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-212">You can also write custom configuration providers.</span></span>

<span data-ttu-id="d1e2c-213">Örneğin, bu yapılandırma geldiği belirtebilirsiniz *appsettings.json* ve ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-213">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="d1e2c-214">Sonra ne zaman değerini *ConnectionString* istenildiğinde, framework ilk olarak görünür *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-214">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="d1e2c-215">Değer var. bulunursa, aynı zamanda bir ortam değişkeni, ortam değişkeninin değerini öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-215">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="d1e2c-216">ASP.NET Core sağlar parolalar gibi gizli yapılandırma verilerini yönetmek için bir [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-216">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d1e2c-217">Üretim gizli öğeleri için öneririz [Azure anahtar kasası](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-217">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="d1e2c-218">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-218">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="d1e2c-219">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="d1e2c-219">Options</span></span>

<span data-ttu-id="d1e2c-220">Mümkünse, ASP.NET Core aşağıdaki *seçenekleri deseni* depolamak ve yapılandırma değerleri almak için.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-220">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="d1e2c-221">Seçenekleri deseni sınıfları, ilgili ayar gruplarını temsil etmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-221">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="d1e2c-222">Örneğin, aşağıdaki kodu WebSockets seçeneklerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-222">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="d1e2c-223">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-223">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="d1e2c-224">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="d1e2c-224">Environments</span></span>

<span data-ttu-id="d1e2c-225">Yürütme ortamlarını gibi *geliştirme*, *hazırlama*, ve *üretim*, ASP.NET Core, birinci sınıf bir kavram olan.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-225">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="d1e2c-226">Bir uygulama ortamı çalışır ayarlayarak belirtebilirsiniz `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-226">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d1e2c-227">ASP.NET Core uygulaması başlatma sırasında bu ortam değişkenini okur ve değerini depolayan bir `IHostingEnvironment` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-227">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="d1e2c-228">Ortam nesnesi herhangi bir uygulamayı DI aracılığıyla kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-228">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="d1e2c-229">Aşağıdaki örnek, koddan `Startup` sınıfı yalnızca geliştirme çalıştığında, ayrıntılı hata bilgileri sağlamak için uygulama yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-229">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="d1e2c-230">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-230">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="d1e2c-231">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d1e2c-231">Logging</span></span>

<span data-ttu-id="d1e2c-232">ASP.NET Core çeşitli günlük yerleşik ve üçüncü taraf sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-232">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="d1e2c-233">Kullanılabilir sağlayıcılar şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-233">Available providers include the following:</span></span>

* <span data-ttu-id="d1e2c-234">Konsol</span><span class="sxs-lookup"><span data-stu-id="d1e2c-234">Console</span></span>
* <span data-ttu-id="d1e2c-235">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="d1e2c-235">Debug</span></span>
* <span data-ttu-id="d1e2c-236">Windows olay izleme</span><span class="sxs-lookup"><span data-stu-id="d1e2c-236">Event Tracing on Windows</span></span>
* <span data-ttu-id="d1e2c-237">Windows olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="d1e2c-237">Windows Event Log</span></span>
* <span data-ttu-id="d1e2c-238">TraceSource</span><span class="sxs-lookup"><span data-stu-id="d1e2c-238">TraceSource</span></span>
* <span data-ttu-id="d1e2c-239">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="d1e2c-239">Azure App Service</span></span>
* <span data-ttu-id="d1e2c-240">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="d1e2c-240">Azure Application Insights</span></span>

<span data-ttu-id="d1e2c-241">Yazma günlükleri öğesinden herhangi bir uygulamanın kodu alarak bir `ILogger` DI ve günlük metotları çağırma nesnesi.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-241">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="d1e2c-242">İşte kullanan örnek kodu bir `ILogger` Oluşturucu ekleme ve vurgulanmış günlük yöntem çağrılarını içeren bir nesne.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-242">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="d1e2c-243">`ILogger` Arabirimi herhangi bir sayıda alanlar için oturum açma sağlayıcısı geçirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-243">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="d1e2c-244">Alanları bir ileti dizesi oluşturmak için yaygın olarak kullanılır, ancak sağlayıcısı Ayrıca bunları ayrı alanlar olarak bir veri deposuna gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-244">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="d1e2c-245">Bu özelliği uygulamak günlük sağlayıcıları için mümkün kılar [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-245">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="d1e2c-246">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-246">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="d1e2c-247">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="d1e2c-247">Routing</span></span>

<span data-ttu-id="d1e2c-248">A *rota* bir işleyici eşlenmiş bir URL deseni şudur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-248">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="d1e2c-249">İşleyici, bir Razor sayfası, bir MVC denetleyicisi veya bir ara yazılım bir eylem yöntemi genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-249">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="d1e2c-250">Uygulamanız tarafından kullanılan URL'leri üzerinde denetim ASP.NET Core yönlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-250">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="d1e2c-251">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-251">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="d1e2c-252">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="d1e2c-252">Error handling</span></span>

<span data-ttu-id="d1e2c-253">ASP.NET Core gibi hataları işlemek için yerleşik özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-253">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="d1e2c-254">Bir geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="d1e2c-254">A developer exception page</span></span>
* <span data-ttu-id="d1e2c-255">Özel hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="d1e2c-255">Custom error pages</span></span>
* <span data-ttu-id="d1e2c-256">Statik durumu kod sayfaları</span><span class="sxs-lookup"><span data-stu-id="d1e2c-256">Static status code pages</span></span>
* <span data-ttu-id="d1e2c-257">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="d1e2c-257">Startup exception handling</span></span>

<span data-ttu-id="d1e2c-258">Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-258">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="d1e2c-259">HTTP isteğinde bulunma</span><span class="sxs-lookup"><span data-stu-id="d1e2c-259">Make HTTP requests</span></span>

<span data-ttu-id="d1e2c-260">Uygulanışı `IHttpClientFactory` oluşturmak için kullanılabilir `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-260">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="d1e2c-261">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="d1e2c-261">The factory:</span></span>

* <span data-ttu-id="d1e2c-262">Adlandırma ve mantıksal yapılandırmak için merkezi bir konum sağlar `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-262">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d1e2c-263">Örneğin, bir *github* istemci kayıtlı ve GitHub erişim sağlamak için yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-263">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="d1e2c-264">Varsayılan istemci diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-264">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="d1e2c-265">Kayıt ve giden bir istek ara yazılım ardışık düzenini oluşturmak için birden fazla temsilci işleyicileri zincirleme destekler.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-265">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d1e2c-266">Bu düzen, ASP.NET Core gelen ara yazılım ardışık benzerdir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-266">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="d1e2c-267">Desen etrafında HTTP isteklerini, önbelleğe alma, hata, seri hale getirme, işleme ve günlüğe kaydetme gibi çapraz kesme konuları yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-267">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="d1e2c-268">Tümleşik şekilde çalışarak *Polly*, geçici hata işlemeye yönelik popüler bir üçüncü taraf kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-268">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="d1e2c-269">Havuzu ve arka plandaki, yaşam süresini yöneten `HttpClientMessageHandler` el ile yönetilmesi sırasında oluşan Genel DNS sorunları önlemek için örnekleri `HttpClient` yaşam süresi yok.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-269">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d1e2c-270">Yapılandırılabilir günlük deneyimi ekler (aracılığıyla `ILogger`) fabrikası tarafından oluşturulan istemcileri aracılığıyla gönderilen tüm istekler için.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-270">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d1e2c-271">Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-271">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="d1e2c-272">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="d1e2c-272">Content root</span></span>

<span data-ttu-id="d1e2c-273">Temel yol, Razor dosyaları gibi uygulama tarafından kullanılan herhangi bir özel içerik için içerik köküdür.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-273">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="d1e2c-274">Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosya için taban yoludur.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-274">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="d1e2c-275">Alternatif bir konuma olabilir belirtilen [konak oluşturmaya](#host).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-275">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d1e2c-276">Daha fazla bilgi için [içerik kök](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-276">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d1e2c-277">Daha fazla bilgi için [içerik kök](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-277">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="d1e2c-278">Web kökü</span><span class="sxs-lookup"><span data-stu-id="d1e2c-278">Web root</span></span>

<span data-ttu-id="d1e2c-279">Web kökü (diğer adıyla *webroot*) genel, statik kaynakları, CSS, JavaScript ve görüntü dosyaları gibi temel yolu.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-279">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="d1e2c-280">Statik dosya ara yazılımı, varsayılan olarak yalnızca bir hizmet web kök dizinine (ve alt dizinleri) dosyalarından verecektir.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-280">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="d1e2c-281">Varsayılan olarak web kök yolu *{içerik kök} / wwwroot*, ancak farklı bir konuma A'da belirtildiği zaman [konak oluşturmaya](#host).</span><span class="sxs-lookup"><span data-stu-id="d1e2c-281">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="d1e2c-282">Razor'daki (*.cshtml*) dosyaları, eğik çizgi tilde `~/` web kök dizinine işaret eder.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-282">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="d1e2c-283">İle başlayan yollar `~/` sanal yol adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-283">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="d1e2c-284">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="d1e2c-284">For more information, see <xref:fundamentals/static-files>.</span></span>
