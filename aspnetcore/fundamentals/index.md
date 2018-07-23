---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları geliştirmek için temel oluşturabilen altyapı kavramları keşfedin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 30c456685ce26522faff9b58fbd2977ad2f2869a
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202633"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="b68dc-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="b68dc-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="b68dc-104">ASP.NET Core uygulamasını bir web sunucusunu oluşturan bir konsol uygulaması olan kendi `Main` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b68dc-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b68dc-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b68dc-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="b68dc-106">`Main` Yöntemini çağıran `WebHost.CreateDefaultBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu deseni izler.</span><span class="sxs-lookup"><span data-stu-id="b68dc-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="b68dc-107">Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="b68dc-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="b68dc-108">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="b68dc-109">IIS üzerinde çalıştırmak ASP.NET Core'nın web ana bilgisayarı varsa çalışır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="b68dc-110">Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="b68dc-111">`UseStartup` açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="b68dc-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="b68dc-112">`IWebHostBuilder`, dönüş türünü `WebHost.CreateDefaultBuilder` çağrısı birçok isteğe bağlı yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="b68dc-113">Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırmak ve `UseContentRoot` kök içerik dizini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="b68dc-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="b68dc-114">`Build` Ve `Run` yöntemleri yapı `IWebHost` nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b68dc-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b68dc-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="b68dc-116">`Main` Yöntemi kullanan `WebHostBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu deseni izler.</span><span class="sxs-lookup"><span data-stu-id="b68dc-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="b68dc-117">Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="b68dc-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="b68dc-118">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="b68dc-119">Diğer web sunucuları gibi [WebListener](xref:fundamentals/servers/weblistener), uygun bir genişletme yöntemi çağrılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="b68dc-120">`UseStartup` açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="b68dc-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="b68dc-121">`WebHostBuilder` dahil olmak üzere birçok isteğe bağlı yöntemler sağlar `UseIISIntegration` IIS ve IIS Express barındırmak ve `UseContentRoot` kök içerik dizini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="b68dc-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="b68dc-122">`Build` Ve `Run` yöntemleri yapı `IWebHost` nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="b68dc-123">Başlangıç</span><span class="sxs-lookup"><span data-stu-id="b68dc-123">Startup</span></span>

<span data-ttu-id="b68dc-124">`UseStartup` Metodunda `WebHostBuilder` belirtir `Startup` uygulamanız için sınıf:</span><span class="sxs-lookup"><span data-stu-id="b68dc-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b68dc-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b68dc-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b68dc-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b68dc-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="b68dc-127">`Startup` Sınıfıdır istek işleme ardışık düzen tanımladığınız ve uygulama tarafından gereken diğer hizmetler yapılandırıldığı.</span><span class="sxs-lookup"><span data-stu-id="b68dc-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="b68dc-128">`Startup` Sınıfı genel olmalıdır ve aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="b68dc-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="b68dc-129">`ConfigureServices` tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Core kimlik) tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="b68dc-130">`Configure` tanımlar [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenini için.</span><span class="sxs-lookup"><span data-stu-id="b68dc-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="b68dc-131">Daha fazla bilgi için [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="b68dc-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="b68dc-132">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="b68dc-132">Content root</span></span>

<span data-ttu-id="b68dc-133">Temel yol görünümleri gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfaları](xref:razor-pages/index)ve statik varlıklar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:razor-pages/index), and static assets.</span></span> <span data-ttu-id="b68dc-134">Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="b68dc-135">Web kökü</span><span class="sxs-lookup"><span data-stu-id="b68dc-135">Web root</span></span>

<span data-ttu-id="b68dc-136">Bir uygulamanın web kök CSS, JavaScript ve görüntü dosyaları gibi genel, statik kaynakları içeren proje dizinindedir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="b68dc-137">Bağımlılık ekleme (Hizmetler)</span><span class="sxs-lookup"><span data-stu-id="b68dc-137">Dependency injection (services)</span></span>

<span data-ttu-id="b68dc-138">Bir hizmeti bir uygulamada ortak tüketim için hazırlanmış bir bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="b68dc-139">Hizmetleri aracılığıyla yapılan [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b68dc-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b68dc-140">ASP.NET Core içeren yerel **miyim**nİşlevin **o**f **C**destekleyen Tim (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="b68dc-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="b68dc-141">İsterseniz, varsayılan yerel kapsayıcı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b68dc-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="b68dc-142">Kendi gevşek bağ modelini avantajı yanı sıra DI Hizmetleri uygulamanız genelinde kullanılabilir yapar (örneğin, [günlüğü](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="b68dc-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="b68dc-143">Daha fazla bilgi için [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b68dc-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="b68dc-144">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="b68dc-144">Middleware</span></span>

<span data-ttu-id="b68dc-145">ASP.NET Core, istek kullanarak işlem hattı oluşturma [ara yazılım](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="b68dc-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="b68dc-146">ASP.NET Core ara yazılım üzerinde zaman uyumsuz mantık gerçekleştiren bir `HttpContext` ve dizideki sonraki ara yazılımı çağırır veya istek doğrudan sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="b68dc-147">"XYZ" adlı bir ara yazılım bileşeni çağırarak eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b68dc-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="b68dc-148">ASP.NET Core, zengin bir yerleşik ara yazılım içerir:</span><span class="sxs-lookup"><span data-stu-id="b68dc-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="b68dc-149">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="b68dc-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="b68dc-150">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="b68dc-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="b68dc-151">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="b68dc-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="b68dc-152">Yanıt Sıkıştırma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="b68dc-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="b68dc-153">URL Yeniden Yazma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="b68dc-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="b68dc-154">[OWIN](http://owin.org)-tabanlı ara yazılım, ASP.NET Core uygulamaları için kullanılabilir ve kendi özel bir ara yazılım yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b68dc-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="b68dc-155">Daha fazla bilgi için [ara yazılım](xref:fundamentals/middleware/index) ve [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="b68dc-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="b68dc-156">HTTP isteklerini başlatma</span><span class="sxs-lookup"><span data-stu-id="b68dc-156">Initiate HTTP requests</span></span>

<span data-ttu-id="b68dc-157">Kullanma hakkında bilgi için `IHttpClientFactory` erişimi `HttpClient` HTTP isteğinde bulunmak için örnekleri görmek [başlatmak HTTP istekleri](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="b68dc-157">For information about using `IHttpClientFactory` to access `HttpClient` instances to make HTTP requests, see [Initiate HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="b68dc-158">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="b68dc-158">Environments</span></span>

<span data-ttu-id="b68dc-159">Ortamlar, "Geliştirme" ve "Üretim" gibi ASP.NET core'da birinci sınıf bir kavram ve ortam değişkenlerini kullanarak ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b68dc-159">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="b68dc-160">Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="b68dc-160">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="b68dc-161">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b68dc-161">Configuration</span></span>

<span data-ttu-id="b68dc-162">ASP.NET Core ad-değer çiftlerine göre bir yapılandırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-162">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="b68dc-163">Yapılandırma modeli temel almayan `System.Configuration` veya *web.config*. Yapılandırma ayarları sıralı bir dizi yapılandırma sağlayıcıları alır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-163">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="b68dc-164">Yerleşik yapılandırma sağlayıcıları, çeşitli dosya biçimleri (XML, JSON, INI) ve ortama dayalı yapılandırma etkinleştirmek için ortam değişkenlerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="b68dc-164">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="b68dc-165">Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b68dc-165">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="b68dc-166">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b68dc-166">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="b68dc-167">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="b68dc-167">Logging</span></span>

<span data-ttu-id="b68dc-168">ASP.NET Core çeşitli günlük sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler.</span><span class="sxs-lookup"><span data-stu-id="b68dc-168">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="b68dc-169">Yerleşik sağlayıcılar, bir veya daha fazla hedefe gönderen günlükleri destekler.</span><span class="sxs-lookup"><span data-stu-id="b68dc-169">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="b68dc-170">Üçüncü taraf günlük altyapılarına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-170">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="b68dc-171">Daha fazla bilgi için [günlüğe kaydetme](xref:fundamentals/logging/index)</span><span class="sxs-lookup"><span data-stu-id="b68dc-171">For more information, see [Logging](xref:fundamentals/logging/index)</span></span>

## <a name="error-handling"></a><span data-ttu-id="b68dc-172">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="b68dc-172">Error handling</span></span>

<span data-ttu-id="b68dc-173">ASP.NET Core uygulamalarında Geliştirici özel durum sayfasında, özel hata sayfaları, statik durumu kod sayfaları ve başlangıç özel durum işleme dahil olmak üzere, hataları işleme için yerleşik özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-173">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="b68dc-174">Daha fazla bilgi için [hatalarının nasıl işleneceğini](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="b68dc-174">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="b68dc-175">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="b68dc-175">Routing</span></span>

<span data-ttu-id="b68dc-176">ASP.NET Core, yol işleyicisi uygulama isteklerinin yönlendirme özellikleri sunar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-176">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="b68dc-177">Daha fazla bilgi için [yönlendirme](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="b68dc-177">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="b68dc-178">Dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="b68dc-178">File providers</span></span>

<span data-ttu-id="b68dc-179">ASP.NET Core, dosya sistemi erişimini kullanarak platformlar arasında dosyaları ile çalışma için ortak bir arabirim sunan dosya sağlayıcıları soyutlar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-179">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="b68dc-180">Daha fazla bilgi için [dosya sağlayıcıları](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="b68dc-180">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="b68dc-181">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="b68dc-181">Static files</span></span>

<span data-ttu-id="b68dc-182">Statik dosya ara yazılımı statik dosyaları, HTML, CSS, görüntü ve JavaScript gibi işlev görür.</span><span class="sxs-lookup"><span data-stu-id="b68dc-182">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="b68dc-183">Daha fazla bilgi için [statik dosyalar](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b68dc-183">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="b68dc-184">Barındırma</span><span class="sxs-lookup"><span data-stu-id="b68dc-184">Hosting</span></span>

<span data-ttu-id="b68dc-185">ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*, uygulama başlatma ve ömür yönetimi için sorumlu olduğu.</span><span class="sxs-lookup"><span data-stu-id="b68dc-185">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="b68dc-186">Daha fazla bilgi için [ASP.NET Core ana](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="b68dc-186">For more information, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="b68dc-187">Oturum ve uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="b68dc-187">Session and app state</span></span>

<span data-ttu-id="b68dc-188">ASP.NET Core, kullanıcının bir web uygulaması gözatar sırada oturum ve uygulama durumu korumak için çeşitli yaklaşımlar sunar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-188">ASP.NET Core offers several approaches to preserve session and app state while the user browses a web app.</span></span>

<span data-ttu-id="b68dc-189">Daha fazla bilgi için [oturum ve uygulama durumu](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="b68dc-189">For more information, see [Session and app state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="b68dc-190">Sunucular</span><span class="sxs-lookup"><span data-stu-id="b68dc-190">Servers</span></span>

<span data-ttu-id="b68dc-191">Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez.</span><span class="sxs-lookup"><span data-stu-id="b68dc-191">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="b68dc-192">Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-192">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="b68dc-193">İletilen istek, arabirimler aracılığıyla erişilebilen bir özellik nesne olarak paketlenir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-193">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="b68dc-194">ASP.NET Core içeren olarak adlandırılan bir yönetilen, platformlar arası web sunucusuna [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b68dc-194">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="b68dc-195">Kestrel'i genellikle çalıştığı bir üretim web sunucusu gibi [IIS](https://www.iis.net/) veya [Ngınx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="b68dc-195">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="b68dc-196">Kestrel'i bir uç sunucusu çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-196">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="b68dc-197">Daha fazla bilgi için [sunucuları](xref:fundamentals/servers/index) ve aşağıdaki konular:</span><span class="sxs-lookup"><span data-stu-id="b68dc-197">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="b68dc-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="b68dc-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="b68dc-199">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="b68dc-199">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="b68dc-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="b68dc-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="b68dc-201">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="b68dc-201">Globalization and localization</span></span>

<span data-ttu-id="b68dc-202">ASP.NET Core ile çok dilli bir Web sitesi oluşturma sitenizin daha geniş kitlelere ulaşmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-202">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="b68dc-203">ASP.NET Core, farklı dillere ve kültürlere yerelleştirme için Hizmetler ve ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-203">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="b68dc-204">Daha fazla bilgi için [Genelleştirme ve Yerelleştirme](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="b68dc-204">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="b68dc-205">İstek özellikleri</span><span class="sxs-lookup"><span data-stu-id="b68dc-205">Request features</span></span>

<span data-ttu-id="b68dc-206">Web sunucusu uygulaması ayrıntıları HTTP istekleriyle ilgili ve yanıtları arabirimlerde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-206">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="b68dc-207">Bu arabirimler, uygulamanın barındırma işlem hattı oluşturup için sunucu uygulamaları ve ara yazılım tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b68dc-207">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="b68dc-208">Daha fazla bilgi için [istek özellikleri](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="b68dc-208">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="b68dc-209">Arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="b68dc-209">Background tasks</span></span>

<span data-ttu-id="b68dc-210">Arka plan görevleri olarak gerçekleştirilen *barındırılan hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="b68dc-210">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="b68dc-211">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır [Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="b68dc-211">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="b68dc-212">Daha fazla bilgi için [görevleri barındırılan hizmetler ile arka plan](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="b68dc-212">For more information, see [Background tasks with hosted services](xref:fundamentals/host/hosted-services).</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="b68dc-213">Erişim HttpContext</span><span class="sxs-lookup"><span data-stu-id="b68dc-213">Access HttpContext</span></span>

<span data-ttu-id="b68dc-214">Erişim `HttpContext` aracılığıyla [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ile varsayılan uygulaması [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="b68dc-214">Access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

<span data-ttu-id="b68dc-215">Daha fazla bilgi için bkz. <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="b68dc-215">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="b68dc-216">.NET (OWIN) için açık Web arabirimi</span><span class="sxs-lookup"><span data-stu-id="b68dc-216">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="b68dc-217">ASP.NET Core, .NET (OWIN) için açık Web arabirimi destekler.</span><span class="sxs-lookup"><span data-stu-id="b68dc-217">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="b68dc-218">OWIN web uygulamalarının web sunucularından ölçeklendirilebilmeleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b68dc-218">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="b68dc-219">Daha fazla bilgi için [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="b68dc-219">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="b68dc-220">WebSockets</span><span class="sxs-lookup"><span data-stu-id="b68dc-220">WebSockets</span></span>

<span data-ttu-id="b68dc-221">[WebSocket](https://wikipedia.org/wiki/WebSocket) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="b68dc-221">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="b68dc-222">Sohbet, yürütebilmektedir oyunlar gibi uygulamalar için kullanılır ve gerçek zamanlı bir web uygulaması işlevindeki istediğiniz herhangi bir yerde.</span><span class="sxs-lookup"><span data-stu-id="b68dc-222">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="b68dc-223">ASP.NET Core web yuvası özellikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="b68dc-223">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="b68dc-224">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="b68dc-224">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="b68dc-225">Microsoft.AspNetCore.App metapackage</span><span class="sxs-lookup"><span data-stu-id="b68dc-225">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="b68dc-226">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage paket Yönetimi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-226">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span> <span data-ttu-id="b68dc-227">Daha fazla bilgi için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b68dc-227">For more information, see [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="b68dc-228">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="b68dc-228">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="b68dc-229">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:</span><span class="sxs-lookup"><span data-stu-id="b68dc-229">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="b68dc-230">Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="b68dc-230">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="b68dc-231">Tüm paketleri Entity Framework Core tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="b68dc-231">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="b68dc-232">ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="b68dc-232">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="b68dc-233">Daha fazla bilgi için [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b68dc-233">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="b68dc-234">.NET core ve .NET Framework çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="b68dc-234">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="b68dc-235">ASP.NET Core uygulaması, .NET Core veya .NET Framework çalışma zamanı hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b68dc-235">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="b68dc-236">Daha fazla bilgi için [.NET Core ve .NET Framework arasında seçim yapma](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="b68dc-236">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="b68dc-237">ASP.NET Core ile ASP.NET arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="b68dc-237">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="b68dc-238">ASP.NET Core ile ASP.NET arasında seçim yapma hakkında daha fazla bilgi için bkz: [ASP.NET Core ve ASP.NET arasında seçim yapma](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="b68dc-238">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
