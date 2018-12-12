---
title: ASP.NET'ten ASP.NET Core geçişi
author: isaac2004
description: ASP.NET Core.web için geçirme mevcut ASP.NET MVC veya Web API uygulamaları için yol gösteren yönergeler alır
ms.author: scaddie
ms.date: 12/11/2018
uid: migration/proper-to-2x/index
ms.openlocfilehash: a9eef832a68afa1a73e3c7c545378da190602ce2
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284402"
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a><span data-ttu-id="b5179-103">ASP.NET'ten ASP.NET Core geçişi</span><span class="sxs-lookup"><span data-stu-id="b5179-103">Migrate from ASP.NET to ASP.NET Core</span></span>

<span data-ttu-id="b5179-104">Tarafından [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="b5179-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="b5179-105">Bu makalede, ASP.NET Core geçirme ASP.NET uygulamaları için bir başvuru kılavuzu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="b5179-105">This article serves as a reference guide for migrating ASP.NET apps to ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5179-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b5179-106">Prerequisites</span></span>

[<span data-ttu-id="b5179-107">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="b5179-107">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download)

## <a name="target-frameworks"></a><span data-ttu-id="b5179-108">Hedef Çerçeve</span><span class="sxs-lookup"><span data-stu-id="b5179-108">Target frameworks</span></span>

<span data-ttu-id="b5179-109">ASP.NET Core projeleri, geliştiricilerin .NET Core, .NET Framework veya her ikisi de hedefleme esnekliği sunar.</span><span class="sxs-lookup"><span data-stu-id="b5179-109">ASP.NET Core projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="b5179-110">Bkz: [sunucu uygulamaları için .NET Core ve .NET Framework arasında seçim](/dotnet/standard/choosing-core-framework-server) hangi hedef çerçeveden en uygun olduğunu belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="b5179-110">See [Choosing between .NET Core and .NET Framework for server apps](/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="b5179-111">.NET Framework'ü hedefleyen, projeler tek NuGet paketlerini başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b5179-111">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="b5179-112">.NET Core'u hedefleyen ASP.NET Core sayesinde çok sayıda açık paket başvuruları ortadan olanak tanır [metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b5179-112">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core [metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b5179-113">Yükleme `Microsoft.AspNetCore.App` metapackage projenizdeki:</span><span class="sxs-lookup"><span data-stu-id="b5179-113">Install the `Microsoft.AspNetCore.App` metapackage in your project:</span></span>

```xml
<ItemGroup>
   <PackageReference Include="Microsoft.AspNetCore.App" />
</ItemGroup>
```

<span data-ttu-id="b5179-114">Metapackage kullanıldığında metapackage içinde başvurulan paket uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="b5179-114">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="b5179-115">.NET Core çalışma zamanı Store bu varlıkları içerir ve bunlar performansını artırmak için önceden derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="b5179-115">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="b5179-116">Bkz: [Microsoft.AspNetCore.App metapackage ASP.NET Core](xref:fundamentals/metapackage-app) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="b5179-116">See [Microsoft.AspNetCore.App metapackage for ASP.NET Core](xref:fundamentals/metapackage-app) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="b5179-117">Proje yapısına</span><span class="sxs-lookup"><span data-stu-id="b5179-117">Project structure differences</span></span>

<span data-ttu-id="b5179-118">*.Csproj* dosya biçimi, ASP.NET Core basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b5179-118">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="b5179-119">Bazı önemli değişiklikler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="b5179-119">Some notable changes include:</span></span>

- <span data-ttu-id="b5179-120">Açık içerme dosyaları projenin bir parçası olarak değerlendirilmesi için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b5179-120">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="b5179-121">Büyük takımlar üzerinde çalışırken bu XML birleştirme çakışmalarını riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="b5179-121">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="b5179-122">Hangi dosya okunabilirliği iyileştirilebileceği diğer projelere GUID tabanlı başvuru vardır.</span><span class="sxs-lookup"><span data-stu-id="b5179-122">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="b5179-123">Dosya, Visual Studio'da kaldırmadan düzenlenebilir:</span><span class="sxs-lookup"><span data-stu-id="b5179-123">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Visual Studio 2017'de CSPROJ bağlam menüsü seçeneği Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="b5179-125">Global.asax dosyası değiştirme</span><span class="sxs-lookup"><span data-stu-id="b5179-125">Global.asax file replacement</span></span>

<span data-ttu-id="b5179-126">ASP.NET Core önyükleme bir uygulama için yeni bir mekanizma sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="b5179-126">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="b5179-127">ASP.NET uygulamaları için giriş noktası *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="b5179-127">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="b5179-128">Yol yapılandırması ve filtre ve alan kayıtları gibi görevleri işlenir *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="b5179-128">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="b5179-129">Bu yaklaşım couples: uygulama ve sunucu, bir uygulama ile uğratan şekilde dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="b5179-129">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="b5179-130">İçinde ayırmak için çaba [OWIN](http://owin.org/) birden çok çerçeveyi birlikte kullanmak için temiz bir yol sağlamak üzere kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b5179-130">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="b5179-131">OWIN yalnızca gerekli modül eklemek için bir işlem hattı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b5179-131">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="b5179-132">Barındırma ortamı alır bir [başlangıç](xref:fundamentals/startup) hizmetler ve uygulamanın istek ardışık düzenini yapılandırmak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="b5179-132">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="b5179-133">`Startup` Ara yazılım bir dizi uygulama ile kaydeder.</span><span class="sxs-lookup"><span data-stu-id="b5179-133">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="b5179-134">Her istek için uygulamanın baş işleyicileri var olan bir dizi bağlı bir liste işaretçisi ile Ara yazılım bileşenlerinin her biri çağırır.</span><span class="sxs-lookup"><span data-stu-id="b5179-134">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="b5179-135">Her bir ara yazılım bileşeni ardışık düzen işleme isteği için bir veya daha fazla işleyicileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b5179-135">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="b5179-136">Bu, listenin başındaki yeni işleyici için bir başvuru döndürerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b5179-136">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="b5179-137">Her işleyici anımsanmasını ve listedeki sonraki işleyicisini çağırarak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b5179-137">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="b5179-138">ASP.NET Core ile bir uygulama için giriş noktasıdır `Startup`, ve bir bağımlılık artık sahip *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="b5179-138">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="b5179-139">OWIN .NET Framework ile kullanırken, şunun gibi bir işlem hattı kullanın:</span><span class="sxs-lookup"><span data-stu-id="b5179-139">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="b5179-140">Bu, varsayılan yollarını yapılandırır ve XmlSerialization için varsayılan olarak Json.</span><span class="sxs-lookup"><span data-stu-id="b5179-140">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="b5179-141">(Hizmetler, yapılandırma ayarlarını, statik dosyalar, vb. yükleniyor.) gerektiği gibi diğer ara yazılımdan bu ardışık düzenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b5179-141">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="b5179-142">ASP.NET Core, benzer bir yaklaşım kullanır, ancak giriş işlemek için OWIN üzerinde içermez.</span><span class="sxs-lookup"><span data-stu-id="b5179-142">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="b5179-143">Bunun yerine, aracılığıyla gerçekleştirilir *Program.cs* `Main` yöntemi (konsol uygulamaları için benzer şekilde) ve `Startup` orada yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b5179-143">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="b5179-144">`Startup` içermelidir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5179-144">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="b5179-145">İçinde `Configure`, gerekli bir ara yazılım ardışık düzenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b5179-145">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="b5179-146">(Şablondan varsayılan web sitesi) aşağıdaki örnekte, genişletme yöntemleri için destek ile işlem hattı yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="b5179-146">In the following example (from the default web site template), extension methods configure the pipeline with support for:</span></span>

* <span data-ttu-id="b5179-147">Hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="b5179-147">Error pages</span></span>
* <span data-ttu-id="b5179-148">HTTP katı aktarım güvenliği</span><span class="sxs-lookup"><span data-stu-id="b5179-148">HTTP Strict Transport Security</span></span>
* <span data-ttu-id="b5179-149">HTTPS için HTTP yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="b5179-149">HTTP redirection to HTTPS</span></span>
* <span data-ttu-id="b5179-150">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b5179-150">ASP.NET Core MVC</span></span>

[!code-csharp[](samples/startup.cs)]

<span data-ttu-id="b5179-151">Konak ve uygulama, gelecekte farklı bir platform için taşıma esnekliği sağlayan ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="b5179-151">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="b5179-152">ASP.NET Core başlangıç ve ara yazılım için daha ayrıntılı bir başvuru için bkz: [ASP.NET Core başlangıç](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="b5179-152">For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="store-configurations"></a><span data-ttu-id="b5179-153">Store yapılandırmaları</span><span class="sxs-lookup"><span data-stu-id="b5179-153">Store configurations</span></span>

<span data-ttu-id="b5179-154">ASP.NET depolama ayarlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="b5179-154">ASP.NET supports storing settings.</span></span> <span data-ttu-id="b5179-155">Bu ayar, örneğin, uygulama dağıtılıp dağıtılmadığını ortamı desteklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b5179-155">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="b5179-156">Tüm özel anahtar-değer çiftlerini depolamak için ortak bir uygulama olan `<appSettings>` bölümünü *Web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b5179-156">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="b5179-157">Uygulamaları kullanarak bu ayarları okuma `ConfigurationManager.AppSettings` koleksiyonda `System.Configuration` ad alanı:</span><span class="sxs-lookup"><span data-stu-id="b5179-157">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="b5179-158">ASP.NET Core uygulaması için yapılandırma verilerini herhangi bir dosyayı depolayabilir ve ara yazılımı önyüklemesi bir parçası olarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="b5179-158">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="b5179-159">Proje şablonlarında kullanılan varsayılan dosya *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b5179-159">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="b5179-160">Bu dosya bir örneğine yüklenirken `IConfiguration` uygulamanızı yapılır iç *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b5179-160">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="b5179-161">Uygulama okur `Configuration` ayarları almak için:</span><span class="sxs-lookup"><span data-stu-id="b5179-161">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="b5179-162">Uzantı işlemi kullanma gibi daha sağlam hale getirmek için bu yaklaşımın [bağımlılık ekleme](xref:fundamentals/dependency-injection) bu değerlere sahip bir hizmet yüklenemedi (dı).</span><span class="sxs-lookup"><span data-stu-id="b5179-162">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="b5179-163">Yapılandırma nesneleri türü kesin belirlenmiş bir dizi DI yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="b5179-163">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

> [!NOTE]
> <span data-ttu-id="b5179-164">ASP.NET Core yapılandırma daha ayrıntılı bir başvuru için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b5179-164">For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="b5179-165">Yerel bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="b5179-165">Native dependency injection</span></span>

<span data-ttu-id="b5179-166">Büyük, ölçeklenebilir uygulamalar oluştururken bir önemli bileşenlerin ve hizmetlerin gevşek eşleştirme hedeftir.</span><span class="sxs-lookup"><span data-stu-id="b5179-166">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="b5179-167">[Bağımlılık ekleme](xref:fundamentals/dependency-injection) bunu elde etmek için yaygın olarak kullanılan bir tekniktir ve onu bir ASP.NET Core, yerel bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="b5179-167">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="b5179-168">ASP.NET uygulamalarında bağımlılık ekleme uygulamak için bir üçüncü taraf kitaplığı geliştiriciler kullanır.</span><span class="sxs-lookup"><span data-stu-id="b5179-168">In ASP.NET apps, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="b5179-169">Bir tür kitaplığı [Unity](https://github.com/unitycontainer/unity), Microsoft Patterns ve uygulamalar tarafından sağlanan.</span><span class="sxs-lookup"><span data-stu-id="b5179-169">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="b5179-170">Unity ile bağımlılık ekleme ayarlama örneği uygulama `IDependencyResolver` sonuna geldik bir `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="b5179-170">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="b5179-171">Öğesinin bir örneğini oluşturur, `UnityContainer`, hizmetinizi kaydedin ve bağımlılık çözümleyicisini ayarlamak `HttpConfiguration` için yeni bir örneğini `UnityResolver` kapsayıcınız için:</span><span class="sxs-lookup"><span data-stu-id="b5179-171">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="b5179-172">Ekleme `IProductRepository` gerektiğinde:</span><span class="sxs-lookup"><span data-stu-id="b5179-172">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="b5179-173">Bağımlılık ekleme ASP.NET Core parçası olduğundan, hizmetinizde ekleyebilirsiniz `ConfigureServices` yöntemi *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b5179-173">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="b5179-174">Depoya her yerden, Unity ile doğru şekilde yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b5179-174">The repository can be injected anywhere, as was true with Unity.</span></span>

> [!NOTE]
> <span data-ttu-id="b5179-175">Bağımlılık ekleme hakkında daha fazla bilgi için bkz. [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b5179-175">For more information on dependency injection, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="b5179-176">Statik dosyaları işleme</span><span class="sxs-lookup"><span data-stu-id="b5179-176">Serve static files</span></span>

<span data-ttu-id="b5179-177">Web geliştirme önemli bir parçası, statik, istemci tarafı varlıklar sunabilme özelliğini ' dir.</span><span class="sxs-lookup"><span data-stu-id="b5179-177">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="b5179-178">En yaygın statik dosyalar HTML, CSS, Javascript ve görüntüleri örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="b5179-178">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="b5179-179">Bu dosyaları ve uygulama (CDN) yayımlanan konumda kaydedilebilir ve bir istek tarafından yüklenebilmesi için başvurulan gerekir.</span><span class="sxs-lookup"><span data-stu-id="b5179-179">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="b5179-180">Bu işlem, ASP.NET Core değişti.</span><span class="sxs-lookup"><span data-stu-id="b5179-180">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="b5179-181">ASP.NET'te, statik dosyalar çeşitli dizinlerinde depolanan ve görünümleri başvuru.</span><span class="sxs-lookup"><span data-stu-id="b5179-181">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="b5179-182">ASP.NET Core, statik dosyalar "web root" depolanır (*&lt;içerik kök&gt;/wwwroot*), aksi şekilde yapılandırılmadıkça.</span><span class="sxs-lookup"><span data-stu-id="b5179-182">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="b5179-183">İstek ardışık düzende çağırarak dosyalar yüklenir `UseStaticFiles` şuradan genişleme metodu `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b5179-183">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> <span data-ttu-id="b5179-184">.NET Framework'ü hedefleyen, NuGet paketini yüklemek `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="b5179-184">If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="b5179-185">Örneğin, bir görüntü varlığı *wwwroot/görüntülerinden* klasörünün erişilebilir bir konumda tarayıcıya gibi `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="b5179-185">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

> [!NOTE]
> <span data-ttu-id="b5179-186">ASP.NET core'da statik dosyaları sunma daha ayrıntılı başvuru için bkz: [statik dosyalar](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b5179-186">For a more in-depth reference to serving static files in ASP.NET Core, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5179-187">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b5179-187">Additional resources</span></span>

- [<span data-ttu-id="b5179-188">.NET Core kitaplıklarını taşıma</span><span class="sxs-lookup"><span data-stu-id="b5179-188">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
