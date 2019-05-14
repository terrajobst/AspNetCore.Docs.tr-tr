---
title: TypeScript ve Web ile ASP.NET Core SignalR kullanma
author: ssougnez
description: Bu öğreticide, Web, istemci, içinde TypeScript yazılmış bir ASP.NET Core SignalR web uygulaması derleme ve paket için yapılandırın.
ms.author: bradyg
ms.custom: mvc
ms.date: 04/23/2019
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: b186dffd724fb2fed49bbc2587ec066db319dffc
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610445"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="1214d-103">TypeScript ve Web ile ASP.NET Core SignalR kullanma</span><span class="sxs-lookup"><span data-stu-id="1214d-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="1214d-104">Tarafından [Sébastien Sougnez](https://twitter.com/ssougnez) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1214d-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1214d-105">[Web](https://webpack.js.org/) geliştiricilerin paket ve derleme bir web uygulamasının istemci-tarafı kaynakları sağlar.</span><span class="sxs-lookup"><span data-stu-id="1214d-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="1214d-106">Web istemcisi yazılmış bir ASP.NET Core SignalR web uygulaması kullanarak bu eğitimde [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="1214d-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="1214d-107">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="1214d-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1214d-108">Bir başlangıç ASP.NET Core SignalR uygulaması iskelesini</span><span class="sxs-lookup"><span data-stu-id="1214d-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="1214d-109">SignalR TypeScript istemciyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1214d-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="1214d-110">Web kullanarak bir derleme işlem hattı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="1214d-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="1214d-111">SignalR sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1214d-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="1214d-112">İstemci ve sunucu arasında iletişimi etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="1214d-112">Enable communication between client and server</span></span>

<span data-ttu-id="1214d-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1214d-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1214d-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="1214d-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1214d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1214d-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1214d-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ile **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="1214d-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="1214d-117">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1214d-117">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="1214d-118">[Node.js](https://nodejs.org/) ile [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="1214d-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1214d-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1214d-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="1214d-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1214d-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="1214d-121">.NET core SDK 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1214d-121">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="1214d-122">C#Visual Studio Code sürümü 1.17.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="1214d-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="1214d-123">[Node.js](https://nodejs.org/) ile [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="1214d-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="1214d-124">ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="1214d-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1214d-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1214d-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1214d-126">' Nde npm aramak için Visual Studio'yu yapılandırma *yolu* ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="1214d-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="1214d-127">Varsayılan olarak, Visual Studio, yükleme dizininde bulunan npm sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="1214d-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="1214d-128">Visual Studio'da bu yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="1214d-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="1214d-129">Gidin **Araçları** > **seçenekleri** > **projeler ve çözümler** > **WebpaketYönetimi**  >  **Dış Web Araçları**.</span><span class="sxs-lookup"><span data-stu-id="1214d-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="1214d-130">Seçin *$(PATH)* listeden girişi.</span><span class="sxs-lookup"><span data-stu-id="1214d-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="1214d-131">Giriş listede ikinci konuma taşımak için yukarı oka tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1214d-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="1214d-133">Visual Studio yapılandırması tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="1214d-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="1214d-134">Projeyi oluşturmak için zaman var.</span><span class="sxs-lookup"><span data-stu-id="1214d-134">It's time to create the project.</span></span>

1. <span data-ttu-id="1214d-135">Kullanım **dosya** > **yeni** > **proje** menüsünde seçenek ve **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="1214d-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="1214d-136">Projeyi adlandırın *SignalRWebPack*seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1214d-136">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="1214d-137">Seçin *.NET Core* açılır ve select hedef çerçeveden *ASP.NET Core 2.2* framework Seçici açılan menüsü'nden.</span><span class="sxs-lookup"><span data-stu-id="1214d-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="1214d-138">Seçin **boş** şablonu ve select **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1214d-138">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1214d-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1214d-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1214d-140">Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="1214d-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="1214d-141">Boş bir ASP.NET Core web uygulaması .NET Core'u hedefleyen oluşturulan bir *SignalRWebPack* dizin.</span><span class="sxs-lookup"><span data-stu-id="1214d-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="1214d-142">Web ve TypeScript yapılandırın</span><span class="sxs-lookup"><span data-stu-id="1214d-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="1214d-143">Aşağıdaki adımlar, JavaScript, TypeScript, dönüştürme ve istemci tarafı kaynaklarını paketleme yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1214d-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="1214d-144">Aşağıdaki komutu yürütün oluşturmak için proje kök dizininde bir *package.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1214d-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="1214d-145">Vurgulanan özellik ekleyin *package.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1214d-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="1214d-146">Ayarı `private` özelliğini `true` sonraki adımda paket yükleme uyarıları engeller.</span><span class="sxs-lookup"><span data-stu-id="1214d-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="1214d-147">Gerekli npm paketlerini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1214d-147">Install the required npm packages.</span></span> <span data-ttu-id="1214d-148">Proje kökünden aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1214d-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="1214d-149">Dikkat edilecek bazı komut ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="1214d-149">Some command details to note:</span></span>

    * <span data-ttu-id="1214d-150">Bir sürüm numarasından `@` her paket adı için oturum açın.</span><span class="sxs-lookup"><span data-stu-id="1214d-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="1214d-151">npm, bu belirli bir paket sürümlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="1214d-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="1214d-152">`-E` Seçeneği, yazma npm'ın varsayılan davranışı devre dışı bırakır [semantic versioning](https://semver.org/) işleçler için aralığı *package.json*.</span><span class="sxs-lookup"><span data-stu-id="1214d-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="1214d-153">Örneğin, `"webpack": "4.29.3"` yerine kullanılan `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="1214d-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="1214d-154">Bu seçenek, istenmeyen yükseltmeleri için yeni bir paket sürümlerini önler.</span><span class="sxs-lookup"><span data-stu-id="1214d-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="1214d-155">Resmi görmek [npm yükleme](https://docs.npmjs.com/cli/install) daha fazla ayrıntı için belgeleri.</span><span class="sxs-lookup"><span data-stu-id="1214d-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="1214d-156">Değiştirin `scripts` özelliği *package.json* aşağıdaki kod parçacığı dosyası:</span><span class="sxs-lookup"><span data-stu-id="1214d-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="1214d-157">Bazı komut dosyaları açıklaması:</span><span class="sxs-lookup"><span data-stu-id="1214d-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="1214d-158">`build`: İstemci tarafı kaynaklarınızı geliştirme modunda oluşturur ve dosya değişikliklerini izler.</span><span class="sxs-lookup"><span data-stu-id="1214d-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="1214d-159">Dosya İzleyici, paket her zaman bir proje dosya değişiklikleri yeniden neden olur.</span><span class="sxs-lookup"><span data-stu-id="1214d-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="1214d-160">`mode` Seçeneği, ağaç sallayabilir ve küçültme gibi üretim iyileştirmeleri devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="1214d-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="1214d-161">Yalnızca `build` geliştirme.</span><span class="sxs-lookup"><span data-stu-id="1214d-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="1214d-162">`release`: İstemci tarafı kaynaklarınızı üretim modunda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1214d-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="1214d-163">`publish`: Çalıştırmaları `release` üretim modunda istemci-tarafı kaynakları paket betiği.</span><span class="sxs-lookup"><span data-stu-id="1214d-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="1214d-164">.NET Core CLI'ın çağırdığı [yayımlama](/dotnet/core/tools/dotnet-publish) uygulamayı yayımlamak için komutu.</span><span class="sxs-lookup"><span data-stu-id="1214d-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="1214d-165">Adlı bir dosya oluşturun *webpack.config.js*, proje kökündeki aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="1214d-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="1214d-166">Önceki dosya Web Derleme yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="1214d-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="1214d-167">Dikkat edilecek bazı yapılandırma ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="1214d-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="1214d-168">`output` Özelliğini geçersiz kılar, varsayılan değerini *dist*.</span><span class="sxs-lookup"><span data-stu-id="1214d-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="1214d-169">Paket yerine yayıldığını *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="1214d-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="1214d-170">`resolve.extensions` Dizi içeren *.js* SignalR istemci JavaScript içeri aktarmak için.</span><span class="sxs-lookup"><span data-stu-id="1214d-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="1214d-171">Yeni bir *src* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="1214d-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="1214d-172">Amacı, projenin istemci-tarafı varlıkları depolamaktır.</span><span class="sxs-lookup"><span data-stu-id="1214d-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="1214d-173">Oluşturma *src/index.html* aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="1214d-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="1214d-174">Önceki HTML spotuna Demirbaş biçimlendirme tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1214d-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="1214d-175">Yeni bir *src/css* dizin.</span><span class="sxs-lookup"><span data-stu-id="1214d-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="1214d-176">Amacı, projenin depolamaktır *.css* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="1214d-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="1214d-177">Oluşturma *src/css/main.css* aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="1214d-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="1214d-178">Önceki *main.css* dosya stiller uygulama.</span><span class="sxs-lookup"><span data-stu-id="1214d-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="1214d-179">Oluşturma *src/tsconfig.json* aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="1214d-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="1214d-180">Yukarıdaki kod üretmek için TypeScript derleyicisi yapılandırır [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 ile uyumlu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1214d-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="1214d-181">Oluşturma *src/index.ts* aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="1214d-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="1214d-182">Önceki TypeScript DOM öğeleri için başvuru alır ve iki olay işleyicisi ekler:</span><span class="sxs-lookup"><span data-stu-id="1214d-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="1214d-183">`keyup`: Kullanıcı bir şey olarak tanımlanmış metin kutusuna yazdığında, bu olay harekete `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="1214d-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="1214d-184">`send` İşlevi, kullanıcının bastığında çağrılır **Enter** anahtarı.</span><span class="sxs-lookup"><span data-stu-id="1214d-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="1214d-185">`click`: Kullanıcı tıkladığında bu olay harekete **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="1214d-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="1214d-186">`send` İşlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1214d-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="1214d-187">ASP.NET Core uygulaması yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1214d-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="1214d-188">Sağlanan kod `Startup.Configure` yöntemi görüntüler *Merhaba Dünya!*.</span><span class="sxs-lookup"><span data-stu-id="1214d-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="1214d-189">Değiştirin `app.Run` yöntem çağrısının çağrılarıyla [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="1214d-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="1214d-190">Yukarıdaki kod bulun ve hizmet sunucuya sağlar *index.html* olmadığını tam URL'sini veya web uygulaması kök URL'sini kullanıcının girdiği dosya.</span><span class="sxs-lookup"><span data-stu-id="1214d-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="1214d-191">Çağrı [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) içinde `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1214d-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="1214d-192">SignalR Hizmetleri projenize ekler.</span><span class="sxs-lookup"><span data-stu-id="1214d-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="1214d-193">Harita bir */hub* yönlendirmek `ChatHub` hub.</span><span class="sxs-lookup"><span data-stu-id="1214d-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="1214d-194">Sonunda aşağıdaki satırları ekleyin `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1214d-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="1214d-195">Adlı yeni bir dizin oluşturma *Hubs*, proje kökündeki.</span><span class="sxs-lookup"><span data-stu-id="1214d-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="1214d-196">Amacı sonraki adımda oluşturduğunuz SignalR hub depolamaktır.</span><span class="sxs-lookup"><span data-stu-id="1214d-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="1214d-197">Hub'ı oluşturma *Hubs/ChatHub.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="1214d-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="1214d-198">Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlemek için dosya `ChatHub` başvurusu:</span><span class="sxs-lookup"><span data-stu-id="1214d-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="1214d-199">İstemci ve sunucu iletişimi etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="1214d-199">Enable client and server communication</span></span>

<span data-ttu-id="1214d-200">Uygulama şu anda ileti göndermek için basit bir form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1214d-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="1214d-201">Bunu yapmak denediğinizde hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="1214d-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="1214d-202">Sunucu, belirli bir yol dinleme ancak gönderilen iletilerle hiçbir şey yapmaz.</span><span class="sxs-lookup"><span data-stu-id="1214d-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="1214d-203">Proje kök dizinini aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1214d-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="1214d-204">Önceki komutta yükler [SignalR TypeScript istemci](https://www.npmjs.com/package/@aspnet/signalr), istemcinin sunucuya ileti göndermek izin verir.</span><span class="sxs-lookup"><span data-stu-id="1214d-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="1214d-205">Vurgulanmış kodu ekleyin *src/index.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1214d-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="1214d-206">Yukarıdaki kod, sunucudan iletileri alma destekler.</span><span class="sxs-lookup"><span data-stu-id="1214d-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="1214d-207">`HubConnectionBuilder` Sınıf sunucu bağlantısı yapılandırmak için yeni bir oluşturucu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1214d-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="1214d-208">`withUrl` İşlevi hub URL'sini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="1214d-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="1214d-209">SignalR, bir istemci ve sunucu arasında ileti alışverişi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1214d-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="1214d-210">Her ileti, belirli bir adı vardır.</span><span class="sxs-lookup"><span data-stu-id="1214d-210">Each message has a specific name.</span></span> <span data-ttu-id="1214d-211">Örneğin, iletileri adıyla olabilir `messageReceived` iletileri bölgede yeni bir ileti görüntülemek için sorumlu mantıksal yürütün.</span><span class="sxs-lookup"><span data-stu-id="1214d-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="1214d-212">Aracılığıyla belirli bir ileti için dinleme yapılabilir `on` işlevi.</span><span class="sxs-lookup"><span data-stu-id="1214d-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="1214d-213">Herhangi bir sayıda ileti adları dinleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1214d-213">You can listen to any number of message names.</span></span> <span data-ttu-id="1214d-214">Yazarın adı ve alınan ileti içeriği gibi iletisi parametreleri geçirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1214d-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="1214d-215">İstemci bir ileti aldıktan sonra yeni bir `div` öğesi oluşturulduğunda yazarın adı ve şu iletiyle içeriği kendi `innerHTML` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1214d-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="1214d-216">Ana eklenir `div` iletileri görüntülemeyi öğesi.</span><span class="sxs-lookup"><span data-stu-id="1214d-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="1214d-217">İstemci bir ileti alabilir, ileti göndermek için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1214d-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="1214d-218">Vurgulanmış kodu ekleyin *src/index.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1214d-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="1214d-219">WebSockets bağlantısı üzerinden ileti gönderme, arama gerektirir `send` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1214d-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="1214d-220">Yöntemin ilk parametresi ileti adıdır.</span><span class="sxs-lookup"><span data-stu-id="1214d-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="1214d-221">İleti verileri diğer parametreler inhabits.</span><span class="sxs-lookup"><span data-stu-id="1214d-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="1214d-222">Bu örnekte, bir ileti olarak tanımlanır. `newMessage` sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1214d-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="1214d-223">Kullanıcı adı ve kullanıcı bir metin kutusunda girişi ileti oluşur.</span><span class="sxs-lookup"><span data-stu-id="1214d-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="1214d-224">Gönderme çalışırsa, metin kutusunun değerini temizlenir.</span><span class="sxs-lookup"><span data-stu-id="1214d-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="1214d-225">Vurgulanan yöntemine ekleyin `ChatHub` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="1214d-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="1214d-226">Sunucu bunları aldıktan sonra önceki kod bağlı tüm kullanıcılar için alınan iletiler yayımlar.</span><span class="sxs-lookup"><span data-stu-id="1214d-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="1214d-227">Genel gereksizdir `on` tüm iletileri almak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1214d-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="1214d-228">İleti adı eklerini sonra adlı bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="1214d-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="1214d-229">Bu örnekte, TypeScript istemci olarak tanımlanan bir ileti gönderir. `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="1214d-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="1214d-230">C# `NewMessage` yöntemi istemci tarafından gönderilen verilerin bekliyor.</span><span class="sxs-lookup"><span data-stu-id="1214d-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="1214d-231">İçin bir çağrı yapılır [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metodunda [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="1214d-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="1214d-232">Alınan iletiler, hub'ına bağlı tüm istemcilere gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1214d-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="1214d-233">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="1214d-233">Test the app</span></span>

<span data-ttu-id="1214d-234">Uygulama aşağıdaki adımlarla çalışır durumda olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1214d-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1214d-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1214d-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1214d-236">Web Çalıştır *yayın* modu.</span><span class="sxs-lookup"><span data-stu-id="1214d-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="1214d-237">Kullanarak **Paket Yöneticisi Konsolu** penceresinde proje kök dizininde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="1214d-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="1214d-238">Proje kök dizininde değilse girin `cd SignalRWebPack` komutu ulaşmadan önce.</span><span class="sxs-lookup"><span data-stu-id="1214d-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="1214d-239">Seçin **hata ayıklama** > **ayıklamadan Başlat** Haya ayıklayıcı eklemeden uygulamayı bir tarayıcıda başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="1214d-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="1214d-240">*Wwwroot/index.html* dosya hizmet `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="1214d-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="1214d-241">Başka bir tarayıcı örneğinde (herhangi bir tarayıcıda) açın.</span><span class="sxs-lookup"><span data-stu-id="1214d-241">Open another browser instance (any browser).</span></span> <span data-ttu-id="1214d-242">Adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="1214d-242">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="1214d-243">Ya da tarayıcı seçin, bir **ileti** metin kutusu seçeneğine tıklayıp **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="1214d-243">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="1214d-244">Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1214d-244">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1214d-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1214d-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1214d-246">Web Çalıştır *yayın* proje kök dizininde aşağıdaki komutu yürüterek modu:</span><span class="sxs-lookup"><span data-stu-id="1214d-246">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="1214d-247">Yapı ve proje kök dizininde aşağıdaki komutu çalıştırarak uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1214d-247">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="1214d-248">Web sunucusu uygulama başlar ve komut localhost üzerinde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="1214d-248">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="1214d-249">Bir tarayıcıda `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="1214d-249">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="1214d-250">*Wwwroot/index.html* dosya hizmet.</span><span class="sxs-lookup"><span data-stu-id="1214d-250">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="1214d-251">Adres çubuğundan URL'yi kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="1214d-251">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="1214d-252">Başka bir tarayıcı örneğinde (herhangi bir tarayıcıda) açın.</span><span class="sxs-lookup"><span data-stu-id="1214d-252">Open another browser instance (any browser).</span></span> <span data-ttu-id="1214d-253">Adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="1214d-253">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="1214d-254">Ya da tarayıcı seçin, bir **ileti** metin kutusu seçeneğine tıklayıp **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="1214d-254">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="1214d-255">Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1214d-255">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Her iki Tarayıcı pencerelerinde görüntülenen ileti](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="1214d-257">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1214d-257">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
