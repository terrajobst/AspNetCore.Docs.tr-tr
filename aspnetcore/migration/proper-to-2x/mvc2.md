---
title: ASP.NET 'den ASP.NET Core 2,0 'ye geçiş
author: isaac2004
description: Mevcut ASP.NET MVC veya Web API uygulamalarını ASP.NET Core 2,0 ' ye geçirmeye yönelik rehberlik alın.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/mvc2
ms.openlocfilehash: 11bd3b948afaedc675ac4249099969382683f653
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664614"
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="c3781-103">ASP.NET 'den ASP.NET Core 2,0 'ye geçiş</span><span class="sxs-lookup"><span data-stu-id="c3781-103">Migrate from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="c3781-104">İle [Isaac Levi](https://isaaclevin.com) tarafından</span><span class="sxs-lookup"><span data-stu-id="c3781-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="c3781-105">Bu makale, ASP.NET uygulamalarını ASP.NET Core 2,0 ' ye geçirmeye yönelik bir başvuru kılavuzu görevi görür.</span><span class="sxs-lookup"><span data-stu-id="c3781-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c3781-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c3781-106">Prerequisites</span></span>

<span data-ttu-id="c3781-107">.Net downloads 'lerden **aşağıdakilerden birini** yükler [: Windows](https://www.microsoft.com/net/download/windows):</span><span class="sxs-lookup"><span data-stu-id="c3781-107">Install **one** of the following from [.NET Downloads: Windows](https://www.microsoft.com/net/download/windows):</span></span>

* <span data-ttu-id="c3781-108">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="c3781-108">.NET Core SDK</span></span>
* <span data-ttu-id="c3781-109">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c3781-109">Visual Studio for Windows</span></span>
  * <span data-ttu-id="c3781-110">**ASP.net ve Web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="c3781-110">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="c3781-111">**.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="c3781-111">**.NET Core cross-platform development** workload</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="c3781-112">Hedef çerçeveler</span><span class="sxs-lookup"><span data-stu-id="c3781-112">Target frameworks</span></span>

<span data-ttu-id="c3781-113">ASP.NET Core 2,0 projeleri, geliştiricilere .NET Core, .NET Framework veya her ikisinin de hedeflenme esnekliği sunar.</span><span class="sxs-lookup"><span data-stu-id="c3781-113">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="c3781-114">En uygun hedef Framework 'ü belirlemek için, bkz. [sunucu uygulamaları için .NET Core ve .NET Framework arasından seçim yapma](/dotnet/standard/choosing-core-framework-server) .</span><span class="sxs-lookup"><span data-stu-id="c3781-114">See [Choosing between .NET Core and .NET Framework for server apps](/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="c3781-115">.NET Framework hedeflenirken, projelerin ayrı NuGet paketlerine başvurması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c3781-115">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="c3781-116">.NET Core 'u hedeflemek, ASP.NET Core 2,0 [metapackage](xref:fundamentals/metapackage)sayesinde çok sayıda açık paket başvurularını ortadan kaldırmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="c3781-116">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="c3781-117">`Microsoft.AspNetCore.All` metapackage 'i projenize yükler:</span><span class="sxs-lookup"><span data-stu-id="c3781-117">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
</ItemGroup>
```

<span data-ttu-id="c3781-118">Metapackage kullanıldığında, metapackage içinde başvurulan hiçbir paket uygulamayla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="c3781-118">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="c3781-119">.NET Core çalışma zamanı deposu bu varlıkları içerir ve performansı artırmak için önceden ön derlenmiş hale getiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="c3781-119">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="c3781-120">Daha fazla ayrıntı için bkz. <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="c3781-120">See <xref:fundamentals/metapackage> for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="c3781-121">Proje yapısı farkları</span><span class="sxs-lookup"><span data-stu-id="c3781-121">Project structure differences</span></span>

<span data-ttu-id="c3781-122">*. Csproj* dosya biçimi ASP.NET Core basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c3781-122">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="c3781-123">Bazı önemli değişiklikler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="c3781-123">Some notable changes include:</span></span>

* <span data-ttu-id="c3781-124">Dosyaların açıkça eklenmesi, projenin bir parçası olarak kabul edilmesi için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c3781-124">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="c3781-125">Bu, büyük ekipler üzerinde çalışırken XML birleştirme çakışmalarının riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="c3781-125">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
* <span data-ttu-id="c3781-126">Diğer projelere GUID tabanlı başvurular yoktur ve bu da dosya okunabilirliğini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="c3781-126">There are no GUID-based references to other projects, which improves file readability.</span></span>
* <span data-ttu-id="c3781-127">Dosya Visual Studio 'da kaldırmadan düzenlenebilir:</span><span class="sxs-lookup"><span data-stu-id="c3781-127">The file can be edited without unloading it in Visual Studio:</span></span>

  ![Visual Studio 2017 ' de CSPROJ bağlam menüsü seçeneğini Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="c3781-129">Global. asax dosyası değiştirme</span><span class="sxs-lookup"><span data-stu-id="c3781-129">Global.asax file replacement</span></span>

<span data-ttu-id="c3781-130">ASP.NET Core, bir uygulamayı önyüklemeden yeni bir mekanizma getirmiştir.</span><span class="sxs-lookup"><span data-stu-id="c3781-130">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="c3781-131">ASP.NET uygulamaları için giriş noktası *Global. asax* dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="c3781-131">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="c3781-132">Rota yapılandırması ve filtre ve alan kayıtları gibi görevler, *Global. asax* dosyasında işlenir.</span><span class="sxs-lookup"><span data-stu-id="c3781-132">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="c3781-133">Bu yaklaşım, uygulamayı ve dağıtıldığı sunucuyu uygulamayı kesintiye uğratan bir şekilde bağar.</span><span class="sxs-lookup"><span data-stu-id="c3781-133">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="c3781-134">Bağımsız olarak, [Owin](https://owin.org/) , birden çok çerçeveyi birlikte kullanmanın bir temizleyici yolunu sağlamak için sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="c3781-134">In an effort to decouple, [OWIN](https://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="c3781-135">OWIN yalnızca gereken modülleri eklemek için bir işlem hattı sağlar.</span><span class="sxs-lookup"><span data-stu-id="c3781-135">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="c3781-136">Barındırma ortamı, hizmetleri ve uygulamanın istek ardışık düzenini yapılandırmak için bir [Başlangıç](xref:fundamentals/startup) işlevi alır.</span><span class="sxs-lookup"><span data-stu-id="c3781-136">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="c3781-137">`Startup` uygulamayla bir ara yazılım kümesini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="c3781-137">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="c3781-138">Her istek için, uygulama bir ara yazılım bileşeninin her birini bağlantılı listenin baş işaretçisi ile mevcut bir işleyici kümesine çağırır.</span><span class="sxs-lookup"><span data-stu-id="c3781-138">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="c3781-139">Her bir ara yazılım bileşeni, istek işleme ardışık düzenine bir veya daha fazla işleyici ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c3781-139">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="c3781-140">Bu, listenin yeni başlığı olan işleyiciye bir başvuru döndürülerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c3781-140">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="c3781-141">Her işleyici, listedeki bir sonraki işleyiciyi hatırlayıp çağırmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="c3781-141">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="c3781-142">ASP.NET Core, bir uygulamaya giriş noktası `Startup`ve artık *Global. asax*' a bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="c3781-142">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="c3781-143">.NET Framework ile OWIN kullanırken, işlem hattı olarak aşağıdaki gibi bir şey kullanın:</span><span class="sxs-lookup"><span data-stu-id="c3781-143">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="c3781-144">Bu, varsayılan rotalarınızı yapılandırır ve varsayılan olarak JSON üzerinden XmlSerialization olur.</span><span class="sxs-lookup"><span data-stu-id="c3781-144">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="c3781-145">Gerektiğinde bu işlem hattına başka bir ara yazılım ekleyin (Yükleme Hizmetleri, yapılandırma ayarları, statik dosyalar vb.).</span><span class="sxs-lookup"><span data-stu-id="c3781-145">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="c3781-146">ASP.NET Core benzer bir yaklaşım kullanır, ancak girişi işlemek için OWıN 'u kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="c3781-146">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="c3781-147">Bunun yerine, *Program.cs* `Main` yöntemi aracılığıyla yapılır (konsol uygulamalarına benzer) ve `Startup` bu şekilde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="c3781-147">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="c3781-148">`Startup` bir `Configure` yöntemi içermelidir.</span><span class="sxs-lookup"><span data-stu-id="c3781-148">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="c3781-149">`Configure`, işlem hattına gerekli bir ara yazılım ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c3781-149">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="c3781-150">Aşağıdaki örnekte (varsayılan Web sitesi şablonundan), işlem hattını desteğiyle yapılandırmak için birkaç uzantı yöntemi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="c3781-150">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="c3781-151">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="c3781-151">BrowserLink</span></span>](https://vswebessentials.com/features/browserlink)
* <span data-ttu-id="c3781-152">Hata sayfaları</span><span class="sxs-lookup"><span data-stu-id="c3781-152">Error pages</span></span>
* <span data-ttu-id="c3781-153">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="c3781-153">Static files</span></span>
* <span data-ttu-id="c3781-154">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c3781-154">ASP.NET Core MVC</span></span>
* <span data-ttu-id="c3781-155">Kimlik</span><span class="sxs-lookup"><span data-stu-id="c3781-155">Identity</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="c3781-156">Ana bilgisayar ve uygulama, gelecekte farklı bir platforma geçme esnekliği sağlayan bir şekilde ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="c3781-156">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="c3781-157">ASP.NET Core başlatmaya ve ara yazılıma yönelik daha ayrıntılı bir başvuru için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c3781-157">For a more in-depth reference to ASP.NET Core Startup and Middleware, see <xref:fundamentals/startup>.</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="c3781-158">Yapılandırma depolama</span><span class="sxs-lookup"><span data-stu-id="c3781-158">Storing configurations</span></span>

<span data-ttu-id="c3781-159">ASP.NET, ayarları depolamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="c3781-159">ASP.NET supports storing settings.</span></span> <span data-ttu-id="c3781-160">Bu ayar, örneğin, uygulamaların dağıtıldığı ortamı desteklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c3781-160">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="c3781-161">Genel bir uygulama, *Web. config* dosyasının `<appSettings>` bölümünde tüm özel anahtar-değer çiftlerini depolandı:</span><span class="sxs-lookup"><span data-stu-id="c3781-161">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="c3781-162">Uygulamalar, `System.Configuration` ad alanındaki `ConfigurationManager.AppSettings` koleksiyonunu kullanarak bu ayarları okur:</span><span class="sxs-lookup"><span data-stu-id="c3781-162">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="c3781-163">ASP.NET Core, uygulama için yapılandırma verilerini herhangi bir dosyada depolayıp ara yazılım önyükleme 'nin bir parçası olarak yükleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c3781-163">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="c3781-164">Proje şablonlarında kullanılan varsayılan dosya *appSettings. JSON*' dır:</span><span class="sxs-lookup"><span data-stu-id="c3781-164">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="c3781-165">Bu dosyayı uygulamanızın içindeki bir `IConfiguration` örneğine yüklemek *Startup.cs*içinde yapılır:</span><span class="sxs-lookup"><span data-stu-id="c3781-165">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="c3781-166">Uygulama, ayarları almak için `Configuration` 'dan okur:</span><span class="sxs-lookup"><span data-stu-id="c3781-166">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="c3781-167">Bu şekilde, bu değerlere sahip bir hizmeti yüklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) kullanma gibi işlemleri daha sağlam hale getirmek için bu yaklaşımın uzantıları vardır.</span><span class="sxs-lookup"><span data-stu-id="c3781-167">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="c3781-168">Dı yaklaşımı, kesin türü belirtilmiş bir yapılandırma nesneleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c3781-168">The DI approach provides a strongly-typed set of configuration objects.</span></span>

```csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
```

<span data-ttu-id="c3781-169">**Note:** ASP.NET Core yapılandırmaya yönelik daha ayrıntılı bir başvuru için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c3781-169">**Note:** For a more in-depth reference to ASP.NET Core configuration, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="c3781-170">Yerel bağımlılığı ekleme</span><span class="sxs-lookup"><span data-stu-id="c3781-170">Native dependency injection</span></span>

<span data-ttu-id="c3781-171">Büyük, ölçeklenebilir uygulamalar, bileşenlerin ve hizmetlerin gevşek bağlantısı olan önemli bir hedef.</span><span class="sxs-lookup"><span data-stu-id="c3781-171">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="c3781-172">[Bağımlılık ekleme](xref:fundamentals/dependency-injection) , bunu elde etmek için popüler bir tekniktir ve ASP.NET Core yerel bir bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="c3781-172">[Dependency injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="c3781-173">ASP.NET uygulamalarında, geliştiriciler bağımlılık ekleme işlemini uygulamak için bir üçüncü taraf kitaplığı kullanır.</span><span class="sxs-lookup"><span data-stu-id="c3781-173">In ASP.NET applications, developers rely on a third-party library to implement dependency injection.</span></span> <span data-ttu-id="c3781-174">Bu tür bir kitaplık [, Microsoft](https://github.com/unitycontainer/unity)düzenleri & uygulamalar tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c3781-174">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="c3781-175">Unity ile bağımlılık ekleme ayarlamayı bir örnek, bir `UnityContainer`sarmalayan `IDependencyResolver` uygulamadır:</span><span class="sxs-lookup"><span data-stu-id="c3781-175">An example of setting up dependency injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](samples/sample8.cs)]

<span data-ttu-id="c3781-176">`UnityContainer`bir örneğini oluşturun, hizmetinizi kaydedin ve `HttpConfiguration` bağımlılık çözümleyici 'yi Kapsayıcınız için yeni `UnityResolver` örneğine ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="c3781-176">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](samples/sample9.cs)]

<span data-ttu-id="c3781-177">Gerektiğinde `IProductRepository` ekle:</span><span class="sxs-lookup"><span data-stu-id="c3781-177">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](samples/sample5.cs)]

<span data-ttu-id="c3781-178">Bağımlılık ekleme ASP.NET Core bir parçası olduğu için hizmetinizi `Startup.ConfigureServices`ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c3781-178">Because dependency injection is part of ASP.NET Core, you can add your service in the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="c3781-179">Depo, Unity ile doğru olduğu için her yerde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="c3781-179">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="c3781-180">ASP.NET Core bağımlılık ekleme hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c3781-180">For more information on dependency injection in ASP.NET Core, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="c3781-181">Statik dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="c3781-181">Serving static files</span></span>

<span data-ttu-id="c3781-182">Web geliştirmenin önemli bir bölümü, statik, istemci tarafı varlıkları sunma olanağıdır.</span><span class="sxs-lookup"><span data-stu-id="c3781-182">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="c3781-183">Statik dosyaların en yaygın örnekleri HTML, CSS, JavaScript ve görüntülerdir.</span><span class="sxs-lookup"><span data-stu-id="c3781-183">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="c3781-184">Bu dosyaların, uygulamanın yayınlanan konumuna (veya CDN) kaydedilmesi gerekir ve bu dosyalar bir istek tarafından yüklenebilmeleri için başvurulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c3781-184">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="c3781-185">Bu işlem ASP.NET Core değişti.</span><span class="sxs-lookup"><span data-stu-id="c3781-185">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="c3781-186">ASP.NET ' de statik dosyalar çeşitli dizinlerde depolanır ve görünümlerde başvurulur.</span><span class="sxs-lookup"><span data-stu-id="c3781-186">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="c3781-187">ASP.NET Core, aksi belirtilmedikçe statik dosyalar "Web root" ( *&lt;içerik kökü&gt;/Wwwroot*) içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="c3781-187">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="c3781-188">Dosyalar, `Startup.Configure``UseStaticFiles` uzantısı yöntemi çağrılarak istek ardışık düzenine yüklenir:</span><span class="sxs-lookup"><span data-stu-id="c3781-188">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="c3781-189">**Note:** .NET Framework hedefliyorsanız, NuGet paketini `Microsoft.AspNetCore.StaticFiles`yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="c3781-189">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="c3781-190">Örneğin, *Wwwroot/görüntüler* klasöründeki bir görüntü varlığına, `http://<app>/images/<imageFileName>`gibi bir konumda tarayıcı tarafından erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="c3781-190">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="c3781-191">**Note:** ASP.NET Core içinde statik dosyalar sunma hakkında daha ayrıntılı bir başvuru için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="c3781-191">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see <xref:fundamentals/static-files>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c3781-192">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c3781-192">Additional resources</span></span>

* [<span data-ttu-id="c3781-193">Kitaplıkları .NET Core 'a taşıma</span><span class="sxs-lookup"><span data-stu-id="c3781-193">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
