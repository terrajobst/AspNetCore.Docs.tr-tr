---
title: ASP.NET Core içinde tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanın
author: scottaddie
description: ASP.NET Core tarafından desteklenen tek sayfalı uygulama (SPA) oluşturmak için JavaScript Hizmetleri kullanmanın avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: c0c73882afd579510ad9cdf5b485c1d6fbeadd1c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663781"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="59bf7-103">ASP.NET Core içinde tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanın</span><span class="sxs-lookup"><span data-stu-id="59bf7-103">Use JavaScript Services to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="59bf7-104">[Scott Ade](https://github.com/scottaddie) ve [fiyaz hasan](https://fiyazhasan.me/) tarafından</span><span class="sxs-lookup"><span data-stu-id="59bf7-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](https://fiyazhasan.me/)</span></span>

<span data-ttu-id="59bf7-105">Tek sayfa uygulama (SPA) web uygulamasının kendi devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türdür.</span><span class="sxs-lookup"><span data-stu-id="59bf7-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="59bf7-106">ASP.NET Core gibi sunucu tarafı çerçeveleri ile, [angular](https://angular.io/) veya yanıt verme gibi ISTEMCI tarafı Spa [çerçevelerini veya kitaplıklarını](https://facebook.github.io/react/)tümleştirme zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks such as ASP.NET Core can be difficult.</span></span> <span data-ttu-id="59bf7-107">Tümleştirme sürecinde uçuşmayı azaltmak için JavaScript Hizmetleri geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-107">JavaScript Services was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="59bf7-108">Ancak, farklı istemci ve sunucu teknoloji yığınları arasında sorunsuz bir işlemi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-108">It enables seamless operation between the different client and server technology stacks.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="59bf7-109">Bu makalede açıklanan özellikler ASP.NET Core 3,0 itibariyle kullanımdan kalkmıştır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-109">The features described in this article are obsolete as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="59bf7-110">[Microsoft. AspNetCore. SpaServices. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) NuGet paketinde daha basıt bir spa çerçeveleri tümleştirme mekanizması mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="59bf7-110">A simpler SPA frameworks integration mechanism is available in the [Microsoft.AspNetCore.SpaServices.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) NuGet package.</span></span> <span data-ttu-id="59bf7-111">Daha fazla bilgi için bkz. [Microsoft. aspnetcore. spaservices ve Microsoft. AspNetCore. NodeServices üzerinde kullanımdan bulunan [Duyuru]](https://github.com/dotnet/AspNetCore/issues/12890).</span><span class="sxs-lookup"><span data-stu-id="59bf7-111">For more information, see [[Announcement] Obsoleting Microsoft.AspNetCore.SpaServices and Microsoft.AspNetCore.NodeServices](https://github.com/dotnet/AspNetCore/issues/12890).</span></span>

::: moniker-end

## <a name="what-is-javascript-services"></a><span data-ttu-id="59bf7-112">JavaScript Hizmetleri nedir?</span><span class="sxs-lookup"><span data-stu-id="59bf7-112">What is JavaScript Services</span></span>

<span data-ttu-id="59bf7-113">JavaScript Hizmetleri, ASP.NET Core yönelik istemci tarafı teknolojilerinin bir koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="59bf7-113">JavaScript Services is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="59bf7-114">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için hedefi sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-114">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="59bf7-115">JavaScript Hizmetleri iki ayrı NuGet paketi içerir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-115">JavaScript Services consists of two distinct NuGet packages:</span></span>

* <span data-ttu-id="59bf7-116">[Microsoft. AspNetCore. nodeservices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (nodeservices)</span><span class="sxs-lookup"><span data-stu-id="59bf7-116">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="59bf7-117">[Microsoft. AspNetCore. spaservices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (spaservices)</span><span class="sxs-lookup"><span data-stu-id="59bf7-117">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>

<span data-ttu-id="59bf7-118">Bu paketler, aşağıdaki senaryolarda faydalıdır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-118">These packages are useful in the following scenarios:</span></span>

* <span data-ttu-id="59bf7-119">Sunucu üzerinde JavaScript çalıştırma</span><span class="sxs-lookup"><span data-stu-id="59bf7-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="59bf7-120">SPA altyapı veya kitaplığı kullanın</span><span class="sxs-lookup"><span data-stu-id="59bf7-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="59bf7-121">İstemci tarafı Web varlıklarla oluşturun</span><span class="sxs-lookup"><span data-stu-id="59bf7-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="59bf7-122">Bu makalenin odak çoğunu SpaServices paketini kullanarak yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

## <a name="what-is-spaservices"></a><span data-ttu-id="59bf7-123">SpaServices nedir</span><span class="sxs-lookup"><span data-stu-id="59bf7-123">What is SpaServices</span></span>

<span data-ttu-id="59bf7-124">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için SpaServices oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="59bf7-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="59bf7-125">Istenmeyen hizmetler ASP.NET Core ile maça geliştirmek için gerekli değildir ve geliştiricileri belirli bir istemci çerçevesinde kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="59bf7-125">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock developers into a particular client framework.</span></span>

<span data-ttu-id="59bf7-126">SpaServices gibi kullanışlı bir altyapı sağlar:</span><span class="sxs-lookup"><span data-stu-id="59bf7-126">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="59bf7-127">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="59bf7-127">Server-side prerendering</span></span>](#server-side-prerendering)
* [<span data-ttu-id="59bf7-128">Web paketi geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="59bf7-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="59bf7-129">Sık kullanılan modül değiştirme</span><span class="sxs-lookup"><span data-stu-id="59bf7-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="59bf7-130">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="59bf7-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="59bf7-131">Toplu olarak, bu altyapı bileşenlerini, hem geliştirme iş akışını hem de çalışma zamanı deneyimi geliştirin.</span><span class="sxs-lookup"><span data-stu-id="59bf7-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="59bf7-132">Bileşenleri ayrı ayrı önemsenmeksizin devralınabilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-132">The components can be adopted individually.</span></span>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="59bf7-133">SpaServices kullanmanın önkoşulları</span><span class="sxs-lookup"><span data-stu-id="59bf7-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="59bf7-134">SpaServices ile çalışmak için aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="59bf7-134">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="59bf7-135">NPM ile [Node. js](https://nodejs.org/) (sürüm 6 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="59bf7-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>

  * <span data-ttu-id="59bf7-136">Bu bileşenler yüklenir ve bulunabilir doğrulamak için komut satırından aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59bf7-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

  * <span data-ttu-id="59bf7-137">Bir Azure Web sitesine dağıtım yapıyorsanız bir işlem gerekmez&mdash;Node. js yüklenir ve sunucu ortamlarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-137">If deploying to an Azure web site, no action is required&mdash;Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="59bf7-138">Visual Studio 2017 kullanarak Windows 'da SDK, **.NET Core platformlar arası geliştirme** iş yükü seçilerek yüklenir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-138">On Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="59bf7-139">[Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="59bf7-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

## <a name="server-side-prerendering"></a><span data-ttu-id="59bf7-140">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="59bf7-140">Server-side prerendering</span></span>

<span data-ttu-id="59bf7-141">Bir evrensel (isomorphic olarak da bilinir) uygulama özellikli olan hem çalışan sunucu ve istemci üzerindeki bir JavaScript uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-141">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="59bf7-142">Angular, React ve diğer popüler çerçeveleri için bu uygulama geliştirme stili Evrensel bir platform sağlar.</span><span class="sxs-lookup"><span data-stu-id="59bf7-142">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="59bf7-143">İlk Node.js aracılığıyla sunucuda framework bileşenleri oluşturma ve yürütme istemciye daha fazla temsilci olur.</span><span class="sxs-lookup"><span data-stu-id="59bf7-143">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="59bf7-144">SpaServices tarafından sunulan ASP.NET Core [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) , sunucuda JavaScript işlevlerini çağırarak sunucu tarafı prerendering 'in uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-144">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="server-side-prerendering-prerequisites"></a><span data-ttu-id="59bf7-145">Sunucu tarafı prerendering önkoşulları</span><span class="sxs-lookup"><span data-stu-id="59bf7-145">Server-side prerendering prerequisites</span></span>

<span data-ttu-id="59bf7-146">[ASPNET-prerendering](https://www.npmjs.com/package/aspnet-prerendering) NPM paketini yükler:</span><span class="sxs-lookup"><span data-stu-id="59bf7-146">Install the [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a><span data-ttu-id="59bf7-147">Sunucu tarafı prerendering yapılandırması</span><span class="sxs-lookup"><span data-stu-id="59bf7-147">Server-side prerendering configuration</span></span>

<span data-ttu-id="59bf7-148">Etiket Yardımcıları, projenin *_ViewImports. cshtml* dosyasında ad alanı kaydı aracılığıyla bulunabilir hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="59bf7-149">Bu etiket Yardımcıları hemen bir HTML benzeri sözdizimi Razor görünüm içinde yararlanarak alt düzey API'ler ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:</span><span class="sxs-lookup"><span data-stu-id="59bf7-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a><span data-ttu-id="59bf7-150">ASP-PreRender-Module etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="59bf7-150">asp-prerender-module Tag Helper</span></span>

<span data-ttu-id="59bf7-151">Yukarıdaki kod örneğinde kullanılan `asp-prerender-module` etiketi Yardımcısı, Node. js aracılığıyla sunucuda *clientapp/Dist/Main-Server. js* ' yi yürütür.</span><span class="sxs-lookup"><span data-stu-id="59bf7-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="59bf7-152">Netme 'nin sake, *Main-Server. js* dosyası, [WebPack](https://webpack.github.io/) derleme sürecinde TypeScript-to-JavaScript transpilation görevinin yapıtı.</span><span class="sxs-lookup"><span data-stu-id="59bf7-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](https://webpack.github.io/) build process.</span></span> <span data-ttu-id="59bf7-153">WebPack `main-server`; bir giriş noktası diğer adını tanımlar. ve, bu diğer ad için bağımlılık grafiğinin çapraz geçişi *clientapp/Boot-Server. TS* dosyasında başlar:</span><span class="sxs-lookup"><span data-stu-id="59bf7-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="59bf7-154">Aşağıdaki angular örneğinde, *clientapp/Boot-Server. TS* dosyası, Node. js aracılığıyla sunucu işlemesini yapılandırmak için `aspnet-prerendering` NPM paketinin `createServerRenderer` işlevini ve `RenderResult` türünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="59bf7-155">Sunucu tarafı işlemeye yönelik HTML biçimlendirmesi, kesin belirlenmiş bir JavaScript `Promise` nesnesine Sarmalanan bir Resolve işlev çağrısına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="59bf7-156">`Promise` nesnesinin önemi, DOM 'ın yer tutucu öğesine ekleme için sayfaya HTML işaretlemesini zaman uyumsuz olarak sağlar.</span><span class="sxs-lookup"><span data-stu-id="59bf7-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a><span data-ttu-id="59bf7-157">ASP-PreRender-veri etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="59bf7-157">asp-prerender-data Tag Helper</span></span>

<span data-ttu-id="59bf7-158">`asp-prerender-module` Tag Yardımcısı ile birlikte kullanıldığında, Razor görünümündeki bağlama bilgilerini sunucu tarafı JavaScript 'e geçirmek için `asp-prerender-data` etiketi Yardımcısı kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="59bf7-159">Örneğin, aşağıdaki biçimlendirme Kullanıcı verilerini `main-server` modülüne geçirir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="59bf7-160">Alınan `UserName` bağımsız değişkeni yerleşik JSON seri hale getirici kullanılarak serileştirilir ve `params.data` nesnesinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="59bf7-161">Aşağıdaki angular örneğinde, veriler `h1` öğesi içinde kişiselleştirilmiş bir selamlama oluşturmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="59bf7-162">Etiket yardımcılarının geçirildiği Özellik adları **PascalCase** gösterimi ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-162">Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="59bf7-163">Aynı özellik adlarının **camelCase**ile temsil edildiği JavaScript 'e kontrast.</span><span class="sxs-lookup"><span data-stu-id="59bf7-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="59bf7-164">Bu fark için varsayılan JSON seri hale getirme yapılandırması sorumludur.</span><span class="sxs-lookup"><span data-stu-id="59bf7-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="59bf7-165">Yukarıdaki kod örneğini genişletmek için, `resolve` işlevine sunulan `globals` özelliğini hibir şekilde görüntüleyerek, veriler sunucudan görünüme geçirilebilir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="59bf7-166">`globals` nesnesinin içinde tanımlanan `postList` dizisi tarayıcının Global `window` nesnesine iliştirilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="59bf7-167">Bu değişken hoisting genel kapsam için aynı verileri bir kez sunucuda ve istemcinin yeniden yüklenmesi için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Pencere nesnesi bağlı genel postList değişkeni](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a><span data-ttu-id="59bf7-169">Web geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="59bf7-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="59bf7-170">[Web paketi geliştirme ara yazılımı](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) , Web paketinin kaynakları isteğe bağlı olarak oluşturmakta olan kolaylaştırılmış bir geliştirme iş akışı</span><span class="sxs-lookup"><span data-stu-id="59bf7-170">[Webpack Dev Middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="59bf7-171">Ara yazılım, otomatik olarak derler ve bir sayfa tarayıcıya yeniden yüklendiğinde istemci-tarafı kaynaklar sunar.</span><span class="sxs-lookup"><span data-stu-id="59bf7-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="59bf7-172">Başka bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Web projenin npm derleme betiği aracılığıyla el ile başlatmak için yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="59bf7-173">*Package. JSON* dosyasındaki NPM derleme betiği aşağıdaki örnekte gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a><span data-ttu-id="59bf7-174">WebPack dev ara yazılım önkoşulları</span><span class="sxs-lookup"><span data-stu-id="59bf7-174">Webpack Dev Middleware prerequisites</span></span>

<span data-ttu-id="59bf7-175">[ASPNET-WebPack](https://www.npmjs.com/package/aspnet-webpack) NPM paketini yükler:</span><span class="sxs-lookup"><span data-stu-id="59bf7-175">Install the [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a><span data-ttu-id="59bf7-176">WebPack dev ara yazılım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="59bf7-176">Webpack Dev Middleware configuration</span></span>

<span data-ttu-id="59bf7-177">Web paketi geliştirme ara yazılımı, *Startup.cs* dosyasının `Configure` yönteminde aşağıdaki kod aracılığıyla http istek ardışık düzenine kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="59bf7-178">`UseStaticFiles` uzantısı yöntemiyle [statik dosya barındırma](xref:fundamentals/static-files) kaydedilmeden önce `UseWebpackDevMiddleware` uzantısı yönteminin çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="59bf7-179">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="59bf7-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="59bf7-180">*WebPack. config. js* dosyasının `output.publicPath` özelliği, ara yazılıma, değişiklikler için `dist` klasörünü izlemesini söyler:</span><span class="sxs-lookup"><span data-stu-id="59bf7-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a><span data-ttu-id="59bf7-181">Sık erişimli modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="59bf7-181">Hot Module Replacement</span></span>

<span data-ttu-id="59bf7-182">WebPack [geliştirme ara yazılımı](#webpack-dev-middleware)'nın bir evrimi olarak Web paketi 'Nin [sık kullanılan modül değiştirme](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) özelliğini düşünün.</span><span class="sxs-lookup"><span data-stu-id="59bf7-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="59bf7-183">HMR aynı avantajları sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="59bf7-184">Bu, geçerli bellek içi durumu ve hata ayıklama oturumu SPA ile neden tarayıcı yenileme ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="59bf7-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="59bf7-185">Web geliştirme ara yazılım hizmeti ve değişiklikleri tarayıcıya gönderilmesini anlamına gelir tarayıcı arasında canlı bir bağlantı yoktur.</span><span class="sxs-lookup"><span data-stu-id="59bf7-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="hot-module-replacement-prerequisites"></a><span data-ttu-id="59bf7-186">Sık kullanılan modül değiştirme önkoşulları</span><span class="sxs-lookup"><span data-stu-id="59bf7-186">Hot Module Replacement prerequisites</span></span>

<span data-ttu-id="59bf7-187">[WebPack-Hot-ara yazılım](https://www.npmjs.com/package/webpack-hot-middleware) NPM paketini yükler:</span><span class="sxs-lookup"><span data-stu-id="59bf7-187">Install the [webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a><span data-ttu-id="59bf7-188">Sık kullanılan modül değiştirme yapılandırması</span><span class="sxs-lookup"><span data-stu-id="59bf7-188">Hot Module Replacement configuration</span></span>

<span data-ttu-id="59bf7-189">HMR bileşeni, `Configure` yönteminde MVC 'nin HTTP isteği ardışık düzenine kayıtlı olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-189">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="59bf7-190">[Web paketi geliştirme ara yazılımı](#webpack-dev-middleware)ile doğru olduğu için, `UseWebpackDevMiddleware` uzantısı yönteminin `UseStaticFiles` uzantı yönteminden önce çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-190">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="59bf7-191">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="59bf7-191">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="59bf7-192">*Web paketi. config. js* dosyası boş bırakılmış olsa bile bir `plugins` dizisi tanımlamalıdır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-192">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="59bf7-193">Uygulamayı tarayıcıda yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="59bf7-193">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Sık erişimli modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a><span data-ttu-id="59bf7-195">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="59bf7-195">Routing helpers</span></span>

<span data-ttu-id="59bf7-196">Çoğu ASP.NET Core tabanlı maça, istemci tarafı yönlendirme genellikle sunucu tarafı yönlendirmeye ek olarak istenir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-196">In most ASP.NET Core-based SPAs, client-side routing is often desired in addition to server-side routing.</span></span> <span data-ttu-id="59bf7-197">SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-197">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="59bf7-198">Yoktur, ancak bir kenar büyük/küçük harf taşıyor sorunlarını: HTTP 404 yanıtları tanımlayan.</span><span class="sxs-lookup"><span data-stu-id="59bf7-198">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="59bf7-199">`/some/page` bir extensionın ' ın kullanıldığı senaryoyu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="59bf7-199">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="59bf7-200">İstek deseni-sunucu tarafı rota match değil, ancak desenine bir istemci-tarafı rota ile eşleşmekte varsayılır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-200">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="59bf7-201">Artık, genellikle sunucusunda bir görüntü dosyası bulmayı bekleyen `/images/user-512.png`gelen isteği düşünün.</span><span class="sxs-lookup"><span data-stu-id="59bf7-201">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="59bf7-202">İstenen kaynak yolu herhangi bir sunucu tarafı yolu veya statik dosya ile eşleşmiyorsa, istemci tarafı uygulamanın&mdash;bunu işleyemesi büyük olasılıkla 404 HTTP durum kodu döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="59bf7-202">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it&mdash;generally returning a 404 HTTP status code is desired.</span></span>

### <a name="routing-helpers-prerequisites"></a><span data-ttu-id="59bf7-203">Yönlendirme Yardımcıları önkoşulları</span><span class="sxs-lookup"><span data-stu-id="59bf7-203">Routing helpers prerequisites</span></span>

<span data-ttu-id="59bf7-204">İstemci tarafı yönlendirme NPM paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="59bf7-204">Install the client-side routing npm package.</span></span> <span data-ttu-id="59bf7-205">Angular örnek olarak kullanıp:</span><span class="sxs-lookup"><span data-stu-id="59bf7-205">Using Angular as an example:</span></span>

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a><span data-ttu-id="59bf7-206">Yönlendirme Yardımcıları yapılandırması</span><span class="sxs-lookup"><span data-stu-id="59bf7-206">Routing helpers configuration</span></span>

<span data-ttu-id="59bf7-207">`Configure` yönteminde `MapSpaFallbackRoute` adlı bir genişletme yöntemi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-207">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="59bf7-208">Yollar yapılandırıldıkları sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-208">Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="59bf7-209">Sonuç olarak, önceki kod örneğinde `default` yolu, önce model eşleştirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-209">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="59bf7-210">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="59bf7-210">Create a new project</span></span>

<span data-ttu-id="59bf7-211">JavaScript Hizmetleri önceden yapılandırılmış uygulama şablonları sağlar.</span><span class="sxs-lookup"><span data-stu-id="59bf7-211">JavaScript Services provide pre-configured application templates.</span></span> <span data-ttu-id="59bf7-212">Istenmeyen hizmetler, bu şablonlarda, angular, tepki ve Redux gibi farklı çerçeveler ve kitaplıklarla birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-212">SpaServices is used in these templates in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="59bf7-213">Bu şablonlar, aşağıdaki komutu çalıştırarak, .NET Core CLI yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-213">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="59bf7-214">Kullanılabilir SPA şablonlar listesi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-214">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="59bf7-215">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="59bf7-215">Templates</span></span>                                 | <span data-ttu-id="59bf7-216">Kısa Ad</span><span class="sxs-lookup"><span data-stu-id="59bf7-216">Short Name</span></span> | <span data-ttu-id="59bf7-217">Dil</span><span class="sxs-lookup"><span data-stu-id="59bf7-217">Language</span></span> | <span data-ttu-id="59bf7-218">Etiketler</span><span class="sxs-lookup"><span data-stu-id="59bf7-218">Tags</span></span>        |
| ------------------------------------------| :--------: | :------: | :---------: |
| <span data-ttu-id="59bf7-219">Angular ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="59bf7-219">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="59bf7-220">Angular</span><span class="sxs-lookup"><span data-stu-id="59bf7-220">angular</span></span>    | <span data-ttu-id="59bf7-221">[C#]</span><span class="sxs-lookup"><span data-stu-id="59bf7-221">[C#]</span></span>     | <span data-ttu-id="59bf7-222">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="59bf7-222">Web/MVC/SPA</span></span> |
| <span data-ttu-id="59bf7-223">React.js ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="59bf7-223">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="59bf7-224">react</span><span class="sxs-lookup"><span data-stu-id="59bf7-224">react</span></span>      | <span data-ttu-id="59bf7-225">[C#]</span><span class="sxs-lookup"><span data-stu-id="59bf7-225">[C#]</span></span>     | <span data-ttu-id="59bf7-226">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="59bf7-226">Web/MVC/SPA</span></span> |
| <span data-ttu-id="59bf7-227">ASP.NET Core MVC React.js ve Redux</span><span class="sxs-lookup"><span data-stu-id="59bf7-227">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="59bf7-228">açarken kilitlenmesi</span><span class="sxs-lookup"><span data-stu-id="59bf7-228">reactredux</span></span> | <span data-ttu-id="59bf7-229">[C#]</span><span class="sxs-lookup"><span data-stu-id="59bf7-229">[C#]</span></span>     | <span data-ttu-id="59bf7-230">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="59bf7-230">Web/MVC/SPA</span></span> |

<span data-ttu-id="59bf7-231">SPA şablonlarından birini kullanarak yeni bir proje oluşturmak için, [DotNet New](/dotnet/core/tools/dotnet-new) komutuna şablonun **kısa adını** ekleyin.</span><span class="sxs-lookup"><span data-stu-id="59bf7-231">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="59bf7-232">Aşağıdaki komut, sunucu tarafı için yapılandırılan ASP.NET Core MVC ile Angular bir uygulama oluşturur:</span><span class="sxs-lookup"><span data-stu-id="59bf7-232">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="59bf7-233">Çalışma zamanı yapılandırma modunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="59bf7-233">Set the runtime configuration mode</span></span>

<span data-ttu-id="59bf7-234">Birincil çalışma zamanı yapılandırma için iki mod vardır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-234">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="59bf7-235">**Geliştirme**:</span><span class="sxs-lookup"><span data-stu-id="59bf7-235">**Development**:</span></span>
  * <span data-ttu-id="59bf7-236">Hata ayıklamayı kolaylaştırmak için kaynak eşlemeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-236">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="59bf7-237">İstemci tarafı kod performans için en iyi duruma değil.</span><span class="sxs-lookup"><span data-stu-id="59bf7-237">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="59bf7-238">**Üretim**:</span><span class="sxs-lookup"><span data-stu-id="59bf7-238">**Production**:</span></span>
  * <span data-ttu-id="59bf7-239">Kaynak eşlemeleri dışlar.</span><span class="sxs-lookup"><span data-stu-id="59bf7-239">Excludes source maps.</span></span>
  * <span data-ttu-id="59bf7-240">Paketleme ve küçültmeye göre istemci tarafı kodunu iyileştirir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-240">Optimizes the client-side code via bundling and minification.</span></span>

<span data-ttu-id="59bf7-241">ASP.NET Core, yapılandırma modunu depolamak için `ASPNETCORE_ENVIRONMENT` adında bir ortam değişkeni kullanır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-241">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="59bf7-242">Daha fazla bilgi için bkz. [ortamı ayarlama](xref:fundamentals/environments#set-the-environment).</span><span class="sxs-lookup"><span data-stu-id="59bf7-242">For more information, see [Set the environment](xref:fundamentals/environments#set-the-environment).</span></span>

### <a name="run-with-net-core-cli"></a><span data-ttu-id="59bf7-243">.NET Core CLI ile Çalıştır</span><span class="sxs-lookup"><span data-stu-id="59bf7-243">Run with .NET Core CLI</span></span>

<span data-ttu-id="59bf7-244">Proje kök dizininde aşağıdaki komutu çalıştırarak gerekli NuGet ve npm paketlerini geri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="59bf7-244">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```dotnetcli
dotnet restore && npm i
```

<span data-ttu-id="59bf7-245">Derleme ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59bf7-245">Build and run the application:</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="59bf7-246">Uygulama, [çalışma zamanı yapılandırma moduna](#set-the-runtime-configuration-mode)göre localhost üzerinde başlatılır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-246">The application starts on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span> <span data-ttu-id="59bf7-247">Tarayıcıda `http://localhost:5000` gitme giriş sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="59bf7-247">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="run-with-visual-studio-2017"></a><span data-ttu-id="59bf7-248">Visual Studio 2017 ile çalıştırma</span><span class="sxs-lookup"><span data-stu-id="59bf7-248">Run with Visual Studio 2017</span></span>

<span data-ttu-id="59bf7-249">[DotNet New](/dotnet/core/tools/dotnet-new) komutu tarafından oluşturulan *. csproj* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="59bf7-249">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="59bf7-250">Gerekli NuGet ve npm paketleri proje üzerinde otomatik olarak geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-250">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="59bf7-251">Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırmaya hazır bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-251">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="59bf7-252">Yeşil çalışma düğmesine tıklayın veya `Ctrl + F5`tuşuna basın ve tarayıcı uygulamanın giriş sayfasında açılır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-252">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="59bf7-253">Uygulama, [çalışma zamanı yapılandırma moduna](#set-the-runtime-configuration-mode)göre localhost üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-253">The application runs on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span>

## <a name="test-the-app"></a><span data-ttu-id="59bf7-254">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="59bf7-254">Test the app</span></span>

<span data-ttu-id="59bf7-255">SpaServices şablonları, [karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/)kullanarak istemci tarafı testleri çalıştıracak şekilde önceden yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-255">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="59bf7-256">Test Çalıştırıcısı bu testler için karma bilgileriyse jasmine bir popüler birim testi çerçevesi için JavaScript, ' dir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-256">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="59bf7-257">Karma, geliştiricilerin her değişiklik yapıldığında testi durdurmak ve çalıştırmak için gerekli olmaması gibi [WebPack dev ara yazılımı](#webpack-dev-middleware) ile birlikte çalışmak üzere yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-257">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="59bf7-258">Test çalışması veya test çalışması karşı çalışan kod olup olmadığını test otomatik olarak çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-258">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="59bf7-259">Angular uygulamasını örnek olarak kullanarak, *Counter. Component. spec. TS* dosyasında `CounterComponent` Için Iki Jasmine test çalışması zaten sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-259">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="59bf7-260">*Clientapp* dizininde komut istemi ' ni açın.</span><span class="sxs-lookup"><span data-stu-id="59bf7-260">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="59bf7-261">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59bf7-261">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="59bf7-262">Betik, *karma. conf. js* dosyasında tanımlanan ayarları okuyan karma Test Çalıştırıcısı 'nı başlatır.</span><span class="sxs-lookup"><span data-stu-id="59bf7-262">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="59bf7-263">Diğer ayarların yanı sıra, *karma. conf. js* `files` dizisi aracılığıyla yürütülecek test dosyalarını tanımlar:</span><span class="sxs-lookup"><span data-stu-id="59bf7-263">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a><span data-ttu-id="59bf7-264">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="59bf7-264">Publish the app</span></span>

<span data-ttu-id="59bf7-265">Azure 'da yayımlama hakkında daha fazla bilgi için [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore.Docs/issues/12474) bakın.</span><span class="sxs-lookup"><span data-stu-id="59bf7-265">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/12474) for more information on publishing to Azure.</span></span>

<span data-ttu-id="59bf7-266">Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştiren çok uğraşmayı gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="59bf7-266">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="59bf7-267">Ktam, Spaservice, yayın sürecinin tamamını `RunWebpack`adlı özel bir MSBuild hedefi ile düzenler:</span><span class="sxs-lookup"><span data-stu-id="59bf7-267">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="59bf7-268">MSBuild hedefi aşağıdaki sorumluluklara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="59bf7-268">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="59bf7-269">NPM paketlerini geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="59bf7-269">Restore the npm packages.</span></span>
1. <span data-ttu-id="59bf7-270">Üçüncü taraf, istemci tarafı varlıkların üretim sınıfı derlemesini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="59bf7-270">Create a production-grade build of the third-party, client-side assets.</span></span>
1. <span data-ttu-id="59bf7-271">Özel istemci tarafı varlıkların üretim sınıfı derlemesini oluşturma.</span><span class="sxs-lookup"><span data-stu-id="59bf7-271">Create a production-grade build of the custom client-side assets.</span></span>
1. <span data-ttu-id="59bf7-272">Web paketi tarafından oluşturulan varlıkları Yayımla klasörüne kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="59bf7-272">Copy the Webpack-generated assets to the publish folder.</span></span>

<span data-ttu-id="59bf7-273">MSBuild hedefi çalıştırılırken çağrılır:</span><span class="sxs-lookup"><span data-stu-id="59bf7-273">The MSBuild target is invoked when running:</span></span>

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="59bf7-274">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="59bf7-274">Additional resources</span></span>

* [<span data-ttu-id="59bf7-275">Angular belgeleri</span><span class="sxs-lookup"><span data-stu-id="59bf7-275">Angular Docs</span></span>](https://angular.io/docs)
