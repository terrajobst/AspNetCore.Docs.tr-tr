---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmak için temel kavramları bulur.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: b74870c93ea0120e43931bb75f47bab19a6de201
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="2904b-103">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="2904b-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="2904b-104">Bir web sunucusu oluşturan bir konsol uygulaması bir ASP.NET Core uygulamadır kendi `Main` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2904b-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2904b-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2904b-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="2904b-106">`Main` Yöntemini çağırır `WebHost.CreateDefaultBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu düzenini izler.</span><span class="sxs-lookup"><span data-stu-id="2904b-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="2904b-107">Oluşturucu web sunucusu tanımlayan yöntemleri vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="2904b-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="2904b-108">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır.</span><span class="sxs-lookup"><span data-stu-id="2904b-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="2904b-109">ASP.NET Core'nın web ana bilgisayarı, IIS'de çalışan varsa çalışır.</span><span class="sxs-lookup"><span data-stu-id="2904b-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="2904b-110">Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun uzantı metodu çağırma tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2904b-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="2904b-111">`UseStartup` açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="2904b-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="2904b-112">`IWebHostBuilder`, dönüş türü `WebHost.CreateDefaultBuilder` çağırma, birçok isteğe bağlı yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="2904b-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="2904b-113">Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırma ve `UseContentRoot` kök içerik dizinini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="2904b-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="2904b-114">`Build` Ve `Run` yöntemleri yapı `IWebHost` uygulamayı barındıran ve HTTP isteklerini dinlemeye başlar nesnesi.</span><span class="sxs-lookup"><span data-stu-id="2904b-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2904b-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2904b-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="2904b-116">`Main` Yöntemi kullanan `WebHostBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu düzenini izler.</span><span class="sxs-lookup"><span data-stu-id="2904b-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="2904b-117">Oluşturucu web sunucusu tanımlayan yöntemleri vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="2904b-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="2904b-118">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2904b-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="2904b-119">Diğer web sunucuları gibi [WebListener](xref:fundamentals/servers/weblistener), uygun uzantı metodu çağırma tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2904b-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="2904b-120">`UseStartup` açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="2904b-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="2904b-121">`WebHostBuilder` dahil olmak üzere birçok isteğe bağlı yöntemler sağlar `UseIISIntegration` IIS ve IIS Express barındırmak için ve `UseContentRoot` kök içerik dizinini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="2904b-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="2904b-122">`Build` Ve `Run` yöntemleri yapı `IWebHost` uygulamayı barındıran ve HTTP isteklerini dinlemeye başlar nesnesi.</span><span class="sxs-lookup"><span data-stu-id="2904b-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

* * *
## <a name="startup"></a><span data-ttu-id="2904b-123">Başlangıç</span><span class="sxs-lookup"><span data-stu-id="2904b-123">Startup</span></span>

<span data-ttu-id="2904b-124">`UseStartup` Yöntemi `WebHostBuilder` belirtir `Startup` sınıfı, uygulamanız için:</span><span class="sxs-lookup"><span data-stu-id="2904b-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2904b-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2904b-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2904b-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2904b-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

* * *
<span data-ttu-id="2904b-127">`Startup` Sınıfı olduğundan istek işleme ardışık düzen tanımladığınız ve burada uygulama tarafından gerekli tüm hizmetler yapılandırılmadı.</span><span class="sxs-lookup"><span data-stu-id="2904b-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="2904b-128">`Startup` Sınıfı genel olmalı ve aşağıdaki yöntemlerden içermelidir:</span><span class="sxs-lookup"><span data-stu-id="2904b-128">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="2904b-129">`ConfigureServices` tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Çekirdek, kimlik) tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2904b-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="2904b-130">`Configure` tanımlar [ara yazılım](xref:fundamentals/middleware/index) isteği ardışık düzeni için.</span><span class="sxs-lookup"><span data-stu-id="2904b-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="2904b-131">Daha fazla bilgi için bkz: [uygulama başlangıç](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2904b-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="2904b-132">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="2904b-132">Content root</span></span>

<span data-ttu-id="2904b-133">Taban yol görünümler gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfalarının](xref:mvc/razor-pages/index)ve statik varlıklar.</span><span class="sxs-lookup"><span data-stu-id="2904b-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="2904b-134">Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2904b-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="2904b-135">Web kök</span><span class="sxs-lookup"><span data-stu-id="2904b-135">Web root</span></span>

<span data-ttu-id="2904b-136">Web uygulama CSS, JavaScript ve görüntü dosyaları gibi ortak, statik kaynakları içeren proje dizininde köküdür.</span><span class="sxs-lookup"><span data-stu-id="2904b-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="2904b-137">Bağımlılık ekleme (Hizmetleri)</span><span class="sxs-lookup"><span data-stu-id="2904b-137">Dependency Injection (Services)</span></span>

<span data-ttu-id="2904b-138">Bir hizmeti bir uygulama içinde ortak tüketim yönelik bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="2904b-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="2904b-139">Hizmetleri kullanılabilir hale getirilir aracılığıyla [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2904b-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2904b-140">ASP.NET Core içeren yerel **ı**nİşlevin **o**f **C**destekleyen Tim (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="2904b-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="2904b-141">İsterseniz, varsayılan yerel kapsayıcı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2904b-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="2904b-142">Kendi gevşek bağlantı avantajı yanı sıra dı Hizmetleri uygulamanız genelinde kullanılabilir hale getirir (örneğin, [günlüğü](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="2904b-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="2904b-143">Daha fazla bilgi için bkz: [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2904b-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="2904b-144">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="2904b-144">Middleware</span></span>

<span data-ttu-id="2904b-145">ASP.NET çekirdek isteği kullanılarak ardışık düzeni oluşturma [Ara](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="2904b-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="2904b-146">ASP.NET Core Ara gerçekleştirir zaman uyumsuz mantığı bir `HttpContext` ve sırayla sonraki ara yazılımı çağırır veya istek doğrudan sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="2904b-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="2904b-147">"XYZ" adlı bir ara yazılım bileşeni çağırarak eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2904b-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="2904b-148">ASP.NET Core zengin bir yerleşik ara yazılımı içerir:</span><span class="sxs-lookup"><span data-stu-id="2904b-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="2904b-149">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="2904b-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="2904b-150">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="2904b-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="2904b-151">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2904b-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="2904b-152">Yanıt Sıkıştırma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="2904b-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="2904b-153">URL Yeniden Yazma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="2904b-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="2904b-154">[OWIN](http://owin.org)-tabanlı ara yazılım ASP.NET Core uygulamaları için kullanılabilir ve kendi özel ara yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2904b-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="2904b-155">Daha fazla bilgi için bkz: [ara yazılımı](xref:fundamentals/middleware/index) ve [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="2904b-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="2904b-156">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="2904b-156">Environments</span></span>

<span data-ttu-id="2904b-157">"Geliştirme" ve "Üretim" gibi ortamları ASP.NET Core, birinci sınıf bir kavram ve ortam değişkenlerini kullanma ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2904b-157">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="2904b-158">Daha fazla bilgi için bkz: [çalışma ile birden çok ortamları](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="2904b-158">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="2904b-159">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2904b-159">Configuration</span></span>

<span data-ttu-id="2904b-160">ASP.NET Core üzerinde ad-değer çiftleri temel alan bir yapılandırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="2904b-160">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="2904b-161">Yapılandırma modeli bağlı değil `System.Configuration` veya *web.config*. Yapılandırma ayarları yapılandırma sağlayıcılarının bir sıralanmış kümesinden alır.</span><span class="sxs-lookup"><span data-stu-id="2904b-161">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="2904b-162">Yerleşik yapılandırma sağlayıcıları çeşitli dosya biçimleri (XML, JSON, INI) ve ortam tabanlı yapılandırmasını etkinleştirmek için ortam değişkenlerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="2904b-162">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="2904b-163">Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2904b-163">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="2904b-164">Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2904b-164">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="2904b-165">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="2904b-165">Logging</span></span>

<span data-ttu-id="2904b-166">ASP.NET çekirdeği günlüğü sağlayıcıları çeşitli çalışır bir günlük API destekler.</span><span class="sxs-lookup"><span data-stu-id="2904b-166">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="2904b-167">Yerleşik sağlayıcılar, bir veya daha fazla hedeflere gönderen günlükleri destekler.</span><span class="sxs-lookup"><span data-stu-id="2904b-167">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="2904b-168">Üçüncü taraf günlük altyapıları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2904b-168">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="2904b-169">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="2904b-169">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="2904b-170">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="2904b-170">Error handling</span></span>

<span data-ttu-id="2904b-171">ASP.NET Core Geliştirici özel durum sayfasında, özel hata sayfaları, statik durum kod sayfaları ve başlangıç özel durum işleme gibi uygulamalar, hataları işlemek için yerleşik özellikler vardır.</span><span class="sxs-lookup"><span data-stu-id="2904b-171">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="2904b-172">Daha fazla bilgi için bkz: [hataların nasıl işleneceğini](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="2904b-172">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="2904b-173">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="2904b-173">Routing</span></span>

<span data-ttu-id="2904b-174">ASP.NET Core rota işleyicileri için uygulama isteği yönlendirme için özellikleri sunar.</span><span class="sxs-lookup"><span data-stu-id="2904b-174">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="2904b-175">Daha fazla bilgi için bkz: [yönlendirme](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="2904b-175">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="2904b-176">Dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="2904b-176">File providers</span></span>

<span data-ttu-id="2904b-177">ASP.NET Core dosya sistemi erişimi platformlarında dosyalarıyla çalışmak için ortak bir arabirim sunar dosya sağlayıcıları kullanımıyla soyutlar.</span><span class="sxs-lookup"><span data-stu-id="2904b-177">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="2904b-178">Daha fazla bilgi için bkz: [dosya sağlayıcıları](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="2904b-178">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="2904b-179">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="2904b-179">Static files</span></span>

<span data-ttu-id="2904b-180">Statik dosya ara yazılımı HTML, CSS, görüntü ve JavaScript gibi statik dosyaları sunar.</span><span class="sxs-lookup"><span data-stu-id="2904b-180">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="2904b-181">Daha fazla bilgi için bkz: [statik dosyaları ile çalışma](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="2904b-181">For more information, see [Work with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="2904b-182">Barındırma</span><span class="sxs-lookup"><span data-stu-id="2904b-182">Hosting</span></span>

<span data-ttu-id="2904b-183">ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*, uygulama başlatma ve ömür boyu yönetimi için sorumlu olduğu.</span><span class="sxs-lookup"><span data-stu-id="2904b-183">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="2904b-184">Daha fazla bilgi için bkz: [barındırma](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="2904b-184">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="2904b-185">Oturum ve uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="2904b-185">Session and application state</span></span>

<span data-ttu-id="2904b-186">Oturum durumunu kaydetmek ve kullanıcı web uygulamanızı gözatar sırasında kullanıcı verilerini depolamak için kullanabileceğiniz ASP.NET Core bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="2904b-186">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="2904b-187">Daha fazla bilgi için bkz: [oturum ve uygulama durumu](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="2904b-187">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="2904b-188">Sunucular</span><span class="sxs-lookup"><span data-stu-id="2904b-188">Servers</span></span>

<span data-ttu-id="2904b-189">Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez.</span><span class="sxs-lookup"><span data-stu-id="2904b-189">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="2904b-190">Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="2904b-190">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="2904b-191">İletilen istek arabirimleri aracılığıyla erişilen özellik nesneleri kümesi olarak paketlenir.</span><span class="sxs-lookup"><span data-stu-id="2904b-191">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="2904b-192">ASP.NET Core içerir olarak adlandırılan bir yönetilen, platformlar arası web sunucusuna [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2904b-192">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="2904b-193">Kestrel sık çalıştırıldığında bir üretim web sunucusu gibi [IIS](https://www.iis.net/) veya [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="2904b-193">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="2904b-194">Kestrel bir uç sunucusu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2904b-194">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="2904b-195">Daha fazla bilgi için bkz: [sunucuları](xref:fundamentals/servers/index) ve aşağıdaki konular:</span><span class="sxs-lookup"><span data-stu-id="2904b-195">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="2904b-196">Kestrel</span><span class="sxs-lookup"><span data-stu-id="2904b-196">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="2904b-197">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="2904b-197">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="2904b-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="2904b-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="2904b-199">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="2904b-199">Globalization and localization</span></span>

<span data-ttu-id="2904b-200">ASP.NET Core ile çok dilli bir Web sitesi oluşturma, daha geniş bir kitleye ulaşmak sitenizin sağlar.</span><span class="sxs-lookup"><span data-stu-id="2904b-200">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="2904b-201">ASP.NET Core farklı dillere ve kültürlere yerelleştirme için hizmetleri ve ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="2904b-201">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="2904b-202">Daha fazla bilgi için bkz: [Genelleştirme ve Yerelleştirme](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="2904b-202">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="2904b-203">İstek özellikleri</span><span class="sxs-lookup"><span data-stu-id="2904b-203">Request features</span></span>

<span data-ttu-id="2904b-204">Web sunucusu uygulama ayrıntılarını ilgili HTTP istekleri ve yanıtları arabirimlerde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2904b-204">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="2904b-205">Bu arabirimleri oluşturmak ve uygulamanın barındırma ardışık düzen değiştirmek için sunucu uygulamaları ve ara yazılım tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2904b-205">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="2904b-206">Daha fazla bilgi için bkz: [istek özellikleri](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="2904b-206">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="2904b-207">Arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="2904b-207">Background tasks</span></span>

<span data-ttu-id="2904b-208">Arka plan görevleri olarak gerçekleştirilen *barındırılan hizmetlere*.</span><span class="sxs-lookup"><span data-stu-id="2904b-208">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="2904b-209">Barındırılan hizmet uygulayan arka plan görevi mantığı ile bir sınıftır [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="2904b-209">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="2904b-210">Daha fazla bilgi için bkz: [arka plan görevleri barındırılan hizmetleri ile](xref:fundamentals/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="2904b-210">For more information, see [Background tasks with hosted services](xref:fundamentals/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="2904b-211">.NET (OWIN) için açık Web arabirimi</span><span class="sxs-lookup"><span data-stu-id="2904b-211">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="2904b-212">ASP.NET Core açık Web arabirimi .NET (OWIN) destekler.</span><span class="sxs-lookup"><span data-stu-id="2904b-212">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="2904b-213">OWIN web uygulamalarının web sunucularından ayrılmış sağlar.</span><span class="sxs-lookup"><span data-stu-id="2904b-213">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="2904b-214">Daha fazla bilgi için bkz: [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="2904b-214">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="2904b-215">WebSockets</span><span class="sxs-lookup"><span data-stu-id="2904b-215">WebSockets</span></span>

<span data-ttu-id="2904b-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) kalıcı iki yönlü iletişim kanalları üzerinden TCP bağlantıları sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="2904b-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="2904b-217">Bir web uygulamasında gerçek zamanlı işlevselliği işlemleriniz herhangi bir yere ve sohbet, yürütebilmektedir gibi uygulamalar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2904b-217">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="2904b-218">ASP.NET Core web yuva özelliklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="2904b-218">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="2904b-219">Daha fazla bilgi için bkz: [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="2904b-219">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="2904b-220">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="2904b-220">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="2904b-221">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET Core metapackage içerir:</span><span class="sxs-lookup"><span data-stu-id="2904b-221">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="2904b-222">Tüm paketler ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2904b-222">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="2904b-223">Tüm paketleri Entity Framework Çekirdek tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2904b-223">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="2904b-224">ASP.NET Core ve Entity Framework Çekirdek tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="2904b-224">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="2904b-225">Daha fazla bilgi için bkz: [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="2904b-225">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="2904b-226">.NET core ve .NET Framework çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="2904b-226">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="2904b-227">Bir ASP.NET Core uygulama .NET Core veya .NET Framework çalışma zamanı hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2904b-227">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="2904b-228">Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="2904b-228">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="2904b-229">ASP.NET Core ve ASP.NET arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="2904b-229">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="2904b-230">ASP.NET Core ve ASP.NET arasında seçim yapma hakkında daha fazla bilgi için bkz: [ASP.NET Core ve ASP.NET arasında seçim yapma](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="2904b-230">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
