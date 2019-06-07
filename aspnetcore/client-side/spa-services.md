---
title: ASP.NET Core tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanma
author: scottaddie
description: Bir tek sayfa uygulaması (ASP.NET Core tarafından desteklenen SPA) oluşturmak için JavaScript Hizmetleri kullanmanın avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 05/28/2019
uid: client-side/spa-services
ms.openlocfilehash: c7cd35865c5bddf0e5efaa9e616832b6755d9227
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750115"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="7eedf-103">ASP.NET Core tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="7eedf-103">Use JavaScript Services to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="7eedf-104">Tarafından [Scott Addie](https://github.com/scottaddie) ve [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="7eedf-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="7eedf-105">Tek sayfa uygulama (SPA) web uygulamasının kendi devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türdür.</span><span class="sxs-lookup"><span data-stu-id="7eedf-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="7eedf-106">İstemci tarafı SPA çerçeveleri veya kitaplıkları gibi tümleştirme [Angular](https://angular.io/) veya [React](https://facebook.github.io/react/), ASP.NET Core gibi sunucu tarafı çerçevelerle zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks such as ASP.NET Core can be difficult.</span></span> <span data-ttu-id="7eedf-107">JavaScript Hizmetleri Tümleştirme sürecindeki uyuşmazlıkları azaltmak için geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-107">JavaScript Services was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="7eedf-108">Ancak, farklı istemci ve sunucu teknoloji yığınları arasında sorunsuz bir işlemi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-108">It enables seamless operation between the different client and server technology stacks.</span></span>

## <a name="what-is-javascript-services"></a><span data-ttu-id="7eedf-109">JavaScript Hizmetleri nedir</span><span class="sxs-lookup"><span data-stu-id="7eedf-109">What is JavaScript Services</span></span>

<span data-ttu-id="7eedf-110">JavaScript Hizmetleri, ASP.NET Core için istemci tarafı teknolojilerinin koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="7eedf-110">JavaScript Services is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="7eedf-111">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için hedefi sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="7eedf-112">JavaScript Hizmetleri iki ayrı NuGet paketlerini oluşur:</span><span class="sxs-lookup"><span data-stu-id="7eedf-112">JavaScript Services consists of two distinct NuGet packages:</span></span>

* <span data-ttu-id="7eedf-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="7eedf-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="7eedf-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="7eedf-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>

<span data-ttu-id="7eedf-115">Bu paketler aşağıdaki senaryolarda kullanışlıdır:</span><span class="sxs-lookup"><span data-stu-id="7eedf-115">These packages are useful in the following scenarios:</span></span>

* <span data-ttu-id="7eedf-116">Sunucu üzerinde JavaScript çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7eedf-116">Run JavaScript on the server</span></span>
* <span data-ttu-id="7eedf-117">SPA altyapı veya kitaplığı kullanın</span><span class="sxs-lookup"><span data-stu-id="7eedf-117">Use a SPA framework or library</span></span>
* <span data-ttu-id="7eedf-118">İstemci tarafı Web varlıklarla oluşturun</span><span class="sxs-lookup"><span data-stu-id="7eedf-118">Build client-side assets with Webpack</span></span>

<span data-ttu-id="7eedf-119">Bu makalenin odak çoğunu SpaServices paketini kullanarak yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-119">Much of the focus in this article is placed on using the SpaServices package.</span></span>

## <a name="what-is-spaservices"></a><span data-ttu-id="7eedf-120">SpaServices nedir</span><span class="sxs-lookup"><span data-stu-id="7eedf-120">What is SpaServices</span></span>

<span data-ttu-id="7eedf-121">ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için SpaServices oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7eedf-121">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="7eedf-122">SpaServices Spa'lar ASP.NET Core ile geliştirmek için gerekli değildir ve belirli istemci altyapısına geliştiriciler kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="7eedf-122">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock developers into a particular client framework.</span></span>

<span data-ttu-id="7eedf-123">SpaServices gibi kullanışlı bir altyapı sağlar:</span><span class="sxs-lookup"><span data-stu-id="7eedf-123">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="7eedf-124">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="7eedf-124">Server-side prerendering</span></span>](#server-side-prerendering)
* [<span data-ttu-id="7eedf-125">Web geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="7eedf-125">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="7eedf-126">Sık erişimli modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="7eedf-126">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="7eedf-127">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="7eedf-127">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="7eedf-128">Toplu olarak, bu altyapı bileşenlerini, hem geliştirme iş akışını hem de çalışma zamanı deneyimi geliştirin.</span><span class="sxs-lookup"><span data-stu-id="7eedf-128">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="7eedf-129">Bileşenleri ayrı ayrı önemsenmeksizin devralınabilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-129">The components can be adopted individually.</span></span>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="7eedf-130">SpaServices kullanmanın önkoşulları</span><span class="sxs-lookup"><span data-stu-id="7eedf-130">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="7eedf-131">SpaServices ile çalışmak için aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="7eedf-131">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="7eedf-132">[Node.js](https://nodejs.org/) (sürüm 6 veya sonrası) npm ile</span><span class="sxs-lookup"><span data-stu-id="7eedf-132">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>

  * <span data-ttu-id="7eedf-133">Bu bileşenler yüklenir ve bulunabilir doğrulamak için komut satırından aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7eedf-133">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

  * <span data-ttu-id="7eedf-134">Bir Azure web sitesine dağıtma, Eylem gerekmiyor&mdash;Node.js yüklü olduğundan ve sunucu ortamlarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-134">If deploying to an Azure web site, no action is required&mdash;Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="7eedf-135">Visual Studio 2017'yi kullanarak Windows üzerinde seçerek SDK'sı yüklü **.NET Core çoklu platform geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="7eedf-135">On Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="7eedf-136">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="7eedf-136">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

## <a name="server-side-prerendering"></a><span data-ttu-id="7eedf-137">Sunucu tarafı prerendering</span><span class="sxs-lookup"><span data-stu-id="7eedf-137">Server-side prerendering</span></span>

<span data-ttu-id="7eedf-138">Bir evrensel (isomorphic olarak da bilinir) uygulama özellikli olan hem çalışan sunucu ve istemci üzerindeki bir JavaScript uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-138">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="7eedf-139">Angular, React ve diğer popüler çerçeveleri için bu uygulama geliştirme stili Evrensel bir platform sağlar.</span><span class="sxs-lookup"><span data-stu-id="7eedf-139">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="7eedf-140">İlk Node.js aracılığıyla sunucuda framework bileşenleri oluşturma ve yürütme istemciye daha fazla temsilci olur.</span><span class="sxs-lookup"><span data-stu-id="7eedf-140">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="7eedf-141">ASP.NET Core [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) tarafından sağlanan SpaServices server JavaScript işlevleri çağrılarak sunucu tarafı prerendering uygulamasını basitleştirin.</span><span class="sxs-lookup"><span data-stu-id="7eedf-141">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="server-side-prerendering-prerequisites"></a><span data-ttu-id="7eedf-142">Sunucu tarafı prerendering önkoşulları</span><span class="sxs-lookup"><span data-stu-id="7eedf-142">Server-side prerendering prerequisites</span></span>

<span data-ttu-id="7eedf-143">Yükleme [aspnet prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-143">Install the [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a><span data-ttu-id="7eedf-144">Sunucu tarafı prerendering yapılandırması</span><span class="sxs-lookup"><span data-stu-id="7eedf-144">Server-side prerendering configuration</span></span>

<span data-ttu-id="7eedf-145">Etiket Yardımcıları projesinin ad kayıt bulunabilir yapılan *_viewımports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7eedf-145">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="7eedf-146">Bu etiket Yardımcıları hemen bir HTML benzeri sözdizimi Razor görünüm içinde yararlanarak alt düzey API'ler ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:</span><span class="sxs-lookup"><span data-stu-id="7eedf-146">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a><span data-ttu-id="7eedf-147">ASP prerender modülü etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="7eedf-147">asp-prerender-module Tag Helper</span></span>

<span data-ttu-id="7eedf-148">`asp-prerender-module` Etiketi Yardımcısı, önceki kod örneğinde kullanılan yürütür *ClientApp/dist/main-server.js* Node.js aracılığıyla sunucuda.</span><span class="sxs-lookup"><span data-stu-id="7eedf-148">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="7eedf-149">Açıklık'ın çok için *ana server.js* dosyasıdır TypeScript JavaScript transpilation görevin bir yapıt [Web](http://webpack.github.io/) derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="7eedf-149">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="7eedf-150">Web tanımlayan bir giriş noktası diğer `main-server`; ve bu diğer adı için bağımlılık grafiği geçişini başlar *ClientApp/önyükleme-server.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7eedf-150">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="7eedf-151">Aşağıdaki Angular örnekte *ClientApp/önyükleme-server.ts* dosya kullanır `createServerRenderer` işlevi ve `RenderResult` tür `aspnet-prerendering` npm paketi, sunucu işleme Node.js aracılığıyla yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-151">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="7eedf-152">Kesin türü belirtilmiş bir JavaScript içinde sarmalanmış bir Çözümle işlev çağrısı için sunucu tarafı işleme geçirilen hedefleyen bir HTML biçimlendirmesi `Promise` nesne.</span><span class="sxs-lookup"><span data-stu-id="7eedf-152">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="7eedf-153">`Promise` Nesnenin önemi olan, zaman uyumsuz olarak DOM'ın yer tutucu öğesi yerleştirmeye sayfasına HTML biçimlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="7eedf-153">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a><span data-ttu-id="7eedf-154">ASP prerender veri etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="7eedf-154">asp-prerender-data Tag Helper</span></span>

<span data-ttu-id="7eedf-155">İle birlikte zaman `asp-prerender-module` etiketi Yardımcısı `asp-prerender-data` etiketi Yardımcısı, bağlamsal bilgiler için sunucu tarafı JavaScript Razor görünümden geçirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-155">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="7eedf-156">Örneğin, kullanıcı verilerini aşağıdaki biçimlendirme geçirir `main-server` Modülü:</span><span class="sxs-lookup"><span data-stu-id="7eedf-156">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="7eedf-157">Alınan `UserName` bağımsız değişkeni yerleşik JSON serileştirici kullanılarak serileştirilmiş ve depolanan `params.data` nesne.</span><span class="sxs-lookup"><span data-stu-id="7eedf-157">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="7eedf-158">Angular aşağıdaki örnekte veri içinde kişiselleştirilmiş bir karşılama oluşturmak için kullanılan bir `h1` öğesi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-158">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="7eedf-159">Etiket Yardımcıları içinde geçirilen özellik adları ile temsil edilir **PascalCase** gösterimi.</span><span class="sxs-lookup"><span data-stu-id="7eedf-159">Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="7eedf-160">Burada aynı özellik adları ile gösterilir, JavaScript, Karşıtlık **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="7eedf-160">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="7eedf-161">Bu fark için varsayılan JSON seri hale getirme yapılandırması sorumludur.</span><span class="sxs-lookup"><span data-stu-id="7eedf-161">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="7eedf-162">Yukarıdaki kod örneğinde genişletmek için verileri sunucudan görünüme hydrating tarafından geçirilebilir `globals` özelliği için sağlanan `resolve` işlevi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-162">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="7eedf-163">`postList` İçinde tanımlanan bir dizi `globals` nesne bağlı tarayıcının genel `window` nesne.</span><span class="sxs-lookup"><span data-stu-id="7eedf-163">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="7eedf-164">Bu değişken hoisting genel kapsam için aynı verileri bir kez sunucuda ve istemcinin yeniden yüklenmesi için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-164">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Pencere nesnesi bağlı genel postList değişkeni](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a><span data-ttu-id="7eedf-166">Web geliştirme ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="7eedf-166">Webpack Dev Middleware</span></span>

<span data-ttu-id="7eedf-167">[Web geliştirme ara yazılım](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) yapabildiği Web yapılar kaynakları isteğe bağlı olarak Basitleştirilmiş geliştirme iş akışı sunar.</span><span class="sxs-lookup"><span data-stu-id="7eedf-167">[Webpack Dev Middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="7eedf-168">Ara yazılım, otomatik olarak derler ve bir sayfa tarayıcıya yeniden yüklendiğinde istemci-tarafı kaynaklar sunar.</span><span class="sxs-lookup"><span data-stu-id="7eedf-168">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="7eedf-169">Başka bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Web projenin npm derleme betiği aracılığıyla el ile başlatmak için yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-169">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="7eedf-170">Bir npm derleme betiği *package.json* dosya, aşağıdaki örnekte gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7eedf-170">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a><span data-ttu-id="7eedf-171">Web geliştirme ara yazılım önkoşulları</span><span class="sxs-lookup"><span data-stu-id="7eedf-171">Webpack Dev Middleware prerequisites</span></span>

<span data-ttu-id="7eedf-172">Yükleme [ASP.NET Web](https://www.npmjs.com/package/aspnet-webpack) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-172">Install the [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a><span data-ttu-id="7eedf-173">Web geliştirme ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7eedf-173">Webpack Dev Middleware configuration</span></span>

<span data-ttu-id="7eedf-174">HTTP istek işlem hattı aşağıdaki kod aracılığıyla içine kayıtlı Web geliştirme ara yazılım *Startup.cs* dosyanın `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-174">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="7eedf-175">`UseWebpackDevMiddleware` Genişletme yöntemi çağrıldığında, önce [statik dosya barındırma kaydetme](xref:fundamentals/static-files) aracılığıyla `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7eedf-175">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="7eedf-176">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="7eedf-176">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="7eedf-177">*Webpack.config.js* dosyanın `output.publicPath` özelliği söyler izlemek için bir ara yazılım `dist` klasördeki değişiklikleri:</span><span class="sxs-lookup"><span data-stu-id="7eedf-177">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a><span data-ttu-id="7eedf-178">Sık erişimli modülü değiştirme</span><span class="sxs-lookup"><span data-stu-id="7eedf-178">Hot Module Replacement</span></span>

<span data-ttu-id="7eedf-179">Web'ın düşünebilirsiniz [sık erişimli modülü değiştirme](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) özelliği'nın Gelişmiş hali olarak [Web geliştirme ara yazılım](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="7eedf-179">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="7eedf-180">HMR aynı avantajları sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-180">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="7eedf-181">Bu, geçerli bellek içi durumu ve hata ayıklama oturumu SPA ile neden tarayıcı yenileme ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="7eedf-181">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="7eedf-182">Web geliştirme ara yazılım hizmeti ve değişiklikleri tarayıcıya gönderilmesini anlamına gelir tarayıcı arasında canlı bir bağlantı yoktur.</span><span class="sxs-lookup"><span data-stu-id="7eedf-182">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="hot-module-replacement-prerequisites"></a><span data-ttu-id="7eedf-183">Sık erişimli modülü değiştirme önkoşulları</span><span class="sxs-lookup"><span data-stu-id="7eedf-183">Hot Module Replacement prerequisites</span></span>

<span data-ttu-id="7eedf-184">Yükleme [Web hot Ara](https://www.npmjs.com/package/webpack-hot-middleware) npm paketi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-184">Install the [webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a><span data-ttu-id="7eedf-185">Sık erişimli modülü değiştirme yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7eedf-185">Hot Module Replacement configuration</span></span>

<span data-ttu-id="7eedf-186">HMR bileşen MVC'nin HTTP istek işlem hattı, kaydedilmesi gerekir `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-186">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="7eedf-187">İle doğru olarak [Web geliştirme ara yazılım](#webpack-dev-middleware), `UseWebpackDevMiddleware` genişletme yöntemi çağrıldığında, önce `UseStaticFiles` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7eedf-187">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="7eedf-188">Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.</span><span class="sxs-lookup"><span data-stu-id="7eedf-188">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="7eedf-189">*Webpack.config.js* dosya tanımlamalıdır bir `plugins` dizi bile boş bırakılır:</span><span class="sxs-lookup"><span data-stu-id="7eedf-189">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="7eedf-190">Uygulamayı tarayıcıda yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="7eedf-190">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Sık erişimli modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a><span data-ttu-id="7eedf-192">Yönlendirme Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="7eedf-192">Routing helpers</span></span>

<span data-ttu-id="7eedf-193">Çoğu ASP.NET Core tabanlı Spa'lar içinde istemci tarafı yönlendirmesi genellikle sunucu tarafı yönlendirmenin yanı sıra istenir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-193">In most ASP.NET Core-based SPAs, client-side routing is often desired in addition to server-side routing.</span></span> <span data-ttu-id="7eedf-194">SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-194">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="7eedf-195">Yoktur, ancak bir kenar büyük/küçük harf taşıyor sorunlarını: HTTP 404 yanıtları tanımlayan.</span><span class="sxs-lookup"><span data-stu-id="7eedf-195">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="7eedf-196">Senaryoyu göz önünde bulundurun uzantısız bir yolu `/some/page` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-196">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="7eedf-197">İstek deseni-sunucu tarafı rota match değil, ancak desenine bir istemci-tarafı rota ile eşleşmekte varsayılır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-197">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="7eedf-198">Şimdi gelen bir istek için göz önünde bulundurun `/images/user-512.png`, hangi genellikle bekliyor sunucudaki bir görüntü dosyası bulunamıyor.</span><span class="sxs-lookup"><span data-stu-id="7eedf-198">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="7eedf-199">Bu istenen kaynak yolu herhangi bir sunucu tarafı rota veya statik dosya eşleşmiyorsa, istemci-tarafı uygulaması, işlemek olası&mdash;istenen genellikle bir 404 HTTP durum kodu döndürerek.</span><span class="sxs-lookup"><span data-stu-id="7eedf-199">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it&mdash;generally returning a 404 HTTP status code is desired.</span></span>

### <a name="routing-helpers-prerequisites"></a><span data-ttu-id="7eedf-200">Yönlendirme Yardımcıları önkoşulları</span><span class="sxs-lookup"><span data-stu-id="7eedf-200">Routing helpers prerequisites</span></span>

<span data-ttu-id="7eedf-201">İstemci tarafı yönlendirme npm paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7eedf-201">Install the client-side routing npm package.</span></span> <span data-ttu-id="7eedf-202">Angular örnek olarak kullanıp:</span><span class="sxs-lookup"><span data-stu-id="7eedf-202">Using Angular as an example:</span></span>

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a><span data-ttu-id="7eedf-203">Yönlendirme Yardımcıları yapılandırması</span><span class="sxs-lookup"><span data-stu-id="7eedf-203">Routing helpers configuration</span></span>

<span data-ttu-id="7eedf-204">Adlı bir genişletme yöntemi `MapSpaFallbackRoute` kullanılır `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-204">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="7eedf-205">Rotalar yapılandırılmış sırada değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-205">Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="7eedf-206">Sonuç olarak, `default` rota önceki kod örneğinde kullanılan ilk desen eşleştirmesi için.</span><span class="sxs-lookup"><span data-stu-id="7eedf-206">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="7eedf-207">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="7eedf-207">Create a new project</span></span>

<span data-ttu-id="7eedf-208">Önceden yapılandırılmış uygulama şablonları JavaScript hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="7eedf-208">JavaScript Services provide pre-configured application templates.</span></span> <span data-ttu-id="7eedf-209">SpaServices bu şablonları farklı çerçeveler ve kitaplıklar gibi Angular, React ve Redux ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-209">SpaServices is used in these templates in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="7eedf-210">Bu şablonlar, aşağıdaki komutu çalıştırarak, .NET Core CLI yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="7eedf-210">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="7eedf-211">Kullanılabilir SPA şablonlar listesi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="7eedf-211">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="7eedf-212">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="7eedf-212">Templates</span></span>                                 | <span data-ttu-id="7eedf-213">Kısa Ad</span><span class="sxs-lookup"><span data-stu-id="7eedf-213">Short Name</span></span> | <span data-ttu-id="7eedf-214">Dil</span><span class="sxs-lookup"><span data-stu-id="7eedf-214">Language</span></span> | <span data-ttu-id="7eedf-215">Etiketler</span><span class="sxs-lookup"><span data-stu-id="7eedf-215">Tags</span></span>        |
| ------------------------------------------| :--------: | :------: | :---------: |
| <span data-ttu-id="7eedf-216">Angular ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7eedf-216">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="7eedf-217">Angular</span><span class="sxs-lookup"><span data-stu-id="7eedf-217">angular</span></span>    | <span data-ttu-id="7eedf-218">[C#]</span><span class="sxs-lookup"><span data-stu-id="7eedf-218">[C#]</span></span>     | <span data-ttu-id="7eedf-219">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="7eedf-219">Web/MVC/SPA</span></span> |
| <span data-ttu-id="7eedf-220">React.js ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7eedf-220">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="7eedf-221">react</span><span class="sxs-lookup"><span data-stu-id="7eedf-221">react</span></span>      | <span data-ttu-id="7eedf-222">[C#]</span><span class="sxs-lookup"><span data-stu-id="7eedf-222">[C#]</span></span>     | <span data-ttu-id="7eedf-223">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="7eedf-223">Web/MVC/SPA</span></span> |
| <span data-ttu-id="7eedf-224">ASP.NET Core MVC React.js ve Redux</span><span class="sxs-lookup"><span data-stu-id="7eedf-224">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="7eedf-225">açarken kilitlenmesi</span><span class="sxs-lookup"><span data-stu-id="7eedf-225">reactredux</span></span> | <span data-ttu-id="7eedf-226">[C#]</span><span class="sxs-lookup"><span data-stu-id="7eedf-226">[C#]</span></span>     | <span data-ttu-id="7eedf-227">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="7eedf-227">Web/MVC/SPA</span></span> |

<span data-ttu-id="7eedf-228">SPA şablonlardan birini kullanarak yeni bir proje oluşturmak için dahil **kısa ad** şablonda, [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu.</span><span class="sxs-lookup"><span data-stu-id="7eedf-228">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="7eedf-229">Aşağıdaki komut, sunucu tarafı için yapılandırılan ASP.NET Core MVC ile Angular bir uygulama oluşturur:</span><span class="sxs-lookup"><span data-stu-id="7eedf-229">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="7eedf-230">Çalışma zamanı yapılandırma modunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7eedf-230">Set the runtime configuration mode</span></span>

<span data-ttu-id="7eedf-231">Birincil çalışma zamanı yapılandırma için iki mod vardır:</span><span class="sxs-lookup"><span data-stu-id="7eedf-231">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="7eedf-232">**Geliştirme**:</span><span class="sxs-lookup"><span data-stu-id="7eedf-232">**Development**:</span></span>
  * <span data-ttu-id="7eedf-233">Hata ayıklamayı kolaylaştırmak için kaynak eşlemeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-233">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="7eedf-234">İstemci tarafı kod performans için en iyi duruma değil.</span><span class="sxs-lookup"><span data-stu-id="7eedf-234">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="7eedf-235">**Üretim**:</span><span class="sxs-lookup"><span data-stu-id="7eedf-235">**Production**:</span></span>
  * <span data-ttu-id="7eedf-236">Kaynak eşlemeleri dışlar.</span><span class="sxs-lookup"><span data-stu-id="7eedf-236">Excludes source maps.</span></span>
  * <span data-ttu-id="7eedf-237">İstemci tarafı kod paketleme ve küçültme ile en iyi duruma getirir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-237">Optimizes the client-side code via bundling and minification.</span></span>

<span data-ttu-id="7eedf-238">ASP.NET Core kullanan adlı bir ortam değişkeni `ASPNETCORE_ENVIRONMENT` yapılandırma modunu depolamak için.</span><span class="sxs-lookup"><span data-stu-id="7eedf-238">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="7eedf-239">Daha fazla bilgi için [ortamı](xref:fundamentals/environments#set-the-environment).</span><span class="sxs-lookup"><span data-stu-id="7eedf-239">For more information, see [Set the environment](xref:fundamentals/environments#set-the-environment).</span></span>

### <a name="run-with-net-core-cli"></a><span data-ttu-id="7eedf-240">.NET Core CLI çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7eedf-240">Run with .NET Core CLI</span></span>

<span data-ttu-id="7eedf-241">Proje kök dizininde aşağıdaki komutu çalıştırarak gerekli NuGet ve npm paketlerini geri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="7eedf-241">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="7eedf-242">Derleme ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7eedf-242">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="7eedf-243">Şunlara göre localhost üzerinde uygulama başladığında [çalışma zamanı yapılandırma modunu](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="7eedf-243">The application starts on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span> <span data-ttu-id="7eedf-244">Gezinme `http://localhost:5000` tarayıcısında giriş sayfası görüntüler.</span><span class="sxs-lookup"><span data-stu-id="7eedf-244">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="run-with-visual-studio-2017"></a><span data-ttu-id="7eedf-245">Visual Studio 2017 ile çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7eedf-245">Run with Visual Studio 2017</span></span>

<span data-ttu-id="7eedf-246">Açık *.csproj* tarafından oluşturulan dosya [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu.</span><span class="sxs-lookup"><span data-stu-id="7eedf-246">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="7eedf-247">Gerekli NuGet ve npm paketleri proje üzerinde otomatik olarak geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-247">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="7eedf-248">Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırmaya hazır bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-248">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="7eedf-249">Yeşil çalıştırma düğmesine veya tuşuna tıklayın `Ctrl + F5`, ve tarayıcıda uygulama giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-249">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="7eedf-250">Uygulama ayarına göre localhost üzerinde çalışır [çalışma zamanı yapılandırma modunu](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="7eedf-250">The application runs on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span>

## <a name="test-the-app"></a><span data-ttu-id="7eedf-251">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="7eedf-251">Test the app</span></span>

<span data-ttu-id="7eedf-252">SpaServices şablonları kullanarak istemci tarafı testleri çalıştırmak için önceden yapılandırılmış [Karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="7eedf-252">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="7eedf-253">Test Çalıştırıcısı bu testler için karma bilgileriyse jasmine bir popüler birim testi çerçevesi için JavaScript, ' dir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-253">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="7eedf-254">Karma çalışmak üzere yapılandırılmış [Web geliştirme ara yazılım](#webpack-dev-middleware) sağlayacak şekilde Geliştirici durdurmak ve değişiklikler her zaman test çalıştırması için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-254">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="7eedf-255">Test çalışması veya test çalışması karşı çalışan kod olup olmadığını test otomatik olarak çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7eedf-255">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="7eedf-256">Angular uygulamasını örnek olarak kullanıp, Jasmine iki test çalışmaları zaten için sağlanan `CounterComponent` içinde *counter.component.spec.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7eedf-256">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="7eedf-257">Komut istemi açın *ClientApp* dizin.</span><span class="sxs-lookup"><span data-stu-id="7eedf-257">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="7eedf-258">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7eedf-258">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="7eedf-259">Komut dosyası içinde tanımlanan ayarlarını okur Karma test Çalıştırıcısı başlatır *karma.conf.js* dosya.</span><span class="sxs-lookup"><span data-stu-id="7eedf-259">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="7eedf-260">Diğer ayarlar arasında *karma.conf.js* aracılığıyla yürütülecek test dosyalarını tanımlar, `files` dizi:</span><span class="sxs-lookup"><span data-stu-id="7eedf-260">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a><span data-ttu-id="7eedf-261">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="7eedf-261">Publish the app</span></span>

<span data-ttu-id="7eedf-262">Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştiren çok uğraşmayı gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="7eedf-262">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="7eedf-263">Ne SpaServices adlı özel bir MSBuild hedefi ile tüm yayın işlem düzenler `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="7eedf-263">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="7eedf-264">MSBuild hedefi aşağıdaki sorumluluklara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="7eedf-264">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="7eedf-265">Npm paketlerini geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7eedf-265">Restore the npm packages.</span></span>
1. <span data-ttu-id="7eedf-266">Bir üretim sınıfı build üçüncü taraf, istemci tarafı varlıkların oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7eedf-266">Create a production-grade build of the third-party, client-side assets.</span></span>
1. <span data-ttu-id="7eedf-267">Özel istemci tarafı varlıkları üretim sınıfı yapısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7eedf-267">Create a production-grade build of the custom client-side assets.</span></span>
1. <span data-ttu-id="7eedf-268">Web oluşturulan varlıkları publish klasörüne kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="7eedf-268">Copy the Webpack-generated assets to the publish folder.</span></span>

<span data-ttu-id="7eedf-269">MSBuild hedefi çalıştırılırken çağrılır:</span><span class="sxs-lookup"><span data-stu-id="7eedf-269">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="7eedf-270">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7eedf-270">Additional resources</span></span>

* [<span data-ttu-id="7eedf-271">Angular belgeleri</span><span class="sxs-lookup"><span data-stu-id="7eedf-271">Angular Docs</span></span>](https://angular.io/docs)
