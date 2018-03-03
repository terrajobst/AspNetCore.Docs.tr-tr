---
title: "ASP.NET ASP.NET Core 2.0 geçirme"
author: isaac2004
description: "ASP.NET Core 2.0 geçirme mevcut ASP.NET MVC veya Web API uygulamaları için yönergeler alırsınız."
manager: wpickett
ms.author: scaddie
ms.date: 08/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/index
ms.openlocfilehash: 877bcdbe5c5fb43e280800b2b843f2dfacb5bb45
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="6df04-103">ASP.NET ASP.NET Core 2.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="6df04-103">Migrating from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="6df04-104">Tarafından [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="6df04-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="6df04-105">Bu makalede, ASP.NET Core 2.0 geçirme ASP.NET uygulamaları için bir başvuru kılavuzu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="6df04-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6df04-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6df04-106">Prerequisites</span></span>

* <span data-ttu-id="6df04-107">[.NET core 2.0.0 SDK](https://dot.net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="6df04-107">[.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="6df04-108">Hedef Çerçeve</span><span class="sxs-lookup"><span data-stu-id="6df04-108">Target frameworks</span></span>
<span data-ttu-id="6df04-109">ASP.NET Core 2.0 projeleri geliştiriciler .NET Core, .NET Framework veya her ikisini hedefleme esnekliği sunar.</span><span class="sxs-lookup"><span data-stu-id="6df04-109">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="6df04-110">Bkz: [sunucu uygulamaları için .NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) hangi hedef Framework'ü en uygun olduğunu belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="6df04-110">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="6df04-111">.NET Framework hedeflerken projeleri tek tek NuGet paketlerini başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6df04-111">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="6df04-112">.NET Core hedefleme ASP.NET Core 2.0 sayesinde çok sayıda açık paket referanslarını ortadan kaldırmanıza olanak tanır [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="6df04-112">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="6df04-113">Yükleme `Microsoft.AspNetCore.All` projenizdeki metapackage:</span><span class="sxs-lookup"><span data-stu-id="6df04-113">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="6df04-114">Metapackage kullanıldığında, metapackage başvurulan hiç paket uygulamayla dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="6df04-114">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="6df04-115">Bunlar performansını artırmak için önceden derlenmiş ve bu varlıkları .NET çekirdeği çalışma zamanı deposu içerir.</span><span class="sxs-lookup"><span data-stu-id="6df04-115">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="6df04-116">Bkz: [ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x](xref:fundamentals/metapackage) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="6df04-116">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="6df04-117">Proje yapısı farklar</span><span class="sxs-lookup"><span data-stu-id="6df04-117">Project structure differences</span></span>
<span data-ttu-id="6df04-118">*.Csproj* dosya biçimi ASP.NET Core basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6df04-118">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="6df04-119">Bazı önemli değişiklikler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="6df04-119">Some notable changes include:</span></span>
- <span data-ttu-id="6df04-120">Açık dosyaları edilmesi bunları projenin bir parçası olarak kabul edilmesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6df04-120">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="6df04-121">Bu büyük takımlar temelinde çalışırken XML birleştirme çakışmaları riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="6df04-121">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="6df04-122">Hangi dosya okunabilirliğini artırır diğer projeler için GUID tabanlı başvuru vardır.</span><span class="sxs-lookup"><span data-stu-id="6df04-122">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="6df04-123">Visual Studio'da kaldırılması olmadan dosya düzenlenebilir:</span><span class="sxs-lookup"><span data-stu-id="6df04-123">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Visual Studio 2017 CSPROJ bağlam menüsü seçeneğini Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="6df04-125">Global.asax dosyası değiştirme</span><span class="sxs-lookup"><span data-stu-id="6df04-125">Global.asax file replacement</span></span>
<span data-ttu-id="6df04-126">ASP.NET Core uygulama Önyüklemesi için yeni bir mekanizma sunar.</span><span class="sxs-lookup"><span data-stu-id="6df04-126">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="6df04-127">ASP.NET uygulamaları için giriş noktası *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="6df04-127">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="6df04-128">Yol yapılandırması ve filtre ve alan kayıtlar gibi görevleri işlenir *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="6df04-128">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="6df04-129">Bu yaklaşım, uygulama ve sunucunun, bir uygulama ile uğratan şekilde dağıtıldığı couples.</span><span class="sxs-lookup"><span data-stu-id="6df04-129">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="6df04-130">Aynı şekilde çaba içinde [OWIN](http://owin.org/) birden çok çerçeveyi birlikte kullanmak için temiz bir şekilde sağlamak için sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6df04-130">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="6df04-131">OWIN yalnızca gerekli modülleri eklemek için bir kanal sağlar.</span><span class="sxs-lookup"><span data-stu-id="6df04-131">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="6df04-132">Barındırma ortamı geçen bir [başlangıç](xref:fundamentals/startup) Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırmak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="6df04-132">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="6df04-133">`Startup` Ara yazılım bir dizi uygulama ile kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6df04-133">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="6df04-134">Her istek için uygulama işleyicileri var olan bir dizi bağlantılı listesinin baş işaretçisi ile Ara yazılım bileşenlerinin her biri çağırır.</span><span class="sxs-lookup"><span data-stu-id="6df04-134">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="6df04-135">Her ara yazılım bileşeni ardışık düzen işleme isteği için bir veya daha fazla işleyicileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6df04-135">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="6df04-136">Bu listeye yeni başı olan işleyici başvuru döndürerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6df04-136">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="6df04-137">Her işleyici anımsama ve listedeki sonraki işleyicisi çağırma sorumludur.</span><span class="sxs-lookup"><span data-stu-id="6df04-137">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="6df04-138">ASP.NET Core ile bir uygulama için giriş noktasıdır `Startup`, ve bir bağımlılık artık sahip *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="6df04-138">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="6df04-139">OWIN .NET Framework ile kullanırken, aşağıdakine benzer bir ardışık düzen halinde kullanın:</span><span class="sxs-lookup"><span data-stu-id="6df04-139">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="6df04-140">Bu, varsayılan yolların yapılandırır ve XmlSerialization için Json üzerinde varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="6df04-140">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="6df04-141">(Hizmetleri, yapılandırma ayarlarını, statik dosyalar, vb. yükleniyor.) gerektiği gibi diğer ara yazılımdan bu ardışık düzene ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6df04-141">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="6df04-142">ASP.NET Core benzer bir yaklaşım kullanır, ancak giriş işlemek OWIN kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="6df04-142">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="6df04-143">Bunun yerine, aracılığıyla yapılır *Program.cs* `Main` yöntemi (konsol uygulamaları gibi) ve `Startup` orada yüklenir.</span><span class="sxs-lookup"><span data-stu-id="6df04-143">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="6df04-144">`Startup` içermelidir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6df04-144">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="6df04-145">İçinde `Configure`, gerekli ara yazılım ardışık düzene ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6df04-145">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="6df04-146">(Şablondan varsayılan web sitesi) aşağıdaki örnekte, birkaç uzantı yöntemleri için destek ile ardışık düzenini yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6df04-146">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="6df04-147">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="6df04-147">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="6df04-148">Hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="6df04-148">Error pages</span></span>
* <span data-ttu-id="6df04-149">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="6df04-149">Static files</span></span>
* <span data-ttu-id="6df04-150">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6df04-150">ASP.NET Core MVC</span></span>
* <span data-ttu-id="6df04-151">Kimlik</span><span class="sxs-lookup"><span data-stu-id="6df04-151">Identity</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="6df04-152">Ana bilgisayar ve uygulama, gelecekte farklı bir platform için taşıma esneklik sağlayan ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="6df04-152">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="6df04-153">**Not:** ASP.NET Core başlatma ve ara yazılımı için daha kapsamlı bir başvuru için bkz: [ASP.NET Core başlangıç](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="6df04-153">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="6df04-154">Depolama yapılandırmaları</span><span class="sxs-lookup"><span data-stu-id="6df04-154">Storing configurations</span></span>
<span data-ttu-id="6df04-155">ASP.NET depolama ayarlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="6df04-155">ASP.NET supports storing settings.</span></span> <span data-ttu-id="6df04-156">Bu ayar, örneğin, uygulamaları dağıtılıp dağıtılmadığını ortamını desteklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6df04-156">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="6df04-157">Tüm özel anahtar-değer çiftlerini depolamak için ortak bir uygulama kullanılmasıydır `<appSettings>` bölümünü *Web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6df04-157">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="6df04-158">Uygulamaları kullanarak bu ayarları okuma `ConfigurationManager.AppSettings` koleksiyonunda `System.Configuration` ad alanı:</span><span class="sxs-lookup"><span data-stu-id="6df04-158">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="6df04-159">ASP.NET Core, herhangi bir dosyada uygulama için yapılandırma verilerini depolamak ve bunları ara yazılım önyüklemesinden bir parçası olarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="6df04-159">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="6df04-160">Proje şablonlarını kullanılan varsayılan dosyası *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="6df04-160">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="6df04-161">Bu dosyayı bir örneğine yüklemesini `IConfiguration` uygulamanızı yapılır iç *haline*:</span><span class="sxs-lookup"><span data-stu-id="6df04-161">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="6df04-162">Uygulama okur `Configuration` ayarları almak için:</span><span class="sxs-lookup"><span data-stu-id="6df04-162">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="6df04-163">Bu yaklaşımın işlemi kullanarak gibi daha sağlam hale uzantıları vardır [bağımlılık ekleme](xref:fundamentals/dependency-injection) bu değerleri içeren bir hizmeti yüklemek için (dı).</span><span class="sxs-lookup"><span data-stu-id="6df04-163">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="6df04-164">DI yaklaşım yapılandırma nesnelerini kesin türü belirtilmiş bir dizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6df04-164">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="6df04-165">**Not:** ASP.NET Core yapılandırma daha kapsamlı bir başvuru için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6df04-165">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="6df04-166">Yerel bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="6df04-166">Native dependency injection</span></span>
<span data-ttu-id="6df04-167">Büyük, ölçeklendirilebilir uygulamalar oluştururken bir önemli bileşenleri ve hizmetlerin gevşek bağlantı hedeftir.</span><span class="sxs-lookup"><span data-stu-id="6df04-167">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="6df04-168">[Bağımlılık ekleme](xref:fundamentals/dependency-injection) bunu elde etmek için yaygın olarak kullanılan bir tekniktir ve ASP.NET Core yerel bir bileşeni olduğu.</span><span class="sxs-lookup"><span data-stu-id="6df04-168">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="6df04-169">ASP.NET uygulamalarında geliştiriciler bağımlılık ekleme uygulamak için bir üçüncü taraf kitaplığı kullanır.</span><span class="sxs-lookup"><span data-stu-id="6df04-169">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="6df04-170">Bu tür bir kitaplığıdır [Unity](https://github.com/unitycontainer/unity), Microsoft Patterns & yöntemler tarafından sağlanan.</span><span class="sxs-lookup"><span data-stu-id="6df04-170">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="6df04-171">Bir bağımlılık ekleme Unity ile ayarlama örneği uygulama `IDependencyResolver` , saran bir `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="6df04-171">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="6df04-172">Bir örneğini oluşturun, `UnityContainer`, hizmetiniz kaydetmek ve bağımlılık çözümleyicisini ayarlayın `HttpConfiguration` yeni örneğine `UnityResolver` , kapsayıcı için:</span><span class="sxs-lookup"><span data-stu-id="6df04-172">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="6df04-173">Eklenmeye `IProductRepository` gerektiğinde:</span><span class="sxs-lookup"><span data-stu-id="6df04-173">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="6df04-174">Bağımlılık ekleme ASP.NET Core parçası olduğundan, hizmetinizi ekleyebilirsiniz `ConfigureServices` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="6df04-174">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="6df04-175">Depo her yerden, Unity ile doğru şekilde yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="6df04-175">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="6df04-176">**Not:** ASP.NET Core bağımlılık ekleme bir ayrıntılı başvuru için bkz: [ASP.NET Core bağımlılık ekleme](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="6df04-176">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="6df04-177">Statik dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="6df04-177">Serving static files</span></span>
<span data-ttu-id="6df04-178">Önemli, bir web geliştirme statik, istemci-tarafı varlıklar sunabilme özelliğini parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="6df04-178">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="6df04-179">En yaygın statik dosyaları HTML, CSS, Javascript ve görüntüleri gösterilebilir.</span><span class="sxs-lookup"><span data-stu-id="6df04-179">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="6df04-180">Bu dosyalar veya uygulama (CDN) yayımlanan konumda kaydedilir ve bir istek tarafından yüklenebilmesi için başvurulan gerekir.</span><span class="sxs-lookup"><span data-stu-id="6df04-180">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="6df04-181">Bu işlem, ASP.NET Core değişti.</span><span class="sxs-lookup"><span data-stu-id="6df04-181">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="6df04-182">ASP.NET, statik dosyalar çeşitli dizinlerde depolanan ve görünümlerde başvurulan.</span><span class="sxs-lookup"><span data-stu-id="6df04-182">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="6df04-183">ASP.NET çekirdek statik dosyaları "web root" depolanır (*&lt;içerik kök&gt;/wwwroot*), aksi belirtilmedikçe.</span><span class="sxs-lookup"><span data-stu-id="6df04-183">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="6df04-184">Dosyalar istek ardışık düzenine çağırarak yüklenir `UseStaticFiles` uzantısı yönteminden `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6df04-184">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="6df04-185">**Not:** .NET Framework'ü hedefleme NuGet paketini yükleyin `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="6df04-185">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="6df04-186">Örneğin, bir görüntü varlığı *wwwroot/görüntüleri* klasördür erişilebilir bir konumda tarayıcıya gibi `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="6df04-186">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="6df04-187">**Not:** ASP.NET Core içinde statik dosyaları sunma daha ayrıntılı başvuru için bkz: [ASP.NET Core statik dosyaları ile çalışmaya giriş](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="6df04-187">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6df04-188">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6df04-188">Additional resources</span></span>

* [<span data-ttu-id="6df04-189">.NET Core kitaplıklara bağlantı noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="6df04-189">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
