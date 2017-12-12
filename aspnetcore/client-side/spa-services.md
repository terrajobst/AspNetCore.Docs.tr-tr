---
title: "Tek sayfa uygulamaları oluşturmak için JavaScriptServices kullanma"
author: scottaddie
description: "Bir tek sayfa uygulaması (ASP.NET Core tarafından yedeklenen SPA) oluşturmak için JavaScriptServices kullanmanın avantajları hakkında bilgi edinin."
keywords: "ASP.NET Core, Açısal SPA, JavaScriptServices, SpaServices"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d47910beef9195295c8da6ac81b83b3ffe20124
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a><span data-ttu-id="b2fbb-104">ASP.NET çekirdeği ile tek sayfa uygulamaları oluşturmak için JavaScriptServices kullanma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-104">Using JavaScriptServices for Creating Single Page Applications with ASP.NET Core</span></span>

<span data-ttu-id="b2fbb-105">Tarafından [Scott Addie](https://github.com/scottaddie) ve [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="b2fbb-105">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="b2fbb-106">Tek sayfa uygulama (SPA) web uygulaması, devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türde değil.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-106">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="b2fbb-107">İstemci tarafı SPA çerçeveleri veya kitaplıklar gibi tümleştirme [Angular](https://angular.io/) veya [tepki](https://facebook.github.io/react/), ASP.NET Core zor olabilir gibi sunucu tarafı çerçeveleri ile.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-107">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="b2fbb-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) tümleştirme işleminde uyuşmazlık azaltmak için geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="b2fbb-109">Farklı istemci ve sunucu teknolojisi yığınları arasında sorunsuz işlemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-109">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="b2fbb-110">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2fbb-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="b2fbb-111">JavaScriptServices nedir?</span><span class="sxs-lookup"><span data-stu-id="b2fbb-111">What is JavaScriptServices?</span></span>

<span data-ttu-id="b2fbb-112">JavaScriptServices ASP.NET Core için istemci tarafı teknolojileri koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-112">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="b2fbb-113">ASP.NET Core geliştiricilerinin SPAs oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak konumlandırmak için kendi hedeftir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-113">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="b2fbb-114">JavaScriptServices üç ayrı NuGet paketlerini oluşur:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-114">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="b2fbb-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="b2fbb-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="b2fbb-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="b2fbb-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="b2fbb-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="b2fbb-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="b2fbb-118">Bu paketleri yararlı varsa:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-118">These packages are useful if you:</span></span>
* <span data-ttu-id="b2fbb-119">JavaScript sunucusunda çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b2fbb-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="b2fbb-120">SPA framework veya kitaplık kullanın</span><span class="sxs-lookup"><span data-stu-id="b2fbb-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="b2fbb-121">İstemci-tarafı varlıklar Webpack ile derleme</span><span class="sxs-lookup"><span data-stu-id="b2fbb-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="b2fbb-122">Bu makalede odak çoğunu SpaServices paketini kullanarak yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="b2fbb-123">SpaServices nedir?</span><span class="sxs-lookup"><span data-stu-id="b2fbb-123">What is SpaServices?</span></span>

<span data-ttu-id="b2fbb-124">SpaServices geliştiricilerinin SPAs oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak ASP.NET Core konumlandırmak için oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="b2fbb-125">SpaServices SPAs ASP.NET Core ile geliştirmek için gerekli değildir ve belirli bir istemci çerçeveye kilitleyin değil.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-125">SpaServices is not required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="b2fbb-126">SpaServices gibi yararlı altyapısı sağlar:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-126">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="b2fbb-127">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="b2fbb-127">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="b2fbb-128">Webpack geliştirme Ara</span><span class="sxs-lookup"><span data-stu-id="b2fbb-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="b2fbb-129">Sık kullanılan modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="b2fbb-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="b2fbb-130">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="b2fbb-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="b2fbb-131">Toplu olarak, bu altyapı bileşenlerini geliştirme iş akışı ve çalışma zamanı deneyimini geliştirin.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="b2fbb-132">Bileşenleri tek tek benimsenen.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-132">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="b2fbb-133">SpaServices kullanma önkoşulları</span><span class="sxs-lookup"><span data-stu-id="b2fbb-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="b2fbb-134">SpaServices ile çalışmak için aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-134">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="b2fbb-135">[Node.js](https://nodejs.org/) (sürüm 6 veya sonrası) npm ile</span><span class="sxs-lookup"><span data-stu-id="b2fbb-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="b2fbb-136">Bu bileşenlerin yüklü olduğundan ve bulunabilir doğrulamak için aşağıdaki komut satırından çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="b2fbb-137">Not: bir Azure web sitesine dağıtıyorsanız, burada bir şey yapmanız gerekmez &mdash; Node.js yüklü olduğundan ve sunucu ortamlarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-137">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="b2fbb-138">[.NET core SDK](https://www.microsoft.com/net/download/core) 1.0 (veya üstü)</span><span class="sxs-lookup"><span data-stu-id="b2fbb-138">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="b2fbb-139">Windows üzerinde değilseniz, bu Visual Studio 2017'ın seçerek yüklenebilir **.NET Core platformlar arası geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-139">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="b2fbb-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="b2fbb-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="b2fbb-141">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="b2fbb-141">Server-side prerendering</span></span>

<span data-ttu-id="b2fbb-142">Sunucu ve istemci üzerinde çalışan hem özellikli bir JavaScript uygulaması, bir evrensel (isomorphic olarak da bilinir) uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-142">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="b2fbb-143">Açısal tepki vermek ve diğer popüler uygulamayı Evrensel bir platform için bu uygulama geliştirme stil sağlayın.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-143">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="b2fbb-144">İlk Node.js aracılığıyla sunucuda framework bileşenlerini oluşturmak ve daha fazla istemciye yürütme temsilci olur.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-144">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="b2fbb-145">ASP.NET Core [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) tarafından sağlanan SpaServices sunucudaki JavaScript işlevlerini çağırarak sunucu tarafı prerendering uyarlamasını basitleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-145">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b2fbb-146">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b2fbb-146">Prerequisites</span></span>

<span data-ttu-id="b2fbb-147">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-147">Install the following:</span></span>
* <span data-ttu-id="b2fbb-148">[ASPNET prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm paket:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-148">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="b2fbb-149">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-149">Configuration</span></span>

<span data-ttu-id="b2fbb-150">Etiket Yardımcıları projesinin üzerinden ad kayıt bulunabilirlik yapılan *_viewımports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-150">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="b2fbb-151">Bu etiket Yardımcıları hemen Razor görünüm içinde HTML benzeri bir sözdizimi yararlanarak alt düzey API'leri ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-151">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="b2fbb-152">`asp-prerender-module` Etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="b2fbb-152">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="b2fbb-153">`asp-prerender-module` Etiket Yardımcısı, önceki kod örneğinde kullanılan yürütür *ClientApp/dist/main-server.js* Node.js aracılığıyla sunucuda.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-153">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="b2fbb-154">Netlik'ın artırmak amacıyla için *ana server.js* dosyasıdır bir yapı içinde TypeScript JavaScript transpilation görevinin [Webpack](http://webpack.github.io/) derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-154">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="b2fbb-155">Webpack tanımlayan bir giriş noktası diğer ad `main-server`; ve bu diğer adı için bağımlılık grafiğinin geçişi başlar *önyükleme/ClientApp-server.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-155">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="b2fbb-156">Aşağıdaki örnekte Açısal, *önyükleme/ClientApp-server.ts* dosya yararlanan `createServerRenderer` işlevi ve `RenderResult` türü `aspnet-prerendering` npm paket Node.js aracılığıyla sunucu işleme yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-156">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="b2fbb-157">Sunucu tarafı işleme kesin türü belirtilmiş bir JavaScript'te kaydırılan bir çözümleme işlev çağrısı için geçirilen hedefleyen HTML biçimlendirmesi `Promise` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-157">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="b2fbb-158">`Promise` Nesnenin anlamlı olduğundan, zaman uyumsuz olarak DOM'ın bir yer tutucu öğeyi yerleştirmeye sayfasına HTML biçimlendirmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-158">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="b2fbb-159">`asp-prerender-data` Etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="b2fbb-159">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="b2fbb-160">İle birlikte zaman `asp-prerender-module` etiket Yardımcısı `asp-prerender-data` etiket Yardımcısı, sunucu tarafı JavaScript Razor görünümünden bağlamsal bilgi geçirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-160">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="b2fbb-161">Örneğin, kullanıcı verilerini aşağıdaki biçimlendirme geçirir `main-server` Modülü:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-161">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="b2fbb-162">Alınan `UserName` bağımsız değişkeni yerleşik JSON seri hale getirici kullanılarak serileştirilmiş ve depolanan `params.data` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-162">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="b2fbb-163">Aşağıdaki Açısal örnekte, veri içinde kişiselleştirilmiş tebrik oluşturmak için kullanılan bir `h1` öğe:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-163">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="b2fbb-164">Not: Etiket Yardımcıları geçirilen özellik adları ile temsil edilen **PascalCase** gösterimi.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-164">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="b2fbb-165">JavaScript, burada aynı özellik adları temsil edilen ile karşılaştırın **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-165">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="b2fbb-166">Varsayılan JSON seri hale getirme yapılandırması için bu fark sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-166">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="b2fbb-167">Önceki kod örneğinde genişletmek için verileri sunucudan görünümüne hydrating tarafından geçirilebilir `globals` özelliği için sağlanan `resolve` işlevi:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-167">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="b2fbb-168">`postList` İçinde tanımlanan dizi `globals` nesnesi bağlı tarayıcının genel `window` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-168">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="b2fbb-169">Bu değişken hoisting genel kapsam için aynı veri kez sunucuda ve istemcide yeniden yükleme için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-169">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Pencere nesnesi bağlı postList genel değişkeni](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="b2fbb-171">Webpack geliştirme Ara</span><span class="sxs-lookup"><span data-stu-id="b2fbb-171">Webpack Dev Middleware</span></span>

<span data-ttu-id="b2fbb-172">[Webpack geliştirme Ara](https://webpack.github.io/docs/webpack-dev-middleware.html) yapabildiği Webpack derlemeler kaynakları isteğe bağlı bir kolaylaştırılmış geliştirme iş akışı sunar.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-172">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="b2fbb-173">Ara yazılım otomatik olarak derler ve sayfa tarayıcıda yeniden yüklendiğinde istemci-tarafı kaynaklar sunar.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-173">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="b2fbb-174">Alternatif bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Webpack projenin npm derleme betiğindeki el ile çağırma yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-174">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="b2fbb-175">Bir npm yapı komut dosyası içinde *package.json* dosya, aşağıdaki örnekte gösterilir:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-175">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="b2fbb-176">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b2fbb-176">Prerequisites</span></span>

<span data-ttu-id="b2fbb-177">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-177">Install the following:</span></span>
* <span data-ttu-id="b2fbb-178">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) npm paket:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-178">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="b2fbb-179">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-179">Configuration</span></span>

<span data-ttu-id="b2fbb-180">Aşağıdaki kodda aracılığıyla HTTP istek ardışık düzenini içine Webpack Dev Ara kayıtlı *haline* dosyanın `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-180">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="b2fbb-181">`UseWebpackDevMiddleware` Genişletme yöntemi çağrılır, önce [statik dosya barındırma kaydetme](xref:fundamentals/static-files) aracılığıyla `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-181">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="b2fbb-182">Yalnızca uygulama geliştirme modunda çalıştığında güvenlik nedenleriyle, ara yazılımın kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-182">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="b2fbb-183">*Webpack.config.js* dosyanın `output.publicPath` özelliği bildirir izlemek için ara yazılım `dist` değişiklikleri için klasör:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-183">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="b2fbb-184">Sık kullanılan modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="b2fbb-184">Hot Module Replacement</span></span>

<span data-ttu-id="b2fbb-185">Webpack'ın düşünün [etkin modülü değiştirme](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) özelliği bir evrimi olarak [Webpack geliştirme Ara](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="b2fbb-185">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="b2fbb-186">HMR hepsi aynı avantajlar sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-186">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="b2fbb-187">Bu geçerli bellek içi durumu ve hata ayıklama oturumu SPA engel tarayıcının bir yenileme ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-187">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="b2fbb-188">Webpack geliştirme Ara hizmeti ve değişiklikleri tarayıcıya gönderilen anlamına gelir tarayıcı arasındaki bir canlı bağlantı yoktur.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-188">There is a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b2fbb-189">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b2fbb-189">Prerequisites</span></span>

<span data-ttu-id="b2fbb-190">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-190">Install the following:</span></span>
* <span data-ttu-id="b2fbb-191">[webpack hot Ara](https://www.npmjs.com/package/webpack-hot-middleware) npm paket:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-191">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="b2fbb-192">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-192">Configuration</span></span>

<span data-ttu-id="b2fbb-193">MVC'ın HTTP istek ardışık düzeninde içine HMR bileşen kaydedilmelidir `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-193">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="b2fbb-194">İle true olarak [Webpack geliştirme Ara](#webpack-dev-middleware), `UseWebpackDevMiddleware` genişletme yöntemi çağrılır, önce `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-194">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="b2fbb-195">Yalnızca uygulama geliştirme modunda çalıştığında güvenlik nedenleriyle, ara yazılımın kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-195">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="b2fbb-196">*Webpack.config.js* dosya tanımlamalıdır bir `plugins` boş bırakılır olsa bile, dizi:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-196">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="b2fbb-197">Tarayıcıda uygulama yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-197">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Sık kullanılan modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="b2fbb-199">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="b2fbb-199">Routing helpers</span></span>

<span data-ttu-id="b2fbb-200">Çoğu ASP.NET Core tabanlı SPAs içinde sunucu tarafı ek olarak istemci tarafı yönlendirmesi isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-200">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="b2fbb-201">SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-201">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="b2fbb-202">Yoktur, ancak, bir sınır durumda zorluklar taşıyor: 404 HTTP yanıtlarını tanımlama.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-202">There is, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="b2fbb-203">Senaryoyu göz önünde bulundurun uzantısız bir yolu `/some/page` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-203">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="b2fbb-204">İstek düzeni-bir sunucu tarafı yol eşleşme değil, ancak bir istemci-tarafı yol kendi desenle eşleşen varsayalım.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-204">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="b2fbb-205">Şimdi bir gelen istek için göz önünde bulundurun `/images/user-512.png`, hangi genellikle bekliyor sunucudaki bir görüntü dosyasını bulmak.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-205">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="b2fbb-206">İstenen kaynak yol herhangi bir sunucu tarafı yol veya statik dosya eşleşmiyorsa, istemci-tarafı uygulamanın bunu işlemesi olası değil — genellikle bir 404 HTTP durum kodu döndürülecek istersiniz.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-206">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b2fbb-207">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b2fbb-207">Prerequisites</span></span>

<span data-ttu-id="b2fbb-208">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-208">Install the following:</span></span>
* <span data-ttu-id="b2fbb-209">İstemci-tarafı yönlendirme npm paket.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-209">The client-side routing npm package.</span></span> <span data-ttu-id="b2fbb-210">Angular örnek olarak kullanarak:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-210">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="b2fbb-211">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-211">Configuration</span></span>

<span data-ttu-id="b2fbb-212">Adlı bir genişletme yöntemi `MapSpaFallbackRoute` kullanılan `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-212">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="b2fbb-213">İpucu: Yollar yapılandırılmış sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-213">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="b2fbb-214">Sonuç olarak, `default` rota önceki kod örneğinde kullanılan ilk desen eşleştirme için.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-214">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="b2fbb-215">Yeni proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-215">Creating a new project</span></span>

<span data-ttu-id="b2fbb-216">JavaScriptServices önceden yapılandırılmış uygulama şablonları sağlar.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-216">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="b2fbb-217">SpaServices bu şablonları farklı çerçeveler ve kitaplıklar Angular, Aurelia, çakıştırma, tepki ve Vue gibi ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-217">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="b2fbb-218">Bu şablonlar, aşağıdaki komutu çalıştırarak .NET Core CLI yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-218">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="b2fbb-219">Kullanılabilir SPA şablonların listesi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-219">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="b2fbb-220">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="b2fbb-220">Templates</span></span>                                 | <span data-ttu-id="b2fbb-221">Kısa Ad</span><span class="sxs-lookup"><span data-stu-id="b2fbb-221">Short Name</span></span> | <span data-ttu-id="b2fbb-222">Dil</span><span class="sxs-lookup"><span data-stu-id="b2fbb-222">Language</span></span> | <span data-ttu-id="b2fbb-223">Etiketler</span><span class="sxs-lookup"><span data-stu-id="b2fbb-223">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="b2fbb-224">MVC ASP.NET Core Açısal ile</span><span class="sxs-lookup"><span data-stu-id="b2fbb-224">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="b2fbb-225">Açısal</span><span class="sxs-lookup"><span data-stu-id="b2fbb-225">angular</span></span>    | <span data-ttu-id="b2fbb-226">[C#]</span><span class="sxs-lookup"><span data-stu-id="b2fbb-226">[C#]</span></span>     | <span data-ttu-id="b2fbb-227">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b2fbb-227">Web/MVC/SPA</span></span> |
| <span data-ttu-id="b2fbb-228">MVC ASP.NET Core Aurelia ile</span><span class="sxs-lookup"><span data-stu-id="b2fbb-228">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="b2fbb-229">aurelia</span><span class="sxs-lookup"><span data-stu-id="b2fbb-229">aurelia</span></span>    | <span data-ttu-id="b2fbb-230">[C#]</span><span class="sxs-lookup"><span data-stu-id="b2fbb-230">[C#]</span></span>     | <span data-ttu-id="b2fbb-231">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b2fbb-231">Web/MVC/SPA</span></span> |
| <span data-ttu-id="b2fbb-232">MVC ASP.NET Core Knockout.js ile</span><span class="sxs-lookup"><span data-stu-id="b2fbb-232">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="b2fbb-233">Boşaltılan</span><span class="sxs-lookup"><span data-stu-id="b2fbb-233">knockout</span></span>   | <span data-ttu-id="b2fbb-234">[C#]</span><span class="sxs-lookup"><span data-stu-id="b2fbb-234">[C#]</span></span>     | <span data-ttu-id="b2fbb-235">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b2fbb-235">Web/MVC/SPA</span></span> |
| <span data-ttu-id="b2fbb-236">MVC ASP.NET Core React.js ile</span><span class="sxs-lookup"><span data-stu-id="b2fbb-236">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="b2fbb-237">tepki</span><span class="sxs-lookup"><span data-stu-id="b2fbb-237">react</span></span>      | <span data-ttu-id="b2fbb-238">[C#]</span><span class="sxs-lookup"><span data-stu-id="b2fbb-238">[C#]</span></span>     | <span data-ttu-id="b2fbb-239">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b2fbb-239">Web/MVC/SPA</span></span> |
| <span data-ttu-id="b2fbb-240">MVC ASP.NET Core React.js ve Redux</span><span class="sxs-lookup"><span data-stu-id="b2fbb-240">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="b2fbb-241">reactredux</span><span class="sxs-lookup"><span data-stu-id="b2fbb-241">reactredux</span></span> | <span data-ttu-id="b2fbb-242">[C#]</span><span class="sxs-lookup"><span data-stu-id="b2fbb-242">[C#]</span></span>     | <span data-ttu-id="b2fbb-243">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b2fbb-243">Web/MVC/SPA</span></span> |
| <span data-ttu-id="b2fbb-244">MVC ASP.NET Core Vue.js ile</span><span class="sxs-lookup"><span data-stu-id="b2fbb-244">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="b2fbb-245">VUE</span><span class="sxs-lookup"><span data-stu-id="b2fbb-245">vue</span></span>        | <span data-ttu-id="b2fbb-246">[C#]</span><span class="sxs-lookup"><span data-stu-id="b2fbb-246">[C#]</span></span>     | <span data-ttu-id="b2fbb-247">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b2fbb-247">Web/MVC/SPA</span></span> | 

<span data-ttu-id="b2fbb-248">SPA şablonlarından birini kullanarak yeni bir proje oluşturmak için içermesi **kısa ad** şablonunun `dotnet new` komutu.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-248">To create a new project using one of the SPA templates, include the **Short Name** of the template in the `dotnet new` command.</span></span> <span data-ttu-id="b2fbb-249">Aşağıdaki komutu Açısal uygulama ASP.NET Core için sunucu tarafı yapılandırılmış MVC ile oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-249">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="b2fbb-250">Çalışma zamanı yapılandırma modunu ayarlama</span><span class="sxs-lookup"><span data-stu-id="b2fbb-250">Set the runtime configuration mode</span></span>

<span data-ttu-id="b2fbb-251">İki birincil çalışma zamanı yapılandırma modlarından mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-251">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="b2fbb-252">**Geliştirme**:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-252">**Development**:</span></span>
    * <span data-ttu-id="b2fbb-253">Hata ayıklama kolaylaştırmak için kaynak eşlemeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-253">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="b2fbb-254">İstemci tarafı kodlar performansı en iyi duruma değil.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-254">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="b2fbb-255">**Üretim**:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-255">**Production**:</span></span>
    * <span data-ttu-id="b2fbb-256">Kaynak eşlemeleri dışlar.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-256">Excludes source maps.</span></span>
    * <span data-ttu-id="b2fbb-257">İstemci tarafı kodu paketleme ve küçültme aracılığıyla en iyi duruma getirir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-257">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="b2fbb-258">ASP.NET Core adlı bir ortam değişkeni kullanır `ASPNETCORE_ENVIRONMENT` yapılandırma modu depolamak için.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-258">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="b2fbb-259">Bkz:  **[ortamını](xref:fundamentals/environments#setting-the-environment)**  daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-259">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="b2fbb-260">.NET Core CLI ile çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-260">Running with .NET Core CLI</span></span>

<span data-ttu-id="b2fbb-261">Npm paketleri ve gerekli NuGet proje kök dizininde aşağıdaki komutu çalıştırarak geri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-261">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="b2fbb-262">Derleme ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-262">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="b2fbb-263">Localhost göre uygulama başlangıç [çalışma zamanı yapılandırma modu](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="b2fbb-263">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="b2fbb-264">Gezinme `http://localhost:5000` giriş sayfasını tarayıcıda görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-264">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="b2fbb-265">Visual Studio 2017 çalışır</span><span class="sxs-lookup"><span data-stu-id="b2fbb-265">Running with Visual Studio 2017</span></span>

<span data-ttu-id="b2fbb-266">Açık *.csproj* tarafından oluşturulan dosya `dotnet new` komutu.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-266">Open the *.csproj* file generated by the `dotnet new` command.</span></span> <span data-ttu-id="b2fbb-267">Gerekli NuGet ve npm'yi paketler proje üzerinde otomatik olarak geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-267">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="b2fbb-268">Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırılmaya hazır bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-268">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="b2fbb-269">Yeşil düğmesine veya tuşuna tıklayın `Ctrl + F5`, ve tarayıcıda uygulama giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-269">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="b2fbb-270">Uygulama göre localhost üzerinde çalışan [çalışma zamanı yapılandırma modu](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="b2fbb-270">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="b2fbb-271">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="b2fbb-271">Testing the app</span></span>

<span data-ttu-id="b2fbb-272">SpaServices şablonları kullanarak istemci tarafı testleri çalıştırmak için önceden yapılandırılmış [Karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="b2fbb-272">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="b2fbb-273">Karma bu testler için test Çalıştırıcısı iken jasmine framework JavaScript için test popüler bir birimi değil.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-273">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="b2fbb-274">Karma çalışmak üzere yapılandırılmış [Webpack geliştirme Ara](#webpack-dev-middleware) sağlayacak şekilde durdurmak ve değişiklikler yapılmadan her zaman testi çalıştırmak zorunda değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-274">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that you don’t have to stop and run the test every time changes are made.</span></span> <span data-ttu-id="b2fbb-275">Test çalışması ya da test çalışması karşı çalışan kod olup olmadığını belirtir, test otomatik olarak çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-275">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="b2fbb-276">Açısal uygulama örnek olarak kullanarak, iki Jasmine test çalışmaları zaten için sağlanan `CounterComponent` içinde *counter.component.spec.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-276">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="b2fbb-277">Proje kökündeki komut istemi açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-277">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="b2fbb-278">Tanımlanan ayarları okur Karma test Çalıştırıcısı komut dosyasını başlatır *karma.conf.js* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-278">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="b2fbb-279">Diğer ayarları arasında *karma.conf.js* aracılığıyla yürütülecek test dosyaları tanımlar, `files` dizisi:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-279">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="b2fbb-280">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="b2fbb-280">Publishing the application</span></span>

<span data-ttu-id="b2fbb-281">Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştirme sıkıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="b2fbb-281">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="b2fbb-282">Thankfully, tüm yayın işlem SpaServices adlı özel bir MSBuild hedef ile düzenler `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-282">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="b2fbb-283">MSBuild hedef aşağıdaki sorumlulukları vardır:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-283">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="b2fbb-284">Npm paket geri yükleme</span><span class="sxs-lookup"><span data-stu-id="b2fbb-284">Restore the npm packages</span></span>
1. <span data-ttu-id="b2fbb-285">Bir üretim düzeyde yapı ve üçüncü taraf, istemci-tarafı varlıkları oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-285">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="b2fbb-286">Bir üretim düzeyde yapı ve özel istemci-tarafı varlıkları oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2fbb-286">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="b2fbb-287">Webpack oluşturulan varlıkları yayımla klasörüne kopyalayın</span><span class="sxs-lookup"><span data-stu-id="b2fbb-287">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="b2fbb-288">MSBuild hedef çalıştırırken çağrılır:</span><span class="sxs-lookup"><span data-stu-id="b2fbb-288">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="b2fbb-289">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b2fbb-289">Additional resources</span></span>

* [<span data-ttu-id="b2fbb-290">Açısal belgeleri</span><span class="sxs-lookup"><span data-stu-id="b2fbb-290">Angular Docs</span></span>](https://angular.io/docs)
