---
title: ASP.NET'ten ASP.NET Core 2.0 geçirme
author: isaac2004
description: Mevcut ASP.NET MVC veya Web API uygulamalarını geçirme ASP.NET Core 2.0 için rehberlik alın.
ms.author: scaddie
ms.date: 08/27/2017
uid: migration/mvc2
ms.openlocfilehash: 42fbabb2fe5bd79a72cd220230faa9d75ff1c9d8
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538394"
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="87c12-103">ASP.NET'ten ASP.NET Core 2.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="87c12-103">Migrate from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="87c12-104">Tarafından [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="87c12-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="87c12-105">Bu makalede, ASP.NET Core 2.0 ASP.NET uygulamalarını geçirme için bir başvuru kılavuzu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="87c12-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87c12-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="87c12-106">Prerequisites</span></span>

<span data-ttu-id="87c12-107">Yükleme **bir** birini [.NET indirir: Windows](https://www.microsoft.com/net/download/windows):</span><span class="sxs-lookup"><span data-stu-id="87c12-107">Install **one** of the following from [.NET Downloads: Windows](https://www.microsoft.com/net/download/windows):</span></span>

* <span data-ttu-id="87c12-108">.NET core SDK'sı</span><span class="sxs-lookup"><span data-stu-id="87c12-108">.NET Core SDK</span></span>
* <span data-ttu-id="87c12-109">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87c12-109">Visual Studio for Windows</span></span>
  * <span data-ttu-id="87c12-110">**ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="87c12-110">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="87c12-111">**.NET core çoklu platform geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="87c12-111">**.NET Core cross-platform development** workload</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="87c12-112">Hedef Çerçeve</span><span class="sxs-lookup"><span data-stu-id="87c12-112">Target frameworks</span></span>
<span data-ttu-id="87c12-113">ASP.NET Core 2.0 projeleri geliştiricilerin, .NET Core, .NET Framework veya her ikisi de hedefleme esnekliği sunar.</span><span class="sxs-lookup"><span data-stu-id="87c12-113">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="87c12-114">Bkz: [sunucu uygulamaları için .NET Core ve .NET Framework arasında seçim](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) hangi hedef çerçeveden en uygun olduğunu belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="87c12-114">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="87c12-115">.NET Framework'ü hedefleyen, projeler tek NuGet paketlerini başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="87c12-115">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="87c12-116">.NET Core'u hedefleyen ASP.NET Core 2.0 sayesinde çok sayıda açık paket başvuruları ortadan olanak tanır [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="87c12-116">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="87c12-117">Yükleme `Microsoft.AspNetCore.All` metapackage projenizdeki:</span><span class="sxs-lookup"><span data-stu-id="87c12-117">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.1.4" />
</ItemGroup>
```

<span data-ttu-id="87c12-118">Metapackage kullanıldığında metapackage içinde başvurulan paket uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="87c12-118">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="87c12-119">.NET Core çalışma zamanı Store bu varlıkları içerir ve bunlar performansını artırmak için önceden derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="87c12-119">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="87c12-120">Bkz: [Microsoft.AspNetCore.All metapackage ASP.NET Core 2.x](xref:fundamentals/metapackage) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="87c12-120">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="87c12-121">Proje yapısına</span><span class="sxs-lookup"><span data-stu-id="87c12-121">Project structure differences</span></span>
<span data-ttu-id="87c12-122">*.Csproj* dosya biçimi, ASP.NET Core basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="87c12-122">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="87c12-123">Bazı önemli değişiklikler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="87c12-123">Some notable changes include:</span></span>
- <span data-ttu-id="87c12-124">Açık içerme dosyaları projenin bir parçası olarak değerlendirilmesi için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="87c12-124">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="87c12-125">Büyük takımlar üzerinde çalışırken bu XML birleştirme çakışmalarını riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="87c12-125">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="87c12-126">Hangi dosya okunabilirliği iyileştirilebileceği diğer projelere GUID tabanlı başvuru vardır.</span><span class="sxs-lookup"><span data-stu-id="87c12-126">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="87c12-127">Dosya, Visual Studio'da kaldırmadan düzenlenebilir:</span><span class="sxs-lookup"><span data-stu-id="87c12-127">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Visual Studio 2017'de CSPROJ bağlam menüsü seçeneği Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="87c12-129">Global.asax dosyası değiştirme</span><span class="sxs-lookup"><span data-stu-id="87c12-129">Global.asax file replacement</span></span>
<span data-ttu-id="87c12-130">ASP.NET Core önyükleme bir uygulama için yeni bir mekanizma sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="87c12-130">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="87c12-131">ASP.NET uygulamaları için giriş noktası *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="87c12-131">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="87c12-132">Yol yapılandırması ve filtre ve alan kayıtları gibi görevleri işlenir *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="87c12-132">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="87c12-133">Bu yaklaşım couples: uygulama ve sunucu, bir uygulama ile uğratan şekilde dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="87c12-133">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="87c12-134">İçinde ayırmak için çaba [OWIN](http://owin.org/) birden çok çerçeveyi birlikte kullanmak için temiz bir yol sağlamak üzere kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="87c12-134">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="87c12-135">OWIN yalnızca gerekli modül eklemek için bir işlem hattı sağlar.</span><span class="sxs-lookup"><span data-stu-id="87c12-135">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="87c12-136">Barındırma ortamı alır bir [başlangıç](xref:fundamentals/startup) hizmetler ve uygulamanın istek ardışık düzenini yapılandırmak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="87c12-136">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="87c12-137">`Startup` Ara yazılım bir dizi uygulama ile kaydeder.</span><span class="sxs-lookup"><span data-stu-id="87c12-137">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="87c12-138">Her istek için uygulamanın baş işleyicileri var olan bir dizi bağlı bir liste işaretçisi ile Ara yazılım bileşenlerinin her biri çağırır.</span><span class="sxs-lookup"><span data-stu-id="87c12-138">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="87c12-139">Her bir ara yazılım bileşeni ardışık düzen işleme isteği için bir veya daha fazla işleyicileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87c12-139">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="87c12-140">Bu, listenin başındaki yeni işleyici için bir başvuru döndürerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="87c12-140">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="87c12-141">Her işleyici anımsanmasını ve listedeki sonraki işleyicisini çağırarak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="87c12-141">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="87c12-142">ASP.NET Core ile bir uygulama için giriş noktasıdır `Startup`, ve bir bağımlılık artık sahip *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="87c12-142">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="87c12-143">OWIN .NET Framework ile kullanırken, şunun gibi bir işlem hattı kullanın:</span><span class="sxs-lookup"><span data-stu-id="87c12-143">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="87c12-144">Bu, varsayılan yollarını yapılandırır ve XmlSerialization için varsayılan olarak Json.</span><span class="sxs-lookup"><span data-stu-id="87c12-144">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="87c12-145">(Hizmetler, yapılandırma ayarlarını, statik dosyalar, vb. yükleniyor.) gerektiği gibi diğer ara yazılımdan bu ardışık düzenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="87c12-145">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="87c12-146">ASP.NET Core, benzer bir yaklaşım kullanır, ancak giriş işlemek için OWIN üzerinde içermez.</span><span class="sxs-lookup"><span data-stu-id="87c12-146">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="87c12-147">Bunun yerine, aracılığıyla gerçekleştirilir *Program.cs* `Main` yöntemi (konsol uygulamaları için benzer şekilde) ve `Startup` orada yüklenir.</span><span class="sxs-lookup"><span data-stu-id="87c12-147">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="87c12-148">`Startup` içermelidir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="87c12-148">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="87c12-149">İçinde `Configure`, gerekli bir ara yazılım ardışık düzenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="87c12-149">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="87c12-150">(Şablondan varsayılan web sitesi) aşağıdaki örnekte, birkaç genişletme yöntemleri için destek ile işlem hattını yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="87c12-150">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="87c12-151">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="87c12-151">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="87c12-152">Hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="87c12-152">Error pages</span></span>
* <span data-ttu-id="87c12-153">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="87c12-153">Static files</span></span>
* <span data-ttu-id="87c12-154">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="87c12-154">ASP.NET Core MVC</span></span>
* <span data-ttu-id="87c12-155">Kimlik</span><span class="sxs-lookup"><span data-stu-id="87c12-155">Identity</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="87c12-156">Konak ve uygulama, gelecekte farklı bir platform için taşıma esnekliği sağlayan ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="87c12-156">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="87c12-157">**Not:** ASP.NET Core başlangıç ve ara yazılım için daha ayrıntılı bir başvuru için bkz: [ASP.NET Core başlangıç](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="87c12-157">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="87c12-158">Depolama yapılandırmaları</span><span class="sxs-lookup"><span data-stu-id="87c12-158">Storing configurations</span></span>
<span data-ttu-id="87c12-159">ASP.NET depolama ayarlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="87c12-159">ASP.NET supports storing settings.</span></span> <span data-ttu-id="87c12-160">Bu ayar, örneğin, uygulama dağıtılıp dağıtılmadığını ortamı desteklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="87c12-160">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="87c12-161">Tüm özel anahtar-değer çiftlerini depolamak için ortak bir uygulama olan `<appSettings>` bölümünü *Web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="87c12-161">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="87c12-162">Uygulamaları kullanarak bu ayarları okuma `ConfigurationManager.AppSettings` koleksiyonda `System.Configuration` ad alanı:</span><span class="sxs-lookup"><span data-stu-id="87c12-162">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="87c12-163">ASP.NET Core uygulaması için yapılandırma verilerini herhangi bir dosyayı depolayabilir ve ara yazılımı önyüklemesi bir parçası olarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="87c12-163">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="87c12-164">Proje şablonlarında kullanılan varsayılan dosya *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="87c12-164">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="87c12-165">Bu dosya bir örneğine yüklenirken `IConfiguration` uygulamanızı yapılır iç *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="87c12-165">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="87c12-166">Uygulama okur `Configuration` ayarları almak için:</span><span class="sxs-lookup"><span data-stu-id="87c12-166">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="87c12-167">Uzantı işlemi kullanma gibi daha sağlam hale getirmek için bu yaklaşımın [bağımlılık ekleme](xref:fundamentals/dependency-injection) bu değerlere sahip bir hizmet yüklenemedi (dı).</span><span class="sxs-lookup"><span data-stu-id="87c12-167">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="87c12-168">Yapılandırma nesneleri türü kesin belirlenmiş bir dizi DI yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="87c12-168">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="87c12-169">**Not:** ASP.NET Core yapılandırma daha ayrıntılı bir başvuru için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="87c12-169">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="87c12-170">Yerel bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="87c12-170">Native dependency injection</span></span>

<span data-ttu-id="87c12-171">Büyük, ölçeklenebilir uygulamalar oluştururken bir önemli bileşenlerin ve hizmetlerin gevşek eşleştirme hedeftir.</span><span class="sxs-lookup"><span data-stu-id="87c12-171">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="87c12-172">[Bağımlılık ekleme](xref:fundamentals/dependency-injection) bunu elde etmek için yaygın olarak kullanılan bir tekniktir ve onu bir ASP.NET Core, yerel bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="87c12-172">[Dependency injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="87c12-173">ASP.NET uygulamalarında bağımlılık ekleme uygulamak için bir üçüncü taraf kitaplığı geliştiriciler kullanır.</span><span class="sxs-lookup"><span data-stu-id="87c12-173">In ASP.NET applications, developers rely on a third-party library to implement dependency injection.</span></span> <span data-ttu-id="87c12-174">Bir tür kitaplığı [Unity](https://github.com/unitycontainer/unity), Microsoft Patterns ve uygulamalar tarafından sağlanan.</span><span class="sxs-lookup"><span data-stu-id="87c12-174">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="87c12-175">Unity ile bağımlılık ekleme ayarlama örneği uygulama `IDependencyResolver` sonuna geldik bir `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="87c12-175">An example of setting up dependency injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="87c12-176">Öğesinin bir örneğini oluşturur, `UnityContainer`, hizmetinizi kaydedin ve bağımlılık çözümleyicisini ayarlamak `HttpConfiguration` için yeni bir örneğini `UnityResolver` kapsayıcınız için:</span><span class="sxs-lookup"><span data-stu-id="87c12-176">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="87c12-177">Ekleme `IProductRepository` gerektiğinde:</span><span class="sxs-lookup"><span data-stu-id="87c12-177">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="87c12-178">Bağımlılık ekleme ASP.NET Core parçası olduğundan, hizmetinizde ekleyebilirsiniz `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="87c12-178">Because dependency injection is part of ASP.NET Core, you can add your service in the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="87c12-179">Depoya her yerden, Unity ile doğru şekilde yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="87c12-179">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="87c12-180">ASP.NET core'da bağımlılık ekleme hakkında daha fazla bilgi için bkz. [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="87c12-180">For more information on dependency injection in ASP.NET Core, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="87c12-181">Statik dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="87c12-181">Serving static files</span></span>

<span data-ttu-id="87c12-182">Web geliştirme önemli bir parçası, statik, istemci tarafı varlıklar sunabilme özelliğini ' dir.</span><span class="sxs-lookup"><span data-stu-id="87c12-182">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="87c12-183">En yaygın statik dosyalar HTML, CSS, Javascript ve görüntüleri örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="87c12-183">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="87c12-184">Bu dosyaları ve uygulama (CDN) yayımlanan konumda kaydedilebilir ve bir istek tarafından yüklenebilmesi için başvurulan gerekir.</span><span class="sxs-lookup"><span data-stu-id="87c12-184">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="87c12-185">Bu işlem, ASP.NET Core değişti.</span><span class="sxs-lookup"><span data-stu-id="87c12-185">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="87c12-186">ASP.NET'te, statik dosyalar çeşitli dizinlerinde depolanan ve görünümleri başvuru.</span><span class="sxs-lookup"><span data-stu-id="87c12-186">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="87c12-187">ASP.NET Core, statik dosyalar "web root" depolanır (*&lt;içerik kök&gt;/wwwroot*), aksi şekilde yapılandırılmadıkça.</span><span class="sxs-lookup"><span data-stu-id="87c12-187">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="87c12-188">İstek ardışık düzende çağırarak dosyalar yüklenir `UseStaticFiles` şuradan genişleme metodu `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="87c12-188">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="87c12-189">**Not:** .NET Framework'ü hedefleyen, NuGet paketini yüklemek `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="87c12-189">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="87c12-190">Örneğin, bir görüntü varlığı *wwwroot/görüntülerinden* klasörünün erişilebilir bir konumda tarayıcıya gibi `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="87c12-190">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="87c12-191">**Not:** ASP.NET core'da statik dosyaları sunma daha ayrıntılı başvuru için bkz: [statik dosyalar](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="87c12-191">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87c12-192">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="87c12-192">Additional resources</span></span>

* [<span data-ttu-id="87c12-193">.NET Core kitaplıklarını taşıma</span><span class="sxs-lookup"><span data-stu-id="87c12-193">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
