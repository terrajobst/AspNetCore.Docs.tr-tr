---
title: ASP.NET Core temelleri
author: rick-anderson
description: "ASP.NET Core uygulamaları oluşturmak için temel kavramları bulur."
keywords: "ASP.NET Core, temelleri, genel bakış"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83bed4676be3ca752442da3fe560f1f2a4d728a1
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="93abc-104">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="93abc-104">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="93abc-105">Bir web sunucusu oluşturan bir konsol uygulaması bir ASP.NET Core uygulamadır kendi `Main` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="93abc-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="93abc-106">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="93abc-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="93abc-107">`Main` Yöntemini çağırır `WebHost.CreateDefaultBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu düzenini izler.</span><span class="sxs-lookup"><span data-stu-id="93abc-107">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="93abc-108">Oluşturucu web sunucusu tanımlayan yöntemleri vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="93abc-108">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="93abc-109">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır.</span><span class="sxs-lookup"><span data-stu-id="93abc-109">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="93abc-110">ASP.NET Core'nın web ana bilgisayarı, IIS'de çalışan varsa çalışır.</span><span class="sxs-lookup"><span data-stu-id="93abc-110">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="93abc-111">Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun uzantı metodu çağırma tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93abc-111">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="93abc-112">`UseStartup`açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="93abc-112">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="93abc-113">`IWebHostBuilder`, dönüş türü `WebHost.CreateDefaultBuilder` çağırma, birçok isteğe bağlı yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="93abc-113">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="93abc-114">Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırma ve `UseContentRoot` kök içerik dizinini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="93abc-114">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="93abc-115">`Build` Ve `Run` yöntemleri yapı `IWebHost` uygulamayı barındıran ve HTTP isteklerini dinlemeye başlar nesnesi.</span><span class="sxs-lookup"><span data-stu-id="93abc-115">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="93abc-116">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="93abc-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="93abc-117">`Main` Yöntemi kullanan `WebHostBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu düzenini izler.</span><span class="sxs-lookup"><span data-stu-id="93abc-117">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="93abc-118">Oluşturucu web sunucusu tanımlayan yöntemleri vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="93abc-118">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="93abc-119">Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93abc-119">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="93abc-120">Diğer web sunucuları gibi [WebListener](xref:fundamentals/servers/weblistener), uygun uzantı metodu çağırma tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93abc-120">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="93abc-121">`UseStartup`açıklanan daha sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="93abc-121">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="93abc-122">`WebHostBuilder`dahil olmak üzere birçok isteğe bağlı yöntemler sağlar `UseIISIntegration` IIS ve IIS Express barındırmak için ve `UseContentRoot` kök içerik dizinini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="93abc-122">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="93abc-123">`Build` Ve `Run` yöntemleri yapı `IWebHost` uygulamayı barındıran ve HTTP isteklerini dinlemeye başlar nesnesi.</span><span class="sxs-lookup"><span data-stu-id="93abc-123">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="93abc-124">Başlangıç</span><span class="sxs-lookup"><span data-stu-id="93abc-124">Startup</span></span>

<span data-ttu-id="93abc-125">`UseStartup` Yöntemi `WebHostBuilder` belirtir `Startup` sınıfı, uygulamanız için:</span><span class="sxs-lookup"><span data-stu-id="93abc-125">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="93abc-126">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="93abc-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="93abc-127">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="93abc-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="93abc-128">`Startup` Sınıfı olduğundan istek işleme ardışık düzen tanımladığınız ve burada uygulama tarafından gerekli tüm hizmetler yapılandırılmadı.</span><span class="sxs-lookup"><span data-stu-id="93abc-128">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="93abc-129">`Startup` Sınıfı genel olmalı ve aşağıdaki yöntemlerden içermelidir:</span><span class="sxs-lookup"><span data-stu-id="93abc-129">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="93abc-130">`ConfigureServices`tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Çekirdek, kimlik) tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93abc-130">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="93abc-131">`Configure`tanımlar [ara yazılım](xref:fundamentals/middleware) isteği ardışık düzeni için.</span><span class="sxs-lookup"><span data-stu-id="93abc-131">`Configure` defines the [middleware](xref:fundamentals/middleware) for the request pipeline.</span></span>

<span data-ttu-id="93abc-132">Daha fazla bilgi için bkz: [uygulama başlangıç](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="93abc-132">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="93abc-133">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="93abc-133">Content root</span></span>

<span data-ttu-id="93abc-134">Taban yol görünümler gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfalarının](xref:mvc/razor-pages/index)ve statik varlıklar.</span><span class="sxs-lookup"><span data-stu-id="93abc-134">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="93abc-135">Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="93abc-135">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="93abc-136">Web kök</span><span class="sxs-lookup"><span data-stu-id="93abc-136">Web root</span></span>

<span data-ttu-id="93abc-137">Web uygulama CSS, JavaScript ve görüntü dosyaları gibi ortak, statik kaynakları içeren proje dizininde köküdür.</span><span class="sxs-lookup"><span data-stu-id="93abc-137">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="93abc-138">Bağımlılık ekleme (Hizmetleri)</span><span class="sxs-lookup"><span data-stu-id="93abc-138">Dependency Injection (Services)</span></span>

<span data-ttu-id="93abc-139">Bir hizmeti bir uygulama içinde ortak tüketim yönelik bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="93abc-139">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="93abc-140">Hizmetleri kullanılabilir hale getirilir aracılığıyla [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93abc-140">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="93abc-141">ASP.NET Core içeren yerel **ı**nİşlevin **o**f **C**destekleyen Tim (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="93abc-141">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="93abc-142">İsterseniz, varsayılan yerel kapsayıcı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93abc-142">You can replace the default native container if you wish.</span></span> <span data-ttu-id="93abc-143">Kendi gevşek bağlantı avantajı yanı sıra dı Hizmetleri uygulamanız genelinde kullanılabilir hale getirir (örneğin, [günlüğü](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="93abc-143">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="93abc-144">Daha fazla bilgi için bkz: [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93abc-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="93abc-145">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="93abc-145">Middleware</span></span>

<span data-ttu-id="93abc-146">ASP.NET çekirdek isteği kullanılarak ardışık düzeni oluşturma [Ara](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="93abc-146">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="93abc-147">ASP.NET Core Ara gerçekleştirir zaman uyumsuz mantığı bir `HttpContext` ve sırayla sonraki ara yazılımı çağırır veya istek doğrudan sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="93abc-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="93abc-148">"XYZ" adlı bir ara yazılım bileşeni çağırarak eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="93abc-148">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="93abc-149">ASP.NET Core zengin bir yerleşik ara yazılım kümesi ile birlikte gelir:</span><span class="sxs-lookup"><span data-stu-id="93abc-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="93abc-150">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="93abc-150">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="93abc-151">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="93abc-151">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="93abc-152">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="93abc-152">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="93abc-153">Yanıt sıkıştırma Ara</span><span class="sxs-lookup"><span data-stu-id="93abc-153">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="93abc-154">URL yeniden yazma işlemi Ara</span><span class="sxs-lookup"><span data-stu-id="93abc-154">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="93abc-155">[OWIN](http://owin.org)-tabanlı ara yazılım ASP.NET Core uygulamaları için kullanılabilir ve kendi özel ara yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93abc-155">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="93abc-156">Daha fazla bilgi için bkz: [ara yazılımı](xref:fundamentals/middleware) ve [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="93abc-156">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="93abc-157">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="93abc-157">Environments</span></span>

<span data-ttu-id="93abc-158">"Geliştirme" ve "Üretim" gibi ortamları ASP.NET Core, birinci sınıf bir kavram ve ortam değişkenlerini kullanma ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93abc-158">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="93abc-159">Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="93abc-159">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="93abc-160">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="93abc-160">Configuration</span></span>

<span data-ttu-id="93abc-161">ASP.NET Core üzerinde ad-değer çiftleri temel alan bir yapılandırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="93abc-161">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="93abc-162">Yapılandırma modeli bağlı değil `System.Configuration` veya *web.config*. Yapılandırma ayarları yapılandırma sağlayıcılarının bir sıralanmış kümesinden alır.</span><span class="sxs-lookup"><span data-stu-id="93abc-162">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="93abc-163">Yerleşik yapılandırma sağlayıcıları çeşitli dosya biçimleri (XML, JSON, INI) ve ortam tabanlı yapılandırmasını etkinleştirmek için ortam değişkenlerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="93abc-163">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="93abc-164">Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93abc-164">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="93abc-165">Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="93abc-165">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="93abc-166">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="93abc-166">Logging</span></span>

<span data-ttu-id="93abc-167">ASP.NET çekirdeği günlüğü sağlayıcıları çeşitli çalışır bir günlük API destekler.</span><span class="sxs-lookup"><span data-stu-id="93abc-167">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="93abc-168">Yerleşik sağlayıcılar, bir veya daha fazla hedeflere gönderen günlükleri destekler.</span><span class="sxs-lookup"><span data-stu-id="93abc-168">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="93abc-169">Üçüncü taraf günlük altyapıları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93abc-169">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="93abc-170">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="93abc-170">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="93abc-171">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="93abc-171">Error handling</span></span>

<span data-ttu-id="93abc-172">ASP.NET Core Geliştirici özel durum sayfasında, özel hata sayfaları, statik durum kod sayfaları ve başlangıç özel durum işleme gibi uygulamalar, hataları işlemek için yerleşik özellikler vardır.</span><span class="sxs-lookup"><span data-stu-id="93abc-172">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="93abc-173">Daha fazla bilgi için bkz: [işleme hatası](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="93abc-173">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="93abc-174">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="93abc-174">Routing</span></span>

<span data-ttu-id="93abc-175">ASP.NET Core rota işleyicileri için uygulama isteği yönlendirme için özellikleri sunar.</span><span class="sxs-lookup"><span data-stu-id="93abc-175">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="93abc-176">Daha fazla bilgi için bkz: [yönlendirme](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="93abc-176">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="93abc-177">Dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="93abc-177">File providers</span></span>

<span data-ttu-id="93abc-178">ASP.NET Core dosya sistemi erişimi platformlarında dosyalarıyla çalışmak için ortak bir arabirim sunar dosya sağlayıcıları kullanımıyla soyutlar.</span><span class="sxs-lookup"><span data-stu-id="93abc-178">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="93abc-179">Daha fazla bilgi için bkz: [dosya sağlayıcıları](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="93abc-179">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="93abc-180">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="93abc-180">Static files</span></span>

<span data-ttu-id="93abc-181">Statik dosya ara yazılımı HTML, CSS, görüntü ve JavaScript gibi statik dosyaları sunar.</span><span class="sxs-lookup"><span data-stu-id="93abc-181">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="93abc-182">Daha fazla bilgi için bkz: [statik dosyaları ile çalışma](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="93abc-182">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="93abc-183">Barındırma</span><span class="sxs-lookup"><span data-stu-id="93abc-183">Hosting</span></span>

<span data-ttu-id="93abc-184">ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*, uygulama başlatma ve ömür boyu yönetimi için sorumlu olduğu.</span><span class="sxs-lookup"><span data-stu-id="93abc-184">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="93abc-185">Daha fazla bilgi için bkz: [barındırma](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="93abc-185">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="93abc-186">Oturum ve uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="93abc-186">Session and application state</span></span>

<span data-ttu-id="93abc-187">Oturum durumunu kaydetmek ve kullanıcı web uygulamanızı gözatar sırasında kullanıcı verilerini depolamak için kullanabileceğiniz ASP.NET Core bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="93abc-187">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="93abc-188">Daha fazla bilgi için bkz: [oturum ve uygulama durumu](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="93abc-188">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="93abc-189">Sunucular</span><span class="sxs-lookup"><span data-stu-id="93abc-189">Servers</span></span>

<span data-ttu-id="93abc-190">Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez.</span><span class="sxs-lookup"><span data-stu-id="93abc-190">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="93abc-191">Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="93abc-191">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="93abc-192">İletilen istek arabirimleri aracılığıyla erişilen özellik nesneleri kümesi olarak paketlenir.</span><span class="sxs-lookup"><span data-stu-id="93abc-192">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="93abc-193">ASP.NET Core içerir olarak adlandırılan bir yönetilen, platformlar arası web sunucusuna [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="93abc-193">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="93abc-194">Kestrel sık çalıştırıldığında bir üretim web sunucusu gibi [IIS](https://www.iis.net/) veya [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="93abc-194">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span> <span data-ttu-id="93abc-195">Kestrel bir uç sunucusu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93abc-195">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="93abc-196">Daha fazla bilgi için bkz: [sunucuları](xref:fundamentals/servers/index) ve aşağıdaki konular:</span><span class="sxs-lookup"><span data-stu-id="93abc-196">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="93abc-197">Kestrel</span><span class="sxs-lookup"><span data-stu-id="93abc-197">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="93abc-198">ASP.NET çekirdeği Modülü</span><span class="sxs-lookup"><span data-stu-id="93abc-198">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="93abc-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="93abc-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="93abc-200">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="93abc-200">Globalization and localization</span></span>

<span data-ttu-id="93abc-201">ASP.NET Core ile çok dilli bir Web sitesi oluşturma, daha geniş bir kitleye ulaşmak sitenizin sağlar.</span><span class="sxs-lookup"><span data-stu-id="93abc-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="93abc-202">ASP.NET Core farklı dillere ve kültürlere yerelleştirme için hizmetleri ve ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="93abc-202">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="93abc-203">Daha fazla bilgi için bkz: [Genelleştirme ve Yerelleştirme](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="93abc-203">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="93abc-204">İstek özellikleri</span><span class="sxs-lookup"><span data-stu-id="93abc-204">Request features</span></span>

<span data-ttu-id="93abc-205">Web sunucusu uygulama ayrıntılarını ilgili HTTP istekleri ve yanıtları arabirimlerde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="93abc-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="93abc-206">Bu arabirimleri oluşturmak ve uygulamanın barındırma ardışık düzen değiştirmek için sunucu uygulamaları ve ara yazılım tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93abc-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="93abc-207">Daha fazla bilgi için bkz: [istek özellikleri](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="93abc-207">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="93abc-208">.NET (OWIN) için açık Web arabirimi</span><span class="sxs-lookup"><span data-stu-id="93abc-208">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="93abc-209">ASP.NET Core açık Web arabirimi .NET (OWIN) destekler.</span><span class="sxs-lookup"><span data-stu-id="93abc-209">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="93abc-210">OWIN web uygulamalarının web sunucularından ayrılmış sağlar.</span><span class="sxs-lookup"><span data-stu-id="93abc-210">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="93abc-211">Daha fazla bilgi için bkz: [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="93abc-211">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="93abc-212">WebSockets</span><span class="sxs-lookup"><span data-stu-id="93abc-212">WebSockets</span></span>

<span data-ttu-id="93abc-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) kalıcı iki yönlü iletişim kanalları üzerinden TCP bağlantıları sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="93abc-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="93abc-214">Bir web uygulamasında gerçek zamanlı işlevselliği işlemleriniz herhangi bir yere ve sohbet, yürütebilmektedir gibi uygulamalar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93abc-214">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="93abc-215">ASP.NET Core web yuva özelliklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="93abc-215">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="93abc-216">Daha fazla bilgi için bkz: [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="93abc-216">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="93abc-217">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="93abc-217">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="93abc-218">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET Core metapackage içerir:</span><span class="sxs-lookup"><span data-stu-id="93abc-218">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="93abc-219">Tüm paketler ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="93abc-219">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="93abc-220">Tüm paketleri Entity Framework Çekirdek tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="93abc-220">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="93abc-221">ASP.NET Core ve Entity Framework Çekirdek tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="93abc-221">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="93abc-222">Daha fazla bilgi için bkz: [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="93abc-222">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="93abc-223">.NET core ve .NET Framework çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="93abc-223">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="93abc-224">Bir ASP.NET Core uygulama .NET Core veya .NET Framework çalışma zamanı hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93abc-224">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="93abc-225">Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="93abc-225">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="93abc-226">ASP.NET Core ve ASP.NET arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="93abc-226">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="93abc-227">ASP.NET Core ve ASP.NET arasında seçim yapma hakkında daha fazla bilgi için bkz: [ASP.NET Core ve ASP.NET arasında seçim yapma](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="93abc-227">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
