---
title: ASP.NET Core tek sayfalı uygulamalar oluşturmak için JavaScriptServices kullanma
author: scottaddie
description: Bir tek sayfa uygulaması (ASP.NET Core tarafından desteklenen SPA) oluşturmak için JavaScriptServices kullanmanın avantajları hakkında bilgi edinin.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6d6a92427d5d4b853248e60a12625573c4375515
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523304"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="cd54a-103">ASP.NET Core tek sayfalı uygulamalar oluşturmak için JavaScriptServices kullanma</span><span class="sxs-lookup"><span data-stu-id="cd54a-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="cd54a-104">Tarafından [Scott Addie](https://github.com/scottaddie) ve [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="cd54a-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="cd54a-105">Tek sayfa uygulama (SPA) web uygulamasının kendi devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türdür.</span><span class="sxs-lookup"><span data-stu-id="cd54a-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="cd54a-106">İstemci tarafı SPA çerçeveleri veya kitaplıkları gibi tümleştirme [Angular](https://angular.io/) veya [React](https://facebook.github.io/react/), sunucu tarafı çerçevelerle gibi ASP.NET Core zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="cd54a-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) tümleştirme sürecindeki uyuşmazlıkları azaltmak için geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="cd54a-108">Ancak, farklı istemci ve sunucu teknoloji yığınları arasında sorunsuz bir işlemi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="cd54a-109">JavaScriptServices nedir</span><span class="sxs-lookup"><span data-stu-id="cd54a-109">What is JavaScriptServices</span></span>

<span data-ttu-id="cd54a-110">JavaScriptServices ASP.NET Core için istemci tarafı teknolojilerinin koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="cd54a-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="cd54a-111">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için hedefi sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="cd54a-112">Üç farklı NuGet paketlerini JavaScriptServices oluşur:</span><span class="sxs-lookup"><span data-stu-id="cd54a-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="cd54a-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="cd54a-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="cd54a-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="cd54a-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="cd54a-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="cd54a-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="cd54a-116">Bu paketler yararlıdır,:</span><span class="sxs-lookup"><span data-stu-id="cd54a-116">These packages are useful if you:</span></span>

* <span data-ttu-id="cd54a-117">Sunucu üzerinde JavaScript çalıştırma</span><span class="sxs-lookup"><span data-stu-id="cd54a-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="cd54a-118">SPA altyapı veya kitaplığı kullanın</span><span class="sxs-lookup"><span data-stu-id="cd54a-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="cd54a-119">İstemci tarafı Web varlıklarla oluşturun</span><span class="sxs-lookup"><span data-stu-id="cd54a-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="cd54a-120">Bu makalenin odak çoğunu SpaServices paketini kullanarak yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="cd54a-121">SpaServices nedir</span><span class="sxs-lookup"><span data-stu-id="cd54a-121">What is SpaServices</span></span>

<span data-ttu-id="cd54a-122">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için SpaServices oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cd54a-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="cd54a-123">SpaServices Spa'lar ASP.NET Core ile geliştirmek için gerekli değildir ve bu belirli istemci altyapısına kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="cd54a-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="cd54a-124">SpaServices gibi kullanışlı bir altyapı sağlar:</span><span class="sxs-lookup"><span data-stu-id="cd54a-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="cd54a-125">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="cd54a-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="cd54a-126">Web geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="cd54a-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="cd54a-127">Sık erişimli modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="cd54a-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="cd54a-128">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="cd54a-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="cd54a-129">Toplu olarak, bu altyapı bileşenlerini, hem geliştirme iş akışını hem de çalışma zamanı deneyimi geliştirin.</span><span class="sxs-lookup"><span data-stu-id="cd54a-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="cd54a-130">Bileşenleri ayrı ayrı önemsenmeksizin devralınabilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="cd54a-131">SpaServices kullanmanın önkoşulları</span><span class="sxs-lookup"><span data-stu-id="cd54a-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="cd54a-132">SpaServices ile çalışmak için aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="cd54a-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="cd54a-133">[Node.js](https://nodejs.org/) (sürüm 6 veya sonrası) npm ile</span><span class="sxs-lookup"><span data-stu-id="cd54a-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="cd54a-134">Bu bileşenler yüklenir ve bulunabilir doğrulamak için komut satırından aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd54a-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="cd54a-135">Not: bir Azure web sitesine dağıtıyorsanız, burada bir şey yapmanız gerekmez &mdash; Node.js yüklü olduğundan ve sunucu ortamlarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="cd54a-136">Visual Studio 2017'yi kullanarak Windows üzerinde kullanıyorsanız, SDK'yı seçerek yüklü **.NET Core çoklu platform geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="cd54a-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="cd54a-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="cd54a-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="cd54a-138">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="cd54a-138">Server-side prerendering</span></span>

<span data-ttu-id="cd54a-139">Bir evrensel (isomorphic olarak da bilinir) uygulama özellikli olan hem çalışan sunucu ve istemci üzerindeki bir JavaScript uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="cd54a-140">Angular, React ve diğer popüler çerçeveleri için bu uygulama geliştirme stili Evrensel bir platform sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd54a-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="cd54a-141">İlk Node.js aracılığıyla sunucuda framework bileşenleri oluşturma ve yürütme istemciye daha fazla temsilci olur.</span><span class="sxs-lookup"><span data-stu-id="cd54a-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="cd54a-142">ASP.NET Core [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) tarafından sağlanan SpaServices server JavaScript işlevleri çağrılarak sunucu tarafı prerendering uygulamasını basitleştirin.</span><span class="sxs-lookup"><span data-stu-id="cd54a-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cd54a-143">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cd54a-143">Prerequisites</span></span>

<span data-ttu-id="cd54a-144">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="cd54a-144">Install the following:</span></span>

* <span data-ttu-id="cd54a-145">[ASP.NET prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="cd54a-146">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cd54a-146">Configuration</span></span>

<span data-ttu-id="cd54a-147">Etiket Yardımcıları projesinin ad kayıt bulunabilir yapılan *_viewımports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cd54a-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="cd54a-148">Bu etiket Yardımcıları hemen bir HTML benzeri sözdizimi Razor görünüm içinde yararlanarak alt düzey API'ler ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:</span><span class="sxs-lookup"><span data-stu-id="cd54a-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="cd54a-149">`asp-prerender-module` Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="cd54a-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="cd54a-150">`asp-prerender-module` Etiketi Yardımcısı, önceki kod örneğinde kullanılan yürütür *ClientApp/dist/main-server.js* Node.js aracılığıyla sunucuda.</span><span class="sxs-lookup"><span data-stu-id="cd54a-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="cd54a-151">Açıklık'ın çok için *ana server.js* dosyasıdır TypeScript JavaScript transpilation görevin bir yapıt [Web](http://webpack.github.io/) derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="cd54a-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="cd54a-152">Web tanımlayan bir giriş noktası diğer `main-server`; ve bu diğer adı için bağımlılık grafiği geçişini başlar *ClientApp/önyükleme-server.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cd54a-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="cd54a-153">Aşağıdaki Angular örnekte *ClientApp/önyükleme-server.ts* dosya kullanır `createServerRenderer` işlevi ve `RenderResult` tür `aspnet-prerendering` npm paketi, sunucu işleme Node.js aracılığıyla yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="cd54a-154">Kesin türü belirtilmiş bir JavaScript içinde sarmalanmış bir Çözümle işlev çağrısı için sunucu tarafı işleme geçirilen hedefleyen bir HTML biçimlendirmesi `Promise` nesne.</span><span class="sxs-lookup"><span data-stu-id="cd54a-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="cd54a-155">`Promise` Nesnenin önemi olan, zaman uyumsuz olarak DOM'ın yer tutucu öğesi yerleştirmeye sayfasına HTML biçimlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd54a-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="cd54a-156">`asp-prerender-data` Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="cd54a-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="cd54a-157">İle birlikte zaman `asp-prerender-module` etiketi Yardımcısı `asp-prerender-data` etiketi Yardımcısı, bağlamsal bilgiler için sunucu tarafı JavaScript Razor görünümden geçirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="cd54a-158">Örneğin, kullanıcı verilerini aşağıdaki biçimlendirme geçirir `main-server` Modülü:</span><span class="sxs-lookup"><span data-stu-id="cd54a-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="cd54a-159">Alınan `UserName` bağımsız değişkeni yerleşik JSON serileştirici kullanılarak serileştirilmiş ve depolanan `params.data` nesne.</span><span class="sxs-lookup"><span data-stu-id="cd54a-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="cd54a-160">Angular aşağıdaki örnekte veri içinde kişiselleştirilmiş bir karşılama oluşturmak için kullanılan bir `h1` öğesi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="cd54a-161">Not: Etiket Yardımcıları içinde geçirilen özellik adları ile temsil edilen **PascalCase** gösterimi.</span><span class="sxs-lookup"><span data-stu-id="cd54a-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="cd54a-162">Burada aynı özellik adları ile gösterilir, JavaScript, Karşıtlık **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="cd54a-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="cd54a-163">Bu fark için varsayılan JSON seri hale getirme yapılandırması sorumludur.</span><span class="sxs-lookup"><span data-stu-id="cd54a-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="cd54a-164">Yukarıdaki kod örneğinde genişletmek için verileri sunucudan görünüme hydrating tarafından geçirilebilir `globals` özelliği için sağlanan `resolve` işlevi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="cd54a-165">`postList` İçinde tanımlanan bir dizi `globals` nesne bağlı tarayıcının genel `window` nesne.</span><span class="sxs-lookup"><span data-stu-id="cd54a-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="cd54a-166">Bu değişken hoisting genel kapsam için aynı verileri bir kez sunucuda ve istemcinin yeniden yüklenmesi için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Pencere nesnesi bağlı genel postList değişkeni](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="cd54a-168">Web geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="cd54a-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="cd54a-169">[Web geliştirme ara yazılım](https://webpack.github.io/docs/webpack-dev-middleware.html) yapabildiği Web yapılar kaynakları isteğe bağlı olarak Basitleştirilmiş geliştirme iş akışı sunar.</span><span class="sxs-lookup"><span data-stu-id="cd54a-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="cd54a-170">Ara yazılım, otomatik olarak derler ve bir sayfa tarayıcıya yeniden yüklendiğinde istemci-tarafı kaynaklar sunar.</span><span class="sxs-lookup"><span data-stu-id="cd54a-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="cd54a-171">Başka bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Web projenin npm derleme betiği aracılığıyla el ile başlatmak için yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="cd54a-172">Bir npm derleme betiği *package.json* dosya, aşağıdaki örnekte gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="cd54a-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="cd54a-173">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cd54a-173">Prerequisites</span></span>

<span data-ttu-id="cd54a-174">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="cd54a-174">Install the following:</span></span>

* <span data-ttu-id="cd54a-175">[ASP.NET Web](https://www.npmjs.com/package/aspnet-webpack) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="cd54a-176">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cd54a-176">Configuration</span></span>

<span data-ttu-id="cd54a-177">HTTP istek işlem hattı aşağıdaki kod aracılığıyla içine kayıtlı Web geliştirme ara yazılım *Startup.cs* dosyanın `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="cd54a-178">`UseWebpackDevMiddleware` Genişletme yöntemi çağrıldığında, önce [statik dosya barındırma kaydetme](xref:fundamentals/static-files) aracılığıyla `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd54a-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="cd54a-179">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="cd54a-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="cd54a-180">*Webpack.config.js* dosyanın `output.publicPath` özelliği söyler izlemek için bir ara yazılım `dist` klasördeki değişiklikleri:</span><span class="sxs-lookup"><span data-stu-id="cd54a-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="cd54a-181">Sık erişimli modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="cd54a-181">Hot Module Replacement</span></span>

<span data-ttu-id="cd54a-182">Web'ın düşünebilirsiniz [sık erişimli modülü değiştirme](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) özelliği'nın Gelişmiş hali olarak [Web geliştirme ara yazılım](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="cd54a-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="cd54a-183">HMR aynı avantajları sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="cd54a-184">Bu, geçerli bellek içi durumu ve hata ayıklama oturumu SPA ile neden tarayıcı yenileme ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="cd54a-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="cd54a-185">Web geliştirme ara yazılım hizmeti ve değişiklikleri tarayıcıya gönderilmesini anlamına gelir tarayıcı arasında canlı bir bağlantı yoktur.</span><span class="sxs-lookup"><span data-stu-id="cd54a-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cd54a-186">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cd54a-186">Prerequisites</span></span>

<span data-ttu-id="cd54a-187">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="cd54a-187">Install the following:</span></span>

* <span data-ttu-id="cd54a-188">[Web hot Ara](https://www.npmjs.com/package/webpack-hot-middleware) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="cd54a-189">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cd54a-189">Configuration</span></span>

<span data-ttu-id="cd54a-190">HMR bileşen MVC'nin HTTP istek işlem hattı, kaydedilmesi gerekir `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="cd54a-191">İle doğru olarak [Web geliştirme ara yazılım](#webpack-dev-middleware), `UseWebpackDevMiddleware` genişletme yöntemi çağrıldığında, önce `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd54a-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="cd54a-192">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="cd54a-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="cd54a-193">*Webpack.config.js* dosya tanımlamalıdır bir `plugins` dizi bile boş bırakılır:</span><span class="sxs-lookup"><span data-stu-id="cd54a-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="cd54a-194">Uygulamayı tarayıcıda yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="cd54a-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Sık erişimli modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="cd54a-196">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="cd54a-196">Routing helpers</span></span>

<span data-ttu-id="cd54a-197">Çoğu ASP.NET Core tabanlı Spa'lar içinde sunucu tarafı ek olarak istemci tarafı yönlendirmesi istersiniz.</span><span class="sxs-lookup"><span data-stu-id="cd54a-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="cd54a-198">SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="cd54a-199">Yoktur, ancak bir kenar büyük/küçük harf taşıyor sorunlarını: HTTP 404 yanıtları tanımlayan.</span><span class="sxs-lookup"><span data-stu-id="cd54a-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="cd54a-200">Senaryoyu göz önünde bulundurun uzantısız bir yolu `/some/page` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="cd54a-201">İstek deseni-sunucu tarafı rota match değil, ancak desenine bir istemci-tarafı rota ile eşleşmekte varsayılır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="cd54a-202">Şimdi gelen bir istek için göz önünde bulundurun `/images/user-512.png`, hangi genellikle bekliyor sunucudaki bir görüntü dosyası bulunamıyor.</span><span class="sxs-lookup"><span data-stu-id="cd54a-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="cd54a-203">Bu istenen kaynak yolu herhangi bir sunucu tarafı rota veya statik dosya eşleşmiyorsa, istemci-tarafı uygulaması, işlemek olası — genellikle bir 404 HTTP durum kodu iade etmek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="cd54a-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cd54a-204">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cd54a-204">Prerequisites</span></span>

<span data-ttu-id="cd54a-205">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="cd54a-205">Install the following:</span></span>

* <span data-ttu-id="cd54a-206">İstemci tarafı yönlendirme npm paket.</span><span class="sxs-lookup"><span data-stu-id="cd54a-206">The client-side routing npm package.</span></span> <span data-ttu-id="cd54a-207">Angular örnek olarak kullanıp:</span><span class="sxs-lookup"><span data-stu-id="cd54a-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="cd54a-208">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cd54a-208">Configuration</span></span>

<span data-ttu-id="cd54a-209">Adlı bir genişletme yöntemi `MapSpaFallbackRoute` kullanılır `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="cd54a-210">İpucu: Rotalar yapılandırılmış sırada değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="cd54a-211">Sonuç olarak, `default` rota önceki kod örneğinde kullanılan ilk desen eşleştirmesi için.</span><span class="sxs-lookup"><span data-stu-id="cd54a-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="cd54a-212">Yeni proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd54a-212">Creating a new project</span></span>

<span data-ttu-id="cd54a-213">JavaScriptServices önceden yapılandırılmış uygulama şablonları sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd54a-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="cd54a-214">SpaServices bu şablonları farklı çerçeveler ve kitaplıklar gibi Angular, React ve Redux ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="cd54a-215">Bu şablonlar, aşağıdaki komutu çalıştırarak, .NET Core CLI yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="cd54a-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="cd54a-216">Kullanılabilir SPA şablonlar listesi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="cd54a-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="cd54a-217">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="cd54a-217">Templates</span></span>                                 | <span data-ttu-id="cd54a-218">Kısa Ad</span><span class="sxs-lookup"><span data-stu-id="cd54a-218">Short Name</span></span> | <span data-ttu-id="cd54a-219">Dil</span><span class="sxs-lookup"><span data-stu-id="cd54a-219">Language</span></span> | <span data-ttu-id="cd54a-220">Etiketler</span><span class="sxs-lookup"><span data-stu-id="cd54a-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="cd54a-221">Angular ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="cd54a-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="cd54a-222">Angular</span><span class="sxs-lookup"><span data-stu-id="cd54a-222">angular</span></span>    | <span data-ttu-id="cd54a-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="cd54a-223">[C#]</span></span>     | <span data-ttu-id="cd54a-224">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cd54a-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="cd54a-225">React.js ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="cd54a-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="cd54a-226">react</span><span class="sxs-lookup"><span data-stu-id="cd54a-226">react</span></span>      | <span data-ttu-id="cd54a-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="cd54a-227">[C#]</span></span>     | <span data-ttu-id="cd54a-228">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cd54a-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="cd54a-229">ASP.NET Core MVC React.js ve Redux</span><span class="sxs-lookup"><span data-stu-id="cd54a-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="cd54a-230">açarken kilitlenmesi</span><span class="sxs-lookup"><span data-stu-id="cd54a-230">reactredux</span></span> | <span data-ttu-id="cd54a-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="cd54a-231">[C#]</span></span>     | <span data-ttu-id="cd54a-232">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cd54a-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="cd54a-233">SPA şablonlardan birini kullanarak yeni bir proje oluşturmak için dahil **kısa ad** şablonda, [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu.</span><span class="sxs-lookup"><span data-stu-id="cd54a-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="cd54a-234">Aşağıdaki komut, sunucu tarafı için yapılandırılan ASP.NET Core MVC ile Angular bir uygulama oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cd54a-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="cd54a-235">Çalışma zamanı yapılandırma modunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="cd54a-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="cd54a-236">Birincil çalışma zamanı yapılandırma için iki mod vardır:</span><span class="sxs-lookup"><span data-stu-id="cd54a-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="cd54a-237">**Geliştirme**:</span><span class="sxs-lookup"><span data-stu-id="cd54a-237">**Development**:</span></span>
  * <span data-ttu-id="cd54a-238">Hata ayıklamayı kolaylaştırmak için kaynak eşlemeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="cd54a-239">İstemci tarafı kod performans için en iyi duruma değil.</span><span class="sxs-lookup"><span data-stu-id="cd54a-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="cd54a-240">**Üretim**:</span><span class="sxs-lookup"><span data-stu-id="cd54a-240">**Production**:</span></span>
  * <span data-ttu-id="cd54a-241">Kaynak eşlemeleri dışlar.</span><span class="sxs-lookup"><span data-stu-id="cd54a-241">Excludes source maps.</span></span>
  * <span data-ttu-id="cd54a-242">İstemci tarafı kod paketleme ve küçültme ile en iyi duruma getirir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="cd54a-243">ASP.NET Core kullanan adlı bir ortam değişkeni `ASPNETCORE_ENVIRONMENT` yapılandırma modunu depolamak için.</span><span class="sxs-lookup"><span data-stu-id="cd54a-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="cd54a-244">Bkz: **[ortamı](xref:fundamentals/environments#set-the-environment)** daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="cd54a-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="cd54a-245">.NET Core CLI ile çalıştırma</span><span class="sxs-lookup"><span data-stu-id="cd54a-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="cd54a-246">Proje kök dizininde aşağıdaki komutu çalıştırarak gerekli NuGet ve npm paketlerini geri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="cd54a-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="cd54a-247">Derleme ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd54a-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="cd54a-248">Şunlara göre localhost üzerinde uygulama başladığında [çalışma zamanı yapılandırma modunu](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="cd54a-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="cd54a-249">Gezinme `http://localhost:5000` tarayıcısında giriş sayfası görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cd54a-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="cd54a-250">Visual Studio 2017 ile çalıştırma</span><span class="sxs-lookup"><span data-stu-id="cd54a-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="cd54a-251">Açık *.csproj* tarafından oluşturulan dosya [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu.</span><span class="sxs-lookup"><span data-stu-id="cd54a-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="cd54a-252">Gerekli NuGet ve npm paketleri proje üzerinde otomatik olarak geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="cd54a-253">Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırmaya hazır bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="cd54a-254">Yeşil çalıştırma düğmesine veya tuşuna tıklayın `Ctrl + F5`, ve tarayıcıda uygulama giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="cd54a-255">Uygulama ayarına göre localhost üzerinde çalışır [çalışma zamanı yapılandırma modunu](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="cd54a-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="cd54a-256">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="cd54a-256">Testing the app</span></span>

<span data-ttu-id="cd54a-257">SpaServices şablonları kullanarak istemci tarafı testleri çalıştırmak için önceden yapılandırılmış [Karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="cd54a-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="cd54a-258">Test Çalıştırıcısı bu testler için karma bilgileriyse jasmine bir popüler birim testi çerçevesi için JavaScript, ' dir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="cd54a-259">Karma çalışmak üzere yapılandırılmış [Web geliştirme ara yazılım](#webpack-dev-middleware) sağlayacak şekilde Geliştirici durdurmak ve değişiklikler her zaman test çalıştırması için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="cd54a-260">Test çalışması veya test çalışması karşı çalışan kod olup olmadığını test otomatik olarak çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="cd54a-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="cd54a-261">Angular uygulamasını örnek olarak kullanıp, Jasmine iki test çalışmaları zaten için sağlanan `CounterComponent` içinde *counter.component.spec.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cd54a-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="cd54a-262">Komut istemi açın *ClientApp* dizin.</span><span class="sxs-lookup"><span data-stu-id="cd54a-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="cd54a-263">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd54a-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="cd54a-264">Komut dosyası içinde tanımlanan ayarlarını okur Karma test Çalıştırıcısı başlatır *karma.conf.js* dosya.</span><span class="sxs-lookup"><span data-stu-id="cd54a-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="cd54a-265">Diğer ayarlar arasında *karma.conf.js* aracılığıyla yürütülecek test dosyalarını tanımlar, `files` dizi:</span><span class="sxs-lookup"><span data-stu-id="cd54a-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="cd54a-266">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="cd54a-266">Publishing the application</span></span>

<span data-ttu-id="cd54a-267">Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştiren çok uğraşmayı gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="cd54a-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="cd54a-268">Ne SpaServices adlı özel bir MSBuild hedefi ile tüm yayın işlem düzenler `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="cd54a-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="cd54a-269">MSBuild hedefi aşağıdaki sorumluluklara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="cd54a-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="cd54a-270">Npm paketlerini geri yükle</span><span class="sxs-lookup"><span data-stu-id="cd54a-270">Restore the npm packages</span></span>
1. <span data-ttu-id="cd54a-271">Üçüncü taraf, istemci tarafı varlıkların üretim sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd54a-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="cd54a-272">Özel istemci tarafı varlıkların üretim sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd54a-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="cd54a-273">Web oluşturulan varlıkları publish klasörüne kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="cd54a-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="cd54a-274">MSBuild hedefi çalıştırılırken çağrılır:</span><span class="sxs-lookup"><span data-stu-id="cd54a-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="cd54a-275">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cd54a-275">Additional resources</span></span>

* [<span data-ttu-id="cd54a-276">Angular belgeleri</span><span class="sxs-lookup"><span data-stu-id="cd54a-276">Angular Docs</span></span>](https://angular.io/docs)
