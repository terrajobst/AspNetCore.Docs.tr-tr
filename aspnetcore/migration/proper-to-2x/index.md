---
title: ASP.NET ASP.NET Core geçirme
author: isaac2004
description: Yönergeler için ASP.NET Core.web geçirme mevcut ASP.NET MVC veya Web API uygulamaları için alma
ms.author: scaddie
ms.date: 08/27/2017
uid: migration/proper-to-2x/index
ms.openlocfilehash: 285f252767e4eacbb6a9ccc50bd98d1916e26bd4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273514"
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a><span data-ttu-id="43509-103">ASP.NET ASP.NET Core geçirme</span><span class="sxs-lookup"><span data-stu-id="43509-103">Migrate from ASP.NET to ASP.NET Core</span></span>

<span data-ttu-id="43509-104">Tarafından [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="43509-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="43509-105">Bu makalede, bir başvuru kılavuzu geçirme ASP.NET uygulamaları için ASP.NET Core olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="43509-105">This article serves as a reference guide for migrating ASP.NET apps to ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43509-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="43509-106">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

## <a name="target-frameworks"></a><span data-ttu-id="43509-107">Hedef Çerçeve</span><span class="sxs-lookup"><span data-stu-id="43509-107">Target frameworks</span></span>

<span data-ttu-id="43509-108">ASP.NET Core projeleri geliştiriciler .NET Core, .NET Framework veya her ikisini hedefleme esnekliği sunar.</span><span class="sxs-lookup"><span data-stu-id="43509-108">ASP.NET Core projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="43509-109">Bkz: [sunucu uygulamaları için .NET Core ve .NET Framework arasında seçim yapma](/dotnet/standard/choosing-core-framework-server) hangi hedef Framework'ü en uygun olduğunu belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="43509-109">See [Choosing between .NET Core and .NET Framework for server apps](/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="43509-110">.NET Framework hedeflerken projeleri tek tek NuGet paketlerini başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="43509-110">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="43509-111">.NET Core hedefleme ASP.NET Core sayesinde çok sayıda açık paket referanslarını ortadan kaldırmanıza olanak tanır [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="43509-111">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="43509-112">Yükleme `Microsoft.AspNetCore.All` projenizdeki metapackage:</span><span class="sxs-lookup"><span data-stu-id="43509-112">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="43509-113">Metapackage kullanıldığında, metapackage başvurulan hiç paket uygulamayla dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="43509-113">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="43509-114">Bunlar performansını artırmak için önceden derlenmiş ve bu varlıkları .NET çekirdeği çalışma zamanı deposu içerir.</span><span class="sxs-lookup"><span data-stu-id="43509-114">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="43509-115">Bkz: [ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x](xref:fundamentals/metapackage) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="43509-115">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="43509-116">Proje yapısı farklar</span><span class="sxs-lookup"><span data-stu-id="43509-116">Project structure differences</span></span>

<span data-ttu-id="43509-117">*.Csproj* dosya biçimi ASP.NET Core basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="43509-117">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="43509-118">Bazı önemli değişiklikler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="43509-118">Some notable changes include:</span></span>

- <span data-ttu-id="43509-119">Açık dosyaları edilmesi bunları projenin bir parçası olarak kabul edilmesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="43509-119">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="43509-120">Bu büyük takımlar temelinde çalışırken XML birleştirme çakışmaları riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="43509-120">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="43509-121">Hangi dosya okunabilirliğini artırır diğer projeler için GUID tabanlı başvuru vardır.</span><span class="sxs-lookup"><span data-stu-id="43509-121">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="43509-122">Visual Studio'da kaldırılması olmadan dosya düzenlenebilir:</span><span class="sxs-lookup"><span data-stu-id="43509-122">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Visual Studio 2017 CSPROJ bağlam menüsü seçeneğini Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="43509-124">Global.asax dosyası değiştirme</span><span class="sxs-lookup"><span data-stu-id="43509-124">Global.asax file replacement</span></span>

<span data-ttu-id="43509-125">ASP.NET Core uygulama Önyüklemesi için yeni bir mekanizma sunar.</span><span class="sxs-lookup"><span data-stu-id="43509-125">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="43509-126">ASP.NET uygulamaları için giriş noktası *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="43509-126">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="43509-127">Yol yapılandırması ve filtre ve alan kayıtlar gibi görevleri işlenir *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="43509-127">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="43509-128">Bu yaklaşım, uygulama ve sunucunun, bir uygulama ile uğratan şekilde dağıtıldığı couples.</span><span class="sxs-lookup"><span data-stu-id="43509-128">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="43509-129">Aynı şekilde çaba içinde [OWIN](http://owin.org/) birden çok çerçeveyi birlikte kullanmak için temiz bir şekilde sağlamak için sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="43509-129">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="43509-130">OWIN yalnızca gerekli modülleri eklemek için bir kanal sağlar.</span><span class="sxs-lookup"><span data-stu-id="43509-130">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="43509-131">Barındırma ortamı geçen bir [başlangıç](xref:fundamentals/startup) Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırmak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="43509-131">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="43509-132">`Startup` Ara yazılım bir dizi uygulama ile kaydeder.</span><span class="sxs-lookup"><span data-stu-id="43509-132">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="43509-133">Her istek için uygulama işleyicileri var olan bir dizi bağlantılı listesinin baş işaretçisi ile Ara yazılım bileşenlerinin her biri çağırır.</span><span class="sxs-lookup"><span data-stu-id="43509-133">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="43509-134">Her ara yazılım bileşeni ardışık düzen işleme isteği için bir veya daha fazla işleyicileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43509-134">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="43509-135">Bu listeye yeni başı olan işleyici başvuru döndürerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="43509-135">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="43509-136">Her işleyici anımsama ve listedeki sonraki işleyicisi çağırma sorumludur.</span><span class="sxs-lookup"><span data-stu-id="43509-136">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="43509-137">ASP.NET Core ile bir uygulama için giriş noktasıdır `Startup`, ve bir bağımlılık artık sahip *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="43509-137">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="43509-138">OWIN .NET Framework ile kullanırken, aşağıdakine benzer bir ardışık düzen halinde kullanın:</span><span class="sxs-lookup"><span data-stu-id="43509-138">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="43509-139">Bu, varsayılan yolların yapılandırır ve XmlSerialization için Json üzerinde varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="43509-139">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="43509-140">(Hizmetleri, yapılandırma ayarlarını, statik dosyalar, vb. yükleniyor.) gerektiği gibi diğer ara yazılımdan bu ardışık düzene ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43509-140">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="43509-141">ASP.NET Core benzer bir yaklaşım kullanır, ancak giriş işlemek OWIN kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="43509-141">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="43509-142">Bunun yerine, aracılığıyla yapılır *Program.cs* `Main` yöntemi (konsol uygulamaları gibi) ve `Startup` orada yüklenir.</span><span class="sxs-lookup"><span data-stu-id="43509-142">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="43509-143">`Startup` içermelidir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43509-143">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="43509-144">İçinde `Configure`, gerekli ara yazılım ardışık düzene ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43509-144">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="43509-145">(Şablondan varsayılan web sitesi) aşağıdaki örnekte, birkaç uzantı yöntemleri için destek ile ardışık düzenini yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="43509-145">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="43509-146">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="43509-146">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="43509-147">Hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="43509-147">Error pages</span></span>
* <span data-ttu-id="43509-148">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="43509-148">Static files</span></span>
* <span data-ttu-id="43509-149">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="43509-149">ASP.NET Core MVC</span></span>
* <span data-ttu-id="43509-150">Kimlik</span><span class="sxs-lookup"><span data-stu-id="43509-150">Identity</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="43509-151">Ana bilgisayar ve uygulama, gelecekte farklı bir platform için taşıma esneklik sağlayan ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="43509-151">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="43509-152">ASP.NET çekirdeği başlangıç ve ara yazılımı için daha kapsamlı bir başvuru için bkz: [ASP.NET Core başlangıç](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="43509-152">For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="store-configurations"></a><span data-ttu-id="43509-153">Depolama yapılandırmaları</span><span class="sxs-lookup"><span data-stu-id="43509-153">Store configurations</span></span>

<span data-ttu-id="43509-154">ASP.NET depolama ayarlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="43509-154">ASP.NET supports storing settings.</span></span> <span data-ttu-id="43509-155">Bu ayar, örneğin, uygulamaları dağıtılıp dağıtılmadığını ortamını desteklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43509-155">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="43509-156">Tüm özel anahtar-değer çiftlerini depolamak için ortak bir uygulama kullanılmasıydır `<appSettings>` bölümünü *Web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="43509-156">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="43509-157">Uygulamaları kullanarak bu ayarları okuma `ConfigurationManager.AppSettings` koleksiyonunda `System.Configuration` ad alanı:</span><span class="sxs-lookup"><span data-stu-id="43509-157">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="43509-158">ASP.NET Core, herhangi bir dosyada uygulama için yapılandırma verilerini depolamak ve bunları ara yazılım önyüklemesinden bir parçası olarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="43509-158">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="43509-159">Proje şablonlarını kullanılan varsayılan dosyası *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="43509-159">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="43509-160">Bu dosyayı bir örneğine yüklemesini `IConfiguration` uygulamanızı yapılır iç *haline*:</span><span class="sxs-lookup"><span data-stu-id="43509-160">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="43509-161">Uygulama okur `Configuration` ayarları almak için:</span><span class="sxs-lookup"><span data-stu-id="43509-161">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="43509-162">Bu yaklaşımın işlemi kullanarak gibi daha sağlam hale uzantıları vardır [bağımlılık ekleme](xref:fundamentals/dependency-injection) bu değerleri içeren bir hizmeti yüklemek için (dı).</span><span class="sxs-lookup"><span data-stu-id="43509-162">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="43509-163">DI yaklaşım yapılandırma nesnelerini kesin türü belirtilmiş bir dizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="43509-163">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

> [!NOTE]
> <span data-ttu-id="43509-164">ASP.NET Core yapılandırma daha kapsamlı bir başvuru için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="43509-164">For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="43509-165">Yerel bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="43509-165">Native dependency injection</span></span>

<span data-ttu-id="43509-166">Büyük, ölçeklendirilebilir uygulamalar oluştururken bir önemli bileşenleri ve hizmetlerin gevşek bağlantı hedeftir.</span><span class="sxs-lookup"><span data-stu-id="43509-166">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="43509-167">[Bağımlılık ekleme](xref:fundamentals/dependency-injection) bunu elde etmek için yaygın olarak kullanılan bir tekniktir ve ASP.NET Core yerel bir bileşeni olduğu.</span><span class="sxs-lookup"><span data-stu-id="43509-167">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="43509-168">ASP.NET uygulamaları, geliştiricilerin bağımlılık ekleme uygulamak için bir üçüncü taraf kitaplığı kullanır.</span><span class="sxs-lookup"><span data-stu-id="43509-168">In ASP.NET apps, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="43509-169">Bu tür bir kitaplığıdır [Unity](https://github.com/unitycontainer/unity), Microsoft Patterns & yöntemler tarafından sağlanan.</span><span class="sxs-lookup"><span data-stu-id="43509-169">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="43509-170">Bir bağımlılık ekleme Unity ile ayarlama örneği uygulama `IDependencyResolver` , saran bir `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="43509-170">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="43509-171">Bir örneğini oluşturun, `UnityContainer`, hizmetiniz kaydetmek ve bağımlılık çözümleyicisini ayarlayın `HttpConfiguration` yeni örneğine `UnityResolver` , kapsayıcı için:</span><span class="sxs-lookup"><span data-stu-id="43509-171">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="43509-172">Eklenmeye `IProductRepository` gerektiğinde:</span><span class="sxs-lookup"><span data-stu-id="43509-172">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="43509-173">Bağımlılık ekleme ASP.NET Core parçası olduğundan, hizmetinizi ekleyebilirsiniz `ConfigureServices` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="43509-173">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="43509-174">Depo her yerden, Unity ile doğru şekilde yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="43509-174">The repository can be injected anywhere, as was true with Unity.</span></span>

> [!NOTE]
> <span data-ttu-id="43509-175">ASP.NET Core bağımlılık ekleme bir ayrıntılı başvuru için bkz: [ASP.NET Core bağımlılık ekleme](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="43509-175">For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="43509-176">Statik dosyaları işleme</span><span class="sxs-lookup"><span data-stu-id="43509-176">Serve static files</span></span>

<span data-ttu-id="43509-177">Önemli, bir web geliştirme statik, istemci-tarafı varlıklar sunabilme özelliğini parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="43509-177">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="43509-178">En yaygın statik dosyaları HTML, CSS, Javascript ve görüntüleri gösterilebilir.</span><span class="sxs-lookup"><span data-stu-id="43509-178">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="43509-179">Bu dosyalar veya uygulama (CDN) yayımlanan konumda kaydedilir ve bir istek tarafından yüklenebilmesi için başvurulan gerekir.</span><span class="sxs-lookup"><span data-stu-id="43509-179">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="43509-180">Bu işlem, ASP.NET Core değişti.</span><span class="sxs-lookup"><span data-stu-id="43509-180">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="43509-181">ASP.NET, statik dosyalar çeşitli dizinlerde depolanan ve görünümlerde başvurulan.</span><span class="sxs-lookup"><span data-stu-id="43509-181">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="43509-182">ASP.NET çekirdek statik dosyaları "web root" depolanır (*&lt;içerik kök&gt;/wwwroot*), aksi belirtilmedikçe.</span><span class="sxs-lookup"><span data-stu-id="43509-182">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="43509-183">Dosyalar istek ardışık düzenine çağırarak yüklenir `UseStaticFiles` uzantısı yönteminden `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="43509-183">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> <span data-ttu-id="43509-184">.NET Framework'ü hedefleme NuGet paketini yükleyin `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="43509-184">If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="43509-185">Örneğin, bir görüntü varlığı *wwwroot/görüntüleri* klasördür erişilebilir bir konumda tarayıcıya gibi `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="43509-185">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

> [!NOTE]
> <span data-ttu-id="43509-186">ASP.NET Core içinde statik dosyaları sunma daha ayrıntılı başvuru için bkz: [statik dosyaları](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="43509-186">For a more in-depth reference to serving static files in ASP.NET Core, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43509-187">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="43509-187">Additional resources</span></span>

- [<span data-ttu-id="43509-188">.NET Core kitaplıklara bağlantı noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="43509-188">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
