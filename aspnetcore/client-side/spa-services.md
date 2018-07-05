---
title: ASP.NET Core tek sayfalı uygulamalar oluşturmak için JavaScriptServices kullanma
author: scottaddie
description: Bir tek sayfa uygulaması (ASP.NET Core tarafından desteklenen SPA) oluşturmak için JavaScriptServices kullanmanın avantajları hakkında bilgi edinin.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6ac922d82e5c93343cd0e9df312719c6df121dcb
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37434006"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="49bb2-103">ASP.NET Core tek sayfalı uygulamalar oluşturmak için JavaScriptServices kullanma</span><span class="sxs-lookup"><span data-stu-id="49bb2-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="49bb2-104">Tarafından [Scott Addie](https://github.com/scottaddie) ve [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="49bb2-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="49bb2-105">Tek sayfa uygulama (SPA) web uygulamasının kendi devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türdür.</span><span class="sxs-lookup"><span data-stu-id="49bb2-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="49bb2-106">İstemci tarafı SPA çerçeveleri veya kitaplıkları gibi tümleştirme [Angular](https://angular.io/) veya [React](https://facebook.github.io/react/), sunucu tarafı çerçevelerle gibi ASP.NET Core zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="49bb2-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) tümleştirme sürecindeki uyuşmazlıkları azaltmak için geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="49bb2-108">Ancak, farklı istemci ve sunucu teknoloji yığınları arasında sorunsuz bir işlemi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="49bb2-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="49bb2-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="49bb2-110">JavaScriptServices nedir?</span><span class="sxs-lookup"><span data-stu-id="49bb2-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="49bb2-111">JavaScriptServices ASP.NET Core için istemci tarafı teknolojilerinin koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="49bb2-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="49bb2-112">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için hedefi sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="49bb2-113">Üç farklı NuGet paketlerini JavaScriptServices oluşur:</span><span class="sxs-lookup"><span data-stu-id="49bb2-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="49bb2-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="49bb2-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="49bb2-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="49bb2-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="49bb2-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="49bb2-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="49bb2-117">Bu paketler yararlıdır,:</span><span class="sxs-lookup"><span data-stu-id="49bb2-117">These packages are useful if you:</span></span>
* <span data-ttu-id="49bb2-118">Sunucu üzerinde JavaScript çalıştırma</span><span class="sxs-lookup"><span data-stu-id="49bb2-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="49bb2-119">SPA altyapı veya kitaplığı kullanın</span><span class="sxs-lookup"><span data-stu-id="49bb2-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="49bb2-120">İstemci tarafı Web varlıklarla oluşturun</span><span class="sxs-lookup"><span data-stu-id="49bb2-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="49bb2-121">Bu makalenin odak çoğunu SpaServices paketini kullanarak yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="49bb2-122">SpaServices nedir?</span><span class="sxs-lookup"><span data-stu-id="49bb2-122">What is SpaServices?</span></span>

<span data-ttu-id="49bb2-123">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için SpaServices oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="49bb2-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="49bb2-124">SpaServices Spa'lar ASP.NET Core ile geliştirmek için gerekli değildir ve bu belirli istemci altyapısına kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="49bb2-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="49bb2-125">SpaServices gibi kullanışlı bir altyapı sağlar:</span><span class="sxs-lookup"><span data-stu-id="49bb2-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="49bb2-126">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="49bb2-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="49bb2-127">Web geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="49bb2-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="49bb2-128">Sık erişimli modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="49bb2-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="49bb2-129">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="49bb2-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="49bb2-130">Toplu olarak, bu altyapı bileşenlerini, hem geliştirme iş akışını hem de çalışma zamanı deneyimi geliştirin.</span><span class="sxs-lookup"><span data-stu-id="49bb2-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="49bb2-131">Bileşenleri ayrı ayrı önemsenmeksizin devralınabilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="49bb2-132">SpaServices kullanmanın önkoşulları</span><span class="sxs-lookup"><span data-stu-id="49bb2-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="49bb2-133">SpaServices ile çalışmak için aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="49bb2-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="49bb2-134">[Node.js](https://nodejs.org/) (sürüm 6 veya sonrası) npm ile</span><span class="sxs-lookup"><span data-stu-id="49bb2-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="49bb2-135">Bu bileşenler yüklenir ve bulunabilir doğrulamak için komut satırından aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="49bb2-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="49bb2-136">Not: bir Azure web sitesine dağıtıyorsanız, burada bir şey yapmanız gerekmez &mdash; Node.js yüklü olduğundan ve sunucu ortamlarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="49bb2-137">Visual Studio 2017'yi kullanarak Windows üzerinde kullanıyorsanız, SDK'yı seçerek yüklü **.NET Core çoklu platform geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="49bb2-137">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="49bb2-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="49bb2-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="49bb2-139">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="49bb2-139">Server-side prerendering</span></span>

<span data-ttu-id="49bb2-140">Bir evrensel (isomorphic olarak da bilinir) uygulama özellikli olan hem çalışan sunucu ve istemci üzerindeki bir JavaScript uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-140">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="49bb2-141">Angular, React ve diğer popüler çerçeveleri için bu uygulama geliştirme stili Evrensel bir platform sağlar.</span><span class="sxs-lookup"><span data-stu-id="49bb2-141">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="49bb2-142">İlk Node.js aracılığıyla sunucuda framework bileşenleri oluşturma ve yürütme istemciye daha fazla temsilci olur.</span><span class="sxs-lookup"><span data-stu-id="49bb2-142">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="49bb2-143">ASP.NET Core [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) tarafından sağlanan SpaServices server JavaScript işlevleri çağrılarak sunucu tarafı prerendering uygulamasını basitleştirin.</span><span class="sxs-lookup"><span data-stu-id="49bb2-143">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="49bb2-144">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="49bb2-144">Prerequisites</span></span>

<span data-ttu-id="49bb2-145">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="49bb2-145">Install the following:</span></span>
* <span data-ttu-id="49bb2-146">[ASP.NET prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-146">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="49bb2-147">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="49bb2-147">Configuration</span></span>

<span data-ttu-id="49bb2-148">Etiket Yardımcıları projesinin ad kayıt bulunabilir yapılan *_viewımports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="49bb2-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="49bb2-149">Bu etiket Yardımcıları hemen bir HTML benzeri sözdizimi Razor görünüm içinde yararlanarak alt düzey API'ler ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:</span><span class="sxs-lookup"><span data-stu-id="49bb2-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="49bb2-150">`asp-prerender-module` Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="49bb2-150">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="49bb2-151">`asp-prerender-module` Etiketi Yardımcısı, önceki kod örneğinde kullanılan yürütür *ClientApp/dist/main-server.js* Node.js aracılığıyla sunucuda.</span><span class="sxs-lookup"><span data-stu-id="49bb2-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="49bb2-152">Açıklık'ın çok için *ana server.js* dosyasıdır TypeScript JavaScript transpilation görevin bir yapıt [Web](http://webpack.github.io/) derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="49bb2-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="49bb2-153">Web tanımlayan bir giriş noktası diğer `main-server`; ve bu diğer adı için bağımlılık grafiği geçişini başlar *ClientApp/önyükleme-server.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="49bb2-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="49bb2-154">Aşağıdaki Angular örnekte *ClientApp/önyükleme-server.ts* dosya kullanır `createServerRenderer` işlevi ve `RenderResult` tür `aspnet-prerendering` npm paketi, sunucu işleme Node.js aracılığıyla yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="49bb2-155">Kesin türü belirtilmiş bir JavaScript içinde sarmalanmış bir Çözümle işlev çağrısı için sunucu tarafı işleme geçirilen hedefleyen bir HTML biçimlendirmesi `Promise` nesne.</span><span class="sxs-lookup"><span data-stu-id="49bb2-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="49bb2-156">`Promise` Nesnenin önemi olan, zaman uyumsuz olarak DOM'ın yer tutucu öğesi yerleştirmeye sayfasına HTML biçimlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="49bb2-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="49bb2-157">`asp-prerender-data` Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="49bb2-157">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="49bb2-158">İle birlikte zaman `asp-prerender-module` etiketi Yardımcısı `asp-prerender-data` etiketi Yardımcısı, bağlamsal bilgiler için sunucu tarafı JavaScript Razor görünümden geçirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="49bb2-159">Örneğin, kullanıcı verilerini aşağıdaki biçimlendirme geçirir `main-server` Modülü:</span><span class="sxs-lookup"><span data-stu-id="49bb2-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="49bb2-160">Alınan `UserName` bağımsız değişkeni yerleşik JSON serileştirici kullanılarak serileştirilmiş ve depolanan `params.data` nesne.</span><span class="sxs-lookup"><span data-stu-id="49bb2-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="49bb2-161">Angular aşağıdaki örnekte veri içinde kişiselleştirilmiş bir karşılama oluşturmak için kullanılan bir `h1` öğesi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="49bb2-162">Not: Etiket Yardımcıları içinde geçirilen özellik adları ile temsil edilen **PascalCase** gösterimi.</span><span class="sxs-lookup"><span data-stu-id="49bb2-162">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="49bb2-163">Burada aynı özellik adları ile gösterilir, JavaScript, Karşıtlık **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="49bb2-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="49bb2-164">Bu fark için varsayılan JSON seri hale getirme yapılandırması sorumludur.</span><span class="sxs-lookup"><span data-stu-id="49bb2-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="49bb2-165">Yukarıdaki kod örneğinde genişletmek için verileri sunucudan görünüme hydrating tarafından geçirilebilir `globals` özelliği için sağlanan `resolve` işlevi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="49bb2-166">`postList` İçinde tanımlanan bir dizi `globals` nesne bağlı tarayıcının genel `window` nesne.</span><span class="sxs-lookup"><span data-stu-id="49bb2-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="49bb2-167">Bu değişken hoisting genel kapsam için aynı verileri bir kez sunucuda ve istemcinin yeniden yüklenmesi için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Pencere nesnesi bağlı genel postList değişkeni](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="49bb2-169">Web geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="49bb2-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="49bb2-170">[Web geliştirme ara yazılım](https://webpack.github.io/docs/webpack-dev-middleware.html) yapabildiği Web yapılar kaynakları isteğe bağlı olarak Basitleştirilmiş geliştirme iş akışı sunar.</span><span class="sxs-lookup"><span data-stu-id="49bb2-170">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="49bb2-171">Ara yazılım, otomatik olarak derler ve bir sayfa tarayıcıya yeniden yüklendiğinde istemci-tarafı kaynaklar sunar.</span><span class="sxs-lookup"><span data-stu-id="49bb2-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="49bb2-172">Başka bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Web projenin npm derleme betiği aracılığıyla el ile başlatmak için yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="49bb2-173">Bir npm derleme betiği *package.json* dosya, aşağıdaki örnekte gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="49bb2-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="49bb2-174">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="49bb2-174">Prerequisites</span></span>

<span data-ttu-id="49bb2-175">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="49bb2-175">Install the following:</span></span>
* <span data-ttu-id="49bb2-176">[ASP.NET Web](https://www.npmjs.com/package/aspnet-webpack) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-176">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="49bb2-177">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="49bb2-177">Configuration</span></span>

<span data-ttu-id="49bb2-178">HTTP istek işlem hattı aşağıdaki kod aracılığıyla içine kayıtlı Web geliştirme ara yazılım *Startup.cs* dosyanın `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-178">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="49bb2-179">`UseWebpackDevMiddleware` Genişletme yöntemi çağrıldığında, önce [statik dosya barındırma kaydetme](xref:fundamentals/static-files) aracılığıyla `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="49bb2-179">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="49bb2-180">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="49bb2-180">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="49bb2-181">*Webpack.config.js* dosyanın `output.publicPath` özelliği söyler izlemek için bir ara yazılım `dist` klasördeki değişiklikleri:</span><span class="sxs-lookup"><span data-stu-id="49bb2-181">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="49bb2-182">Sık erişimli modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="49bb2-182">Hot Module Replacement</span></span>

<span data-ttu-id="49bb2-183">Web'ın düşünebilirsiniz [sık erişimli modülü değiştirme](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) özelliği'nın Gelişmiş hali olarak [Web geliştirme ara yazılım](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="49bb2-183">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="49bb2-184">HMR aynı avantajları sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-184">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="49bb2-185">Bu, geçerli bellek içi durumu ve hata ayıklama oturumu SPA ile neden tarayıcı yenileme ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="49bb2-185">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="49bb2-186">Web geliştirme ara yazılım hizmeti ve değişiklikleri tarayıcıya gönderilmesini anlamına gelir tarayıcı arasında canlı bir bağlantı yoktur.</span><span class="sxs-lookup"><span data-stu-id="49bb2-186">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="49bb2-187">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="49bb2-187">Prerequisites</span></span>

<span data-ttu-id="49bb2-188">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="49bb2-188">Install the following:</span></span>
* <span data-ttu-id="49bb2-189">[Web hot Ara](https://www.npmjs.com/package/webpack-hot-middleware) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-189">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="49bb2-190">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="49bb2-190">Configuration</span></span>

<span data-ttu-id="49bb2-191">HMR bileşen MVC'nin HTTP istek işlem hattı, kaydedilmesi gerekir `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-191">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="49bb2-192">İle doğru olarak [Web geliştirme ara yazılım](#webpack-dev-middleware), `UseWebpackDevMiddleware` genişletme yöntemi çağrıldığında, önce `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="49bb2-192">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="49bb2-193">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="49bb2-193">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="49bb2-194">*Webpack.config.js* dosya tanımlamalıdır bir `plugins` dizi bile boş bırakılır:</span><span class="sxs-lookup"><span data-stu-id="49bb2-194">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="49bb2-195">Uygulamayı tarayıcıda yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="49bb2-195">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Sık erişimli modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="49bb2-197">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="49bb2-197">Routing helpers</span></span>

<span data-ttu-id="49bb2-198">Çoğu ASP.NET Core tabanlı Spa'lar içinde sunucu tarafı ek olarak istemci tarafı yönlendirmesi istersiniz.</span><span class="sxs-lookup"><span data-stu-id="49bb2-198">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="49bb2-199">SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-199">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="49bb2-200">Yoktur, ancak bir kenar büyük/küçük harf taşıyor sorunlarını: HTTP 404 yanıtları tanımlayan.</span><span class="sxs-lookup"><span data-stu-id="49bb2-200">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="49bb2-201">Senaryoyu göz önünde bulundurun uzantısız bir yolu `/some/page` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-201">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="49bb2-202">İstek deseni-sunucu tarafı rota match değil, ancak desenine bir istemci-tarafı rota ile eşleşmekte varsayılır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-202">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="49bb2-203">Şimdi gelen bir istek için göz önünde bulundurun `/images/user-512.png`, hangi genellikle bekliyor sunucudaki bir görüntü dosyası bulunamıyor.</span><span class="sxs-lookup"><span data-stu-id="49bb2-203">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="49bb2-204">Bu istenen kaynak yolu herhangi bir sunucu tarafı rota veya statik dosya eşleşmiyorsa, istemci-tarafı uygulaması, işlemek olası — genellikle bir 404 HTTP durum kodu iade etmek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="49bb2-204">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="49bb2-205">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="49bb2-205">Prerequisites</span></span>

<span data-ttu-id="49bb2-206">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="49bb2-206">Install the following:</span></span>
* <span data-ttu-id="49bb2-207">İstemci tarafı yönlendirme npm paket.</span><span class="sxs-lookup"><span data-stu-id="49bb2-207">The client-side routing npm package.</span></span> <span data-ttu-id="49bb2-208">Angular örnek olarak kullanıp:</span><span class="sxs-lookup"><span data-stu-id="49bb2-208">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="49bb2-209">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="49bb2-209">Configuration</span></span>

<span data-ttu-id="49bb2-210">Adlı bir genişletme yöntemi `MapSpaFallbackRoute` kullanılır `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-210">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="49bb2-211">İpucu: Rotalar yapılandırılmış sırada değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-211">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="49bb2-212">Sonuç olarak, `default` rota önceki kod örneğinde kullanılan ilk desen eşleştirmesi için.</span><span class="sxs-lookup"><span data-stu-id="49bb2-212">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="49bb2-213">Yeni proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="49bb2-213">Creating a new project</span></span>

<span data-ttu-id="49bb2-214">JavaScriptServices önceden yapılandırılmış uygulama şablonları sağlar.</span><span class="sxs-lookup"><span data-stu-id="49bb2-214">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="49bb2-215">SpaServices bu şablonları farklı çerçeveler ve kitaplıklar gibi Angular, React ve Redux ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-215">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="49bb2-216">Bu şablonlar, aşağıdaki komutu çalıştırarak, .NET Core CLI yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="49bb2-216">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="49bb2-217">Kullanılabilir SPA şablonlar listesi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="49bb2-217">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="49bb2-218">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="49bb2-218">Templates</span></span>                                 | <span data-ttu-id="49bb2-219">Kısa Ad</span><span class="sxs-lookup"><span data-stu-id="49bb2-219">Short Name</span></span> | <span data-ttu-id="49bb2-220">Dil</span><span class="sxs-lookup"><span data-stu-id="49bb2-220">Language</span></span> | <span data-ttu-id="49bb2-221">Etiketler</span><span class="sxs-lookup"><span data-stu-id="49bb2-221">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="49bb2-222">Angular ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="49bb2-222">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="49bb2-223">angular</span><span class="sxs-lookup"><span data-stu-id="49bb2-223">angular</span></span>    | <span data-ttu-id="49bb2-224">[C#]</span><span class="sxs-lookup"><span data-stu-id="49bb2-224">[C#]</span></span>     | <span data-ttu-id="49bb2-225">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="49bb2-225">Web/MVC/SPA</span></span> |
| <span data-ttu-id="49bb2-226">React.js ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="49bb2-226">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="49bb2-227">react</span><span class="sxs-lookup"><span data-stu-id="49bb2-227">react</span></span>      | <span data-ttu-id="49bb2-228">[C#]</span><span class="sxs-lookup"><span data-stu-id="49bb2-228">[C#]</span></span>     | <span data-ttu-id="49bb2-229">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="49bb2-229">Web/MVC/SPA</span></span> |
| <span data-ttu-id="49bb2-230">ASP.NET Core MVC React.js ve Redux</span><span class="sxs-lookup"><span data-stu-id="49bb2-230">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="49bb2-231">açarken kilitlenmesi</span><span class="sxs-lookup"><span data-stu-id="49bb2-231">reactredux</span></span> | <span data-ttu-id="49bb2-232">[C#]</span><span class="sxs-lookup"><span data-stu-id="49bb2-232">[C#]</span></span>     | <span data-ttu-id="49bb2-233">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="49bb2-233">Web/MVC/SPA</span></span> |

<span data-ttu-id="49bb2-234">SPA şablonlardan birini kullanarak yeni bir proje oluşturmak için dahil **kısa ad** şablonda, [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu.</span><span class="sxs-lookup"><span data-stu-id="49bb2-234">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="49bb2-235">Aşağıdaki komut, sunucu tarafı için yapılandırılan ASP.NET Core MVC ile Angular bir uygulama oluşturur:</span><span class="sxs-lookup"><span data-stu-id="49bb2-235">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="49bb2-236">Çalışma zamanı yapılandırma modunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="49bb2-236">Set the runtime configuration mode</span></span>

<span data-ttu-id="49bb2-237">Birincil çalışma zamanı yapılandırma için iki mod vardır:</span><span class="sxs-lookup"><span data-stu-id="49bb2-237">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="49bb2-238">**Geliştirme**:</span><span class="sxs-lookup"><span data-stu-id="49bb2-238">**Development**:</span></span>
    * <span data-ttu-id="49bb2-239">Hata ayıklamayı kolaylaştırmak için kaynak eşlemeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-239">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="49bb2-240">İstemci tarafı kod performans için en iyi duruma değil.</span><span class="sxs-lookup"><span data-stu-id="49bb2-240">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="49bb2-241">**Üretim**:</span><span class="sxs-lookup"><span data-stu-id="49bb2-241">**Production**:</span></span>
    * <span data-ttu-id="49bb2-242">Kaynak eşlemeleri dışlar.</span><span class="sxs-lookup"><span data-stu-id="49bb2-242">Excludes source maps.</span></span>
    * <span data-ttu-id="49bb2-243">İstemci tarafı kod paketleme ve küçültme ile en iyi duruma getirir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-243">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="49bb2-244">ASP.NET Core kullanan adlı bir ortam değişkeni `ASPNETCORE_ENVIRONMENT` yapılandırma modunu depolamak için.</span><span class="sxs-lookup"><span data-stu-id="49bb2-244">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="49bb2-245">Bkz: **[ortamı](xref:fundamentals/environments#set-the-environment)** daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="49bb2-245">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="49bb2-246">.NET Core CLI ile çalıştırma</span><span class="sxs-lookup"><span data-stu-id="49bb2-246">Running with .NET Core CLI</span></span>

<span data-ttu-id="49bb2-247">Proje kök dizininde aşağıdaki komutu çalıştırarak gerekli NuGet ve npm paketlerini geri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="49bb2-247">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="49bb2-248">Derleme ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="49bb2-248">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="49bb2-249">Şunlara göre localhost üzerinde uygulama başladığında [çalışma zamanı yapılandırma modunu](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="49bb2-249">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="49bb2-250">Gezinme `http://localhost:5000` tarayıcısında giriş sayfası görüntüler.</span><span class="sxs-lookup"><span data-stu-id="49bb2-250">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="49bb2-251">Visual Studio 2017 ile çalıştırma</span><span class="sxs-lookup"><span data-stu-id="49bb2-251">Running with Visual Studio 2017</span></span>

<span data-ttu-id="49bb2-252">Açık *.csproj* tarafından oluşturulan dosya [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu.</span><span class="sxs-lookup"><span data-stu-id="49bb2-252">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="49bb2-253">Gerekli NuGet ve npm paketleri proje üzerinde otomatik olarak geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-253">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="49bb2-254">Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırmaya hazır bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-254">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="49bb2-255">Yeşil çalıştırma düğmesine veya tuşuna tıklayın `Ctrl + F5`, ve tarayıcıda uygulama giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-255">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="49bb2-256">Uygulama ayarına göre localhost üzerinde çalışır [çalışma zamanı yapılandırma modunu](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="49bb2-256">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="49bb2-257">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="49bb2-257">Testing the app</span></span>

<span data-ttu-id="49bb2-258">SpaServices şablonları kullanarak istemci tarafı testleri çalıştırmak için önceden yapılandırılmış [Karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="49bb2-258">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="49bb2-259">Test Çalıştırıcısı bu testler için karma bilgileriyse jasmine bir popüler birim testi çerçevesi için JavaScript, ' dir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-259">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="49bb2-260">Karma çalışmak üzere yapılandırılmış [Web geliştirme ara yazılım](#webpack-dev-middleware) sağlayacak şekilde Geliştirici durdurmak ve değişiklikler her zaman test çalıştırması için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-260">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="49bb2-261">Test çalışması veya test çalışması karşı çalışan kod olup olmadığını test otomatik olarak çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="49bb2-261">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="49bb2-262">Angular uygulamasını örnek olarak kullanıp, Jasmine iki test çalışmaları zaten için sağlanan `CounterComponent` içinde *counter.component.spec.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="49bb2-262">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="49bb2-263">Komut istemi açın *ClientApp* dizin.</span><span class="sxs-lookup"><span data-stu-id="49bb2-263">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="49bb2-264">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="49bb2-264">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="49bb2-265">Komut dosyası içinde tanımlanan ayarlarını okur Karma test Çalıştırıcısı başlatır *karma.conf.js* dosya.</span><span class="sxs-lookup"><span data-stu-id="49bb2-265">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="49bb2-266">Diğer ayarlar arasında *karma.conf.js* aracılığıyla yürütülecek test dosyalarını tanımlar, `files` dizi:</span><span class="sxs-lookup"><span data-stu-id="49bb2-266">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="49bb2-267">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="49bb2-267">Publishing the application</span></span>

<span data-ttu-id="49bb2-268">Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştiren çok uğraşmayı gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="49bb2-268">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="49bb2-269">Ne SpaServices adlı özel bir MSBuild hedefi ile tüm yayın işlem düzenler `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="49bb2-269">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="49bb2-270">MSBuild hedefi aşağıdaki sorumluluklara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="49bb2-270">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="49bb2-271">Npm paketlerini geri yükle</span><span class="sxs-lookup"><span data-stu-id="49bb2-271">Restore the npm packages</span></span>
1. <span data-ttu-id="49bb2-272">Üçüncü taraf, istemci tarafı varlıkların üretim sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="49bb2-272">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="49bb2-273">Özel istemci tarafı varlıkların üretim sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="49bb2-273">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="49bb2-274">Web oluşturulan varlıkları publish klasörüne kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="49bb2-274">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="49bb2-275">MSBuild hedefi çalıştırılırken çağrılır:</span><span class="sxs-lookup"><span data-stu-id="49bb2-275">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="49bb2-276">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="49bb2-276">Additional resources</span></span>

* [<span data-ttu-id="49bb2-277">Angular belgeleri</span><span class="sxs-lookup"><span data-stu-id="49bb2-277">Angular Docs</span></span>](https://angular.io/docs)
