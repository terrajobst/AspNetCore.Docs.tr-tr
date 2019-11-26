---
title: TypeScript ve WebPack ile ASP.NET Core SignalR kullanma
author: ssougnez
description: Bu öğreticide, Web paketini istemcisi TypeScript 'te yazılmış bir ASP.NET Core SignalR Web uygulaması paketleyip derlemek üzere yapılandırırsınız.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7c99c9e79647995886aec5b3a91584fd2f24451
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317482"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a><span data-ttu-id="d6a49-103">TypeScript ve WebPack ile ASP.NET Core SignalR kullanma</span><span class="sxs-lookup"><span data-stu-id="d6a49-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="d6a49-104">, [Sébastien Sougnez](https://twitter.com/ssougnez) ve [Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="d6a49-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d6a49-105">[WebPack](https://webpack.js.org/) , geliştiricilerin bir Web uygulamasının istemci tarafı kaynaklarını paketleyip oluşturmalarına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="d6a49-106">Bu öğretici, istemcisinin [TypeScript](https://www.typescriptlang.org/)'te yazıldığı bir ASP.NET Core SignalR Web uygulamasında WebPack 'in kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="d6a49-107">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="d6a49-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d6a49-108">Bir başlatıcı ASP.NET Core SignalR uygulama için yapı iskelesi</span><span class="sxs-lookup"><span data-stu-id="d6a49-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="d6a49-109">SignalR TypeScript istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6a49-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="d6a49-110">WebPack kullanarak derleme işlem hattı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6a49-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="d6a49-111">SignalR sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6a49-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="d6a49-112">İstemci ve sunucu arasındaki iletişimi etkinleştir</span><span class="sxs-lookup"><span data-stu-id="d6a49-112">Enable communication between client and server</span></span>

<span data-ttu-id="d6a49-113">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6a49-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="d6a49-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d6a49-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6a49-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6a49-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d6a49-116">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="d6a49-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="d6a49-117">.NET Core SDK 3,0 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d6a49-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d6a49-118">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="d6a49-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6a49-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="d6a49-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="d6a49-121">.NET Core SDK 3,0 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d6a49-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="d6a49-122">C#Visual Studio Code sürüm 1.17.1 veya üzeri için</span><span class="sxs-lookup"><span data-stu-id="d6a49-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="d6a49-123">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="d6a49-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="d6a49-124">ASP.NET Core Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="d6a49-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6a49-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6a49-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d6a49-126">Visual Studio 'Yu, *Path* ortam değişkeninde NPM için arama yapmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="d6a49-127">Varsayılan olarak, Visual Studio yükleme dizininde bulunan NPM sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="d6a49-128">Visual Studio 'da şu yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="d6a49-129">**Web Paket Yönetimi** > **dış web araçları**> > > **Araçlar** ve **Seçenekler** **'** e gidin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="d6a49-130">Listeden *$ (yol)* girişini seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="d6a49-131">Girdiyi listedeki ikinci konuma taşımak için yukarı oka tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="d6a49-133">Visual Studio yapılandırması tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="d6a49-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="d6a49-134">Projeyi oluşturma zamanı.</span><span class="sxs-lookup"><span data-stu-id="d6a49-134">It's time to create the project.</span></span>

1. <span data-ttu-id="d6a49-135">**Dosya** > **Yeni** > **projesi** menü seçeneğini kullanın ve **ASP.NET Core Web uygulaması** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="d6a49-136">Projeyi *Signalrwebpack*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="d6a49-137">Hedef çerçeve açılır listesinden *.NET Core* ' u seçin ve çerçeve Seçicisi açılır listesinden *ASP.NET Core 3,0* ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="d6a49-138">**Boş** şablonu seçin ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6a49-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d6a49-140">**Tümleşik terminalde**aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d6a49-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="d6a49-141">.NET Core 'u hedefleyen boş bir ASP.NET Core Web uygulaması, bir *Signalrwebpack* dizininde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="d6a49-142">WebPack ve TypeScript yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6a49-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="d6a49-143">Aşağıdaki adımlar, TypeScript 'in JavaScript 'e dönüştürülmesini ve istemci tarafı kaynaklarını paketlemeyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="d6a49-144">Bir *Package. JSON* dosyası oluşturmak için proje kökünde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6a49-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="d6a49-145">Vurgulanan özelliği *Package. JSON* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="d6a49-146">`private` özelliğinin `true` olarak ayarlanması, sonraki adımda paket yükleme uyarılarını önler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="d6a49-147">Gerekli NPM paketlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-147">Install the required npm packages.</span></span> <span data-ttu-id="d6a49-148">Proje kökünden aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6a49-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="d6a49-149">Aklınızda bazı komut ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="d6a49-149">Some command details to note:</span></span>

    * <span data-ttu-id="d6a49-150">Sürüm numarası, her paket adı için `@` işaretini izler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="d6a49-151">NPM bu özel paket sürümlerini yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="d6a49-152">`-E` seçeneği, NPM 'nin [semantik sürüm](https://semver.org/) aralığı işleçlerini *Package. JSON*öğesine yazmanın varsayılan davranışını devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="d6a49-153">Örneğin, `"webpack": "^4.29.3"`yerine `"webpack": "4.29.3"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="d6a49-154">Bu seçenek, daha yeni paket sürümlerine istenmeden yükseltme yapılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="d6a49-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="d6a49-155">Daha ayrıntılı bilgi için resmi [NPM-Install](https://docs.npmjs.com/cli/install) docs bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="d6a49-156">*Package. JSON* dosyasının `scripts` özelliğini aşağıdaki kod parçacığıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="d6a49-157">Betiklerin bazı açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="d6a49-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="d6a49-158">`build`: istemci tarafı kaynaklarınızı geliştirme modunda paketler ve dosya değişikliklerini izler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="d6a49-159">Dosya izleyici, bir proje dosyası her değiştiğinde paketin yeniden oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="d6a49-160">`mode` seçeneği, Tree gerçekleşmesi ve minbirleşme gibi üretim iyileştirmeleri devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="d6a49-161">Yalnızca geliştirmede `build` kullanın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="d6a49-162">`release`: istemci tarafı kaynaklarınızı üretim modunda Paketlayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="d6a49-163">`publish`: `release` betiği, istemci tarafı kaynaklarını üretim modunda paketleyip çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="d6a49-164">Uygulamayı yayımlamak için .NET Core CLI [Publish](/dotnet/core/tools/dotnet-publish) komutunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="d6a49-165">Aşağıdaki içeriğe sahip proje kökünde *WebPack. config. js*adlı bir dosya oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="d6a49-166">Yukarıdaki dosya Web paketi derlemesini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="d6a49-167">Aklınızda bazı yapılandırma ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="d6a49-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="d6a49-168">`output` özelliği, *dağ*'ın varsayılan değerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="d6a49-169">Paket, *Wwwroot* dizininde yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="d6a49-170">`resolve.extensions` dizisi, SignalR istemci JavaScript 'ı içeri aktarmak için *. js* içerir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="d6a49-171">Proje kökünde yeni bir *src* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="d6a49-172">Amaç, projenin istemci tarafı varlıklarını depolardır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="d6a49-173">Aşağıdaki içerikle *src/index.html* oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="d6a49-174">Önceki HTML, giriş sayfasının ortak işaretlemesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="d6a49-175">Yeni bir *src/CSS* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="d6a49-176">Amacı, projenin *. css* dosyalarını depolamadır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="d6a49-177">Aşağıdaki içerikle *src/CSS/Main. css* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="d6a49-178">Önceki *Main. css* dosyası uygulamayı stiller.</span><span class="sxs-lookup"><span data-stu-id="d6a49-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="d6a49-179">Şu içerikle *src/tsconfig. JSON* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="d6a49-180">Yukarıdaki kod, TypeScript derleyicisini [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript üretecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="d6a49-181">Aşağıdaki içeriğe sahip *src/index. TS* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="d6a49-182">Önceki TypeScript, DOM öğelerine başvuruları alır ve iki olay işleyicisini ekler:</span><span class="sxs-lookup"><span data-stu-id="d6a49-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="d6a49-183">`keyup`: Bu olay Kullanıcı metin kutusuna `tbMessage`olarak tanımlanan bir şeyi yazdığında ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="d6a49-184">`send` işlevi, Kullanıcı **ENTER** tuşuna bastığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="d6a49-185">`click`: Kullanıcı **Gönder** düğmesine tıkladığında bu olay ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="d6a49-186">`send` işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="d6a49-187">ASP.NET Core uygulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6a49-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="d6a49-188">`Startup.Configure` yönteminde, [Usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)öğesine çağrılar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="d6a49-189">Yukarıdaki kod, kullanıcının tam URL 'sini veya Web uygulamasının kök URL 'sini girmeksizin *Dizin. html* dosyasını bulmasını ve sunmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="d6a49-190">`Startup.Configure` yönteminin sonunda, bir */hub* yolunu `ChatHub` hub 'ına eşleyin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="d6a49-191">Merhaba Dünya görüntülenen kodu değiştirin *!*</span><span class="sxs-lookup"><span data-stu-id="d6a49-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="d6a49-192">Aşağıdaki satırla:</span><span class="sxs-lookup"><span data-stu-id="d6a49-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="d6a49-193">`Startup.ConfigureServices` yönteminde [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)öğesini çağırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="d6a49-194">SignalR hizmetlerini projenize ekler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="d6a49-195">Proje kökünde *hub*olarak adlandırılan yeni bir dizin oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="d6a49-196">Amacı, bir sonraki adımda oluşturulan SignalR hub 'ını deposağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="d6a49-197">Aşağıdaki kodla hub *hub 'ları/ChatHub. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="d6a49-198">`ChatHub` başvurusunu çözümlemek için, *Startup.cs* dosyasının en üstüne aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="d6a49-199">İstemci ve sunucu iletişimini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="d6a49-199">Enable client and server communication</span></span>

<span data-ttu-id="d6a49-200">Uygulama Şu anda ileti göndermek için basit bir form görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="d6a49-201">Bunu yapmayı denediğinizde hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="d6a49-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="d6a49-202">Sunucu belirli bir yolu dinliyor, ancak gönderilen iletilerle hiçbir şey yapmıyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="d6a49-203">Proje kökünde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6a49-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="d6a49-204">Yukarıdaki komut, istemcinin sunucuya ileti göndermesini sağlayan [SignalR TypeScript istemcisini](https://www.npmjs.com/package/@microsoft/signalr)yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="d6a49-205">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="d6a49-206">Yukarıdaki kod, sunucudan ileti almayı destekler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="d6a49-207">`HubConnectionBuilder` sınıfı, sunucu bağlantısını yapılandırmak için yeni bir Oluşturucu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="d6a49-208">`withUrl` işlevi hub URL 'sini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-208">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="d6a49-209">, istemci ile sunucu arasında ileti alışverişi yapılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="d6a49-209"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="d6a49-210">Her ileti belirli bir ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-210">Each message has a specific name.</span></span> <span data-ttu-id="d6a49-211">Örneğin, ileti bölgesindeki yeni iletiyi görüntülemekten sorumlu mantığı çalıştıran `messageReceived` adına sahip iletilere sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d6a49-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="d6a49-212">Belirli bir iletiyi dinlemek `on` işlevi aracılığıyla yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="d6a49-213">Herhangi bir sayıda ileti adını dinleyebilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-213">You can listen to any number of message names.</span></span> <span data-ttu-id="d6a49-214">Ayrıca, yazarın adı ve alınan iletinin içeriği gibi parametreleri iletiye geçirmek da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d6a49-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="d6a49-215">İstemci bir ileti aldıktan sonra yazarın adı ve `innerHTML` özniteliğinde ileti içeriğiyle yeni bir `div` öğesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="d6a49-216">Bu, iletileri görüntüleyen ana `div` öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="d6a49-217">Artık istemci bir ileti aldığına göre, ileti gönderecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="d6a49-218">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="d6a49-219">WebSockets bağlantısı aracılığıyla bir ileti gönderildiğinde `send` yönteminin çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="d6a49-220">Yöntemin ilk parametresi ileti adıdır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="d6a49-221">İleti verileri diğer parametreleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="d6a49-222">Bu örnekte, `newMessage` sunucusuna gönderildiği gibi bir ileti gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="d6a49-223">İleti, bir metin kutusundan Kullanıcı adından ve Kullanıcı girişinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="d6a49-224">Gönderme işlemi çalışırsa metin kutusu değeri temizlenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="d6a49-225">Vurgulanan yöntemi `ChatHub` sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="d6a49-226">Yukarıdaki kod, sunucu onları aldıktan sonra tüm bağlı kullanıcılara ileti almış iletileri yayınlar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="d6a49-227">Tüm iletileri almak için genel bir `on` yöntemi olması gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="d6a49-228">İleti adından sonra adlandırılmış bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="d6a49-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="d6a49-229">Bu örnekte, TypeScript istemcisi `newMessage`olarak tanımlanan bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="d6a49-230">C# `NewMessage` yöntemi, istemci tarafından gönderilen verileri bekler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="d6a49-231">[Istemcilerdeki](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) [sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemine bir çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="d6a49-232">Alınan iletiler, hub 'a bağlı tüm istemcilere gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="d6a49-233">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d6a49-233">Test the app</span></span>

<span data-ttu-id="d6a49-234">Uygulamanın aşağıdaki adımlarla çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6a49-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6a49-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d6a49-236">Web paketini *yayın* modunda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="d6a49-237">**Paket Yöneticisi konsol** penceresini kullanarak, proje kökünde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="d6a49-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="d6a49-238">Proje kökünde değilseniz, komutu girmeden önce `cd SignalRWebPack` girin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d6a49-239">Hata ayıklayıcıyı eklemeden uygulamayı bir tarayıcıda başlatmak \*\*için hata ayıklama > \*\* **Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="d6a49-240">*Wwwroot/index.html* dosyası `http://localhost:<port_number>`olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="d6a49-241">Derleme hataları alırsanız, çözümü kapatıp yeniden açmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="d6a49-242">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="d6a49-243">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d6a49-244">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d6a49-245">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6a49-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="d6a49-247">Proje kökünde aşağıdaki komutu yürüterek Web paketini *yayın* modunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d6a49-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d6a49-248">Proje kökünde aşağıdaki komutu yürüterek uygulamayı derleyin ve çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d6a49-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="d6a49-249">Web sunucusu uygulamayı başlatır ve localhost üzerinde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="d6a49-250">`http://localhost:<port_number>`için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="d6a49-251">*Wwwroot/index.html* dosyası sunulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="d6a49-252">Adres çubuğundan URL 'YI kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="d6a49-253">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="d6a49-254">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d6a49-255">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d6a49-256">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![ileti hem tarayıcı penceresinde görüntüleniyor](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6a49-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6a49-258">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d6a49-259">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="d6a49-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="d6a49-260">.NET Core SDK 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d6a49-260">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d6a49-261">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="d6a49-261">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6a49-262">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-262">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="d6a49-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-263">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="d6a49-264">.NET Core SDK 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d6a49-264">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="d6a49-265">C#Visual Studio Code sürüm 1.17.1 veya üzeri için</span><span class="sxs-lookup"><span data-stu-id="d6a49-265">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="d6a49-266">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="d6a49-266">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="d6a49-267">ASP.NET Core Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="d6a49-267">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6a49-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6a49-268">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d6a49-269">Visual Studio 'Yu, *Path* ortam değişkeninde NPM için arama yapmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-269">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="d6a49-270">Varsayılan olarak, Visual Studio yükleme dizininde bulunan NPM sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-270">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="d6a49-271">Visual Studio 'da şu yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-271">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="d6a49-272">**Web Paket Yönetimi** > **dış web araçları**> > > **Araçlar** ve **Seçenekler** **'** e gidin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-272">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="d6a49-273">Listeden *$ (yol)* girişini seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-273">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="d6a49-274">Girdiyi listedeki ikinci konuma taşımak için yukarı oka tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-274">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="d6a49-276">Visual Studio yapılandırması tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="d6a49-276">Visual Studio configuration is completed.</span></span> <span data-ttu-id="d6a49-277">Projeyi oluşturma zamanı.</span><span class="sxs-lookup"><span data-stu-id="d6a49-277">It's time to create the project.</span></span>

1. <span data-ttu-id="d6a49-278">**Dosya** > **Yeni** > **projesi** menü seçeneğini kullanın ve **ASP.NET Core Web uygulaması** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-278">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="d6a49-279">Projeyi *Signalrwebpack*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-279">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="d6a49-280">Hedef çerçeve açılır listesinden *.NET Core* ' u seçin ve çerçeve Seçicisi açılır listesinden *ASP.NET Core 2,2* ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-280">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="d6a49-281">**Boş** şablonu seçin ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-281">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6a49-282">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-282">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d6a49-283">**Tümleşik terminalde**aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d6a49-283">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="d6a49-284">.NET Core 'u hedefleyen boş bir ASP.NET Core Web uygulaması, bir *Signalrwebpack* dizininde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-284">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="d6a49-285">WebPack ve TypeScript yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6a49-285">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="d6a49-286">Aşağıdaki adımlar, TypeScript 'in JavaScript 'e dönüştürülmesini ve istemci tarafı kaynaklarını paketlemeyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-286">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="d6a49-287">Bir *Package. JSON* dosyası oluşturmak için proje kökünde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6a49-287">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="d6a49-288">Vurgulanan özelliği *Package. JSON* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-288">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="d6a49-289">`private` özelliğinin `true` olarak ayarlanması, sonraki adımda paket yükleme uyarılarını önler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-289">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="d6a49-290">Gerekli NPM paketlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-290">Install the required npm packages.</span></span> <span data-ttu-id="d6a49-291">Proje kökünden aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6a49-291">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="d6a49-292">Aklınızda bazı komut ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="d6a49-292">Some command details to note:</span></span>

    * <span data-ttu-id="d6a49-293">Sürüm numarası, her paket adı için `@` işaretini izler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-293">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="d6a49-294">NPM bu özel paket sürümlerini yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-294">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="d6a49-295">`-E` seçeneği, NPM 'nin [semantik sürüm](https://semver.org/) aralığı işleçlerini *Package. JSON*öğesine yazmanın varsayılan davranışını devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-295">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="d6a49-296">Örneğin, `"webpack": "^4.29.3"`yerine `"webpack": "4.29.3"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-296">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="d6a49-297">Bu seçenek, daha yeni paket sürümlerine istenmeden yükseltme yapılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="d6a49-297">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="d6a49-298">Daha ayrıntılı bilgi için resmi [NPM-Install](https://docs.npmjs.com/cli/install) docs bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-298">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="d6a49-299">*Package. JSON* dosyasının `scripts` özelliğini aşağıdaki kod parçacığıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-299">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="d6a49-300">Betiklerin bazı açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="d6a49-300">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="d6a49-301">`build`: istemci tarafı kaynaklarınızı geliştirme modunda paketler ve dosya değişikliklerini izler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-301">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="d6a49-302">Dosya izleyici, bir proje dosyası her değiştiğinde paketin yeniden oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-302">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="d6a49-303">`mode` seçeneği, Tree gerçekleşmesi ve minbirleşme gibi üretim iyileştirmeleri devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-303">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="d6a49-304">Yalnızca geliştirmede `build` kullanın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-304">Only use `build` in development.</span></span>
    * <span data-ttu-id="d6a49-305">`release`: istemci tarafı kaynaklarınızı üretim modunda Paketlayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-305">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="d6a49-306">`publish`: `release` betiği, istemci tarafı kaynaklarını üretim modunda paketleyip çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-306">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="d6a49-307">Uygulamayı yayımlamak için .NET Core CLI [Publish](/dotnet/core/tools/dotnet-publish) komutunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-307">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="d6a49-308">Aşağıdaki içeriğe sahip proje kökünde *WebPack. config. js*adlı bir dosya oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-308">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="d6a49-309">Yukarıdaki dosya Web paketi derlemesini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-309">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="d6a49-310">Aklınızda bazı yapılandırma ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="d6a49-310">Some configuration details to note:</span></span>

    * <span data-ttu-id="d6a49-311">`output` özelliği, *dağ*'ın varsayılan değerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-311">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="d6a49-312">Paket, *Wwwroot* dizininde yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-312">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="d6a49-313">`resolve.extensions` dizisi, SignalR istemci JavaScript 'ı içeri aktarmak için *. js* içerir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-313">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="d6a49-314">Proje kökünde yeni bir *src* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-314">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="d6a49-315">Amaç, projenin istemci tarafı varlıklarını depolardır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-315">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="d6a49-316">Aşağıdaki içerikle *src/index.html* oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-316">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="d6a49-317">Önceki HTML, giriş sayfasının ortak işaretlemesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-317">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="d6a49-318">Yeni bir *src/CSS* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-318">Create a new *src/css* directory.</span></span> <span data-ttu-id="d6a49-319">Amacı, projenin *. css* dosyalarını depolamadır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-319">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="d6a49-320">Aşağıdaki içerikle *src/CSS/Main. css* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-320">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="d6a49-321">Önceki *Main. css* dosyası uygulamayı stiller.</span><span class="sxs-lookup"><span data-stu-id="d6a49-321">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="d6a49-322">Şu içerikle *src/tsconfig. JSON* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-322">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="d6a49-323">Yukarıdaki kod, TypeScript derleyicisini [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript üretecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-323">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="d6a49-324">Aşağıdaki içeriğe sahip *src/index. TS* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-324">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="d6a49-325">Önceki TypeScript, DOM öğelerine başvuruları alır ve iki olay işleyicisini ekler:</span><span class="sxs-lookup"><span data-stu-id="d6a49-325">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="d6a49-326">`keyup`: Bu olay Kullanıcı metin kutusuna `tbMessage`olarak tanımlanan bir şeyi yazdığında ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-326">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="d6a49-327">`send` işlevi, Kullanıcı **ENTER** tuşuna bastığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-327">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="d6a49-328">`click`: Kullanıcı **Gönder** düğmesine tıkladığında bu olay ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-328">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="d6a49-329">`send` işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-329">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="d6a49-330">ASP.NET Core uygulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6a49-330">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="d6a49-331">`Startup.Configure` yönteminde belirtilen kod *Merhaba Dünya!* görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-331">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="d6a49-332">`app.Run` yöntemi çağrısını, [Usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)çağrılarıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-332">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="d6a49-333">Yukarıdaki kod, kullanıcının tam URL 'sini veya Web uygulamasının kök URL 'sini girmeksizin *Dizin. html* dosyasını bulmasını ve sunmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-333">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="d6a49-334">`Startup.ConfigureServices` yönteminde [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) öğesini çağırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-334">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d6a49-335">SignalR hizmetlerini projenize ekler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-335">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="d6a49-336">Bir */hub* yolunu `ChatHub` hub 'ına eşleyin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-336">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="d6a49-337">`Startup.Configure` yönteminin sonuna aşağıdaki satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-337">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="d6a49-338">Proje kökünde *hub*olarak adlandırılan yeni bir dizin oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-338">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="d6a49-339">Amacı, bir sonraki adımda oluşturulan SignalR hub 'ını deposağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-339">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="d6a49-340">Aşağıdaki kodla hub *hub 'ları/ChatHub. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d6a49-340">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="d6a49-341">`ChatHub` başvurusunu çözümlemek için, *Startup.cs* dosyasının en üstüne aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-341">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="d6a49-342">İstemci ve sunucu iletişimini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="d6a49-342">Enable client and server communication</span></span>

<span data-ttu-id="d6a49-343">Uygulama Şu anda ileti göndermek için basit bir form görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-343">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="d6a49-344">Bunu yapmayı denediğinizde hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="d6a49-344">Nothing happens when you try to do so.</span></span> <span data-ttu-id="d6a49-345">Sunucu belirli bir yolu dinliyor, ancak gönderilen iletilerle hiçbir şey yapmıyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-345">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="d6a49-346">Proje kökünde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6a49-346">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="d6a49-347">Yukarıdaki komut, istemcinin sunucuya ileti göndermesini sağlayan [SignalR TypeScript istemcisini](https://www.npmjs.com/package/@microsoft/signalr)yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="d6a49-347">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="d6a49-348">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-348">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="d6a49-349">Yukarıdaki kod, sunucudan ileti almayı destekler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-349">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="d6a49-350">`HubConnectionBuilder` sınıfı, sunucu bağlantısını yapılandırmak için yeni bir Oluşturucu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-350">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="d6a49-351">`withUrl` işlevi hub URL 'sini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-351">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="d6a49-352">, istemci ile sunucu arasında ileti alışverişi yapılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="d6a49-352"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="d6a49-353">Her ileti belirli bir ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-353">Each message has a specific name.</span></span> <span data-ttu-id="d6a49-354">Örneğin, ileti bölgesindeki yeni iletiyi görüntülemekten sorumlu mantığı çalıştıran `messageReceived` adına sahip iletilere sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d6a49-354">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="d6a49-355">Belirli bir iletiyi dinlemek `on` işlevi aracılığıyla yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-355">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="d6a49-356">Herhangi bir sayıda ileti adını dinleyebilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-356">You can listen to any number of message names.</span></span> <span data-ttu-id="d6a49-357">Ayrıca, yazarın adı ve alınan iletinin içeriği gibi parametreleri iletiye geçirmek da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d6a49-357">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="d6a49-358">İstemci bir ileti aldıktan sonra yazarın adı ve `innerHTML` özniteliğinde ileti içeriğiyle yeni bir `div` öğesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-358">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="d6a49-359">Bu, iletileri görüntüleyen ana `div` öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-359">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="d6a49-360">Artık istemci bir ileti aldığına göre, ileti gönderecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-360">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="d6a49-361">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-361">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="d6a49-362">WebSockets bağlantısı aracılığıyla bir ileti gönderildiğinde `send` yönteminin çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-362">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="d6a49-363">Yöntemin ilk parametresi ileti adıdır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-363">The method's first parameter is the message name.</span></span> <span data-ttu-id="d6a49-364">İleti verileri diğer parametreleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-364">The message data inhabits the other parameters.</span></span> <span data-ttu-id="d6a49-365">Bu örnekte, `newMessage` sunucusuna gönderildiği gibi bir ileti gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-365">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="d6a49-366">İleti, bir metin kutusundan Kullanıcı adından ve Kullanıcı girişinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-366">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="d6a49-367">Gönderme işlemi çalışırsa metin kutusu değeri temizlenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-367">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="d6a49-368">Vurgulanan yöntemi `ChatHub` sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d6a49-368">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="d6a49-369">Yukarıdaki kod, sunucu onları aldıktan sonra tüm bağlı kullanıcılara ileti almış iletileri yayınlar.</span><span class="sxs-lookup"><span data-stu-id="d6a49-369">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="d6a49-370">Tüm iletileri almak için genel bir `on` yöntemi olması gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-370">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="d6a49-371">İleti adından sonra adlandırılmış bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="d6a49-371">A method named after the message name suffices.</span></span>

    <span data-ttu-id="d6a49-372">Bu örnekte, TypeScript istemcisi `newMessage`olarak tanımlanan bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-372">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="d6a49-373">C# `NewMessage` yöntemi, istemci tarafından gönderilen verileri bekler.</span><span class="sxs-lookup"><span data-stu-id="d6a49-373">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="d6a49-374">[Istemcilerdeki](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) [sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemine bir çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="d6a49-374">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="d6a49-375">Alınan iletiler, hub 'a bağlı tüm istemcilere gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-375">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="d6a49-376">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d6a49-376">Test the app</span></span>

<span data-ttu-id="d6a49-377">Uygulamanın aşağıdaki adımlarla çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="d6a49-377">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6a49-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6a49-378">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d6a49-379">Web paketini *yayın* modunda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-379">Run Webpack in *release* mode.</span></span> <span data-ttu-id="d6a49-380">**Paket Yöneticisi konsol** penceresini kullanarak, proje kökünde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="d6a49-380">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="d6a49-381">Proje kökünde değilseniz, komutu girmeden önce `cd SignalRWebPack` girin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-381">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d6a49-382">Hata ayıklayıcıyı eklemeden uygulamayı bir tarayıcıda başlatmak \*\*için hata ayıklama > \*\* **Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d6a49-382">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="d6a49-383">*Wwwroot/index.html* dosyası `http://localhost:<port_number>`olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-383">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="d6a49-384">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-384">Open another browser instance (any browser).</span></span> <span data-ttu-id="d6a49-385">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-385">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d6a49-386">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-386">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d6a49-387">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-387">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6a49-388">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6a49-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="d6a49-389">Proje kökünde aşağıdaki komutu yürüterek Web paketini *yayın* modunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d6a49-389">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d6a49-390">Proje kökünde aşağıdaki komutu yürüterek uygulamayı derleyin ve çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d6a49-390">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="d6a49-391">Web sunucusu uygulamayı başlatır ve localhost üzerinde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-391">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="d6a49-392">`http://localhost:<port_number>`için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-392">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="d6a49-393">*Wwwroot/index.html* dosyası sunulur.</span><span class="sxs-lookup"><span data-stu-id="d6a49-393">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="d6a49-394">Adres çubuğundan URL 'YI kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-394">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="d6a49-395">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-395">Open another browser instance (any browser).</span></span> <span data-ttu-id="d6a49-396">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-396">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d6a49-397">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d6a49-397">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d6a49-398">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d6a49-398">The unique user name and message are displayed on both pages instantly.</span></span>

---

![ileti hem tarayıcı penceresinde görüntüleniyor](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d6a49-400">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d6a49-400">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
