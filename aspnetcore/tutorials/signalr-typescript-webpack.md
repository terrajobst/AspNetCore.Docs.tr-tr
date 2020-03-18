---
title: TypeScript ve WebPack ile ASP.NET Core SignalR kullanma
author: ssougnez
description: Bu öğreticide, Web paketini istemcisi TypeScript 'te yazılmış bir ASP.NET Core SignalR Web uygulaması paketleyip derlemek üzere yapılandırırsınız.
ms.author: bradyg
ms.custom: mvc
ms.date: 02/10/2020
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: ce5752743912a979a95fb5d504e4bcbb2b69ce1e
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511346"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="6784c-103">TypeScript ve WebPack ile ASP.NET Core SignalR kullanın</span><span class="sxs-lookup"><span data-stu-id="6784c-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="6784c-104">, [Sébastien Sougnez](https://twitter.com/ssougnez) ve [Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="6784c-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="6784c-105">[WebPack](https://webpack.js.org/) , geliştiricilerin bir Web uygulamasının istemci tarafı kaynaklarını paketleyip oluşturmalarına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6784c-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="6784c-106">Bu öğreticide, istemcisinin [TypeScript](https://www.typescriptlang.org/)'te yazıldığı bir ASP.NET Core SignalR Web uygulamasında WebPack 'in kullanımı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6784c-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="6784c-107">Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6784c-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6784c-108">Bir başlatıcı ASP.NET Core SignalR uygulaması yapı iskelesi</span><span class="sxs-lookup"><span data-stu-id="6784c-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="6784c-109">SignalR TypeScript istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6784c-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="6784c-110">WebPack kullanarak derleme işlem hattı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6784c-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="6784c-111">SignalR sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6784c-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="6784c-112">İstemci ve sunucu arasındaki iletişimi etkinleştir</span><span class="sxs-lookup"><span data-stu-id="6784c-112">Enable communication between client and server</span></span>

<span data-ttu-id="6784c-113">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6784c-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="6784c-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6784c-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6784c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6784c-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6784c-116">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="6784c-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6784c-117">.NET Core SDK 3.0 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="6784c-117">.NET Core SDK 3.0 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* <span data-ttu-id="6784c-118">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="6784c-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6784c-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6784c-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6784c-121">.NET Core SDK 3.0 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="6784c-121">.NET Core SDK 3.0 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* [<span data-ttu-id="6784c-122">C#Visual Studio Code sürüm 1.17.1 veya üzeri için</span><span class="sxs-lookup"><span data-stu-id="6784c-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* <span data-ttu-id="6784c-123">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="6784c-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="6784c-124">ASP.NET Core Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6784c-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6784c-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6784c-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6784c-126">Visual Studio 'Yu, *Path* ortam değişkeninde NPM için arama yapmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="6784c-127">Varsayılan olarak, Visual Studio yükleme dizininde bulunan NPM sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="6784c-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="6784c-128">Visual Studio 'da şu yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="6784c-129">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="6784c-129">Launch Visual Studio.</span></span> <span data-ttu-id="6784c-130">Başlangıç penceresinde, **kod olmadan devam et**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-130">At the start window, select **Continue without code**.</span></span>
1. <span data-ttu-id="6784c-131">**Web Paket Yönetimi** > **dış web araçları**> > > **Araçlar** ve **Seçenekler** **'** e gidin.</span><span class="sxs-lookup"><span data-stu-id="6784c-131">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="6784c-132">Listeden *$ (yol)* girişini seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-132">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="6784c-133">Yukarı oka tıklayarak girişi listedeki ikinci konuma taşıyın ve **Tamam**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-133">Click the up arrow to move the entry to the second position in the list, and select **OK**.</span></span>

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="6784c-135">Visual Studio yapılandırması tamamlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6784c-135">Visual Studio configuration is complete.</span></span>

1. <span data-ttu-id="6784c-136">**Dosya** > **Yeni** > **projesi** menü seçeneğini kullanın ve **ASP.NET Core Web uygulaması** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-136">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="6784c-137">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-137">Select **Next**.</span></span>
1. <span data-ttu-id="6784c-138">Projeyi *Signalrwebpack*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-138">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="6784c-139">Hedef çerçeve açılır listesinden *.NET Core* ' u seçin ve çerçeve Seçicisi açılır listesinden *ASP.NET Core 3,1* ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-139">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.1* from the framework selector drop-down.</span></span> <span data-ttu-id="6784c-140">**Boş** şablonu seçin ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-140">Select the **Empty** template, and select **Create**.</span></span>

<span data-ttu-id="6784c-141">`Microsoft.TypeScript.MSBuild` paketini projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-141">Add the `Microsoft.TypeScript.MSBuild` package to the project:</span></span>

1. <span data-ttu-id="6784c-142">**Çözüm Gezgini** (sağ bölme) içinde, proje düğümüne sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-142">In **Solution Explorer** (right pane), right-click the project node and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="6784c-143">**Araştır** sekmesinde, `Microsoft.TypeScript.MSBuild`arayın ve ardından, **paketi yüklemek için sağdaki aç '** a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-143">In the **Browse** tab, search for `Microsoft.TypeScript.MSBuild`, and then click **Install** on the right to install the package.</span></span>

<span data-ttu-id="6784c-144">Visual Studio, NuGet paketini **Çözüm Gezgini**' deki **Bağımlılıklar** düğümüne ekler ve projede TypeScript derlemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="6784c-144">Visual Studio adds the NuGet package under the **Dependencies** node in **Solution Explorer**, enabling TypeScript compilation in the project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6784c-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-145">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6784c-146">**Tümleşik terminalde**aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-146">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
code -r SignalRWebPack
```

* <span data-ttu-id="6784c-147">`dotnet new` komutu, bir *Signalrwebpack* dizininde boş bir ASP.NET Core Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6784c-147">The `dotnet new` command creates an empty ASP.NET Core web app in a *SignalRWebPack* directory.</span></span>
* <span data-ttu-id="6784c-148">`code` komutu, geçerli Visual Studio Code örneğindeki *Signalrwebpack* klasörünü açar.</span><span class="sxs-lookup"><span data-stu-id="6784c-148">The `code` command opens the *SignalRWebPack* folder in the current instance of Visual Studio Code.</span></span>

<span data-ttu-id="6784c-149">**Tümleşik terminalde**aşağıdaki .NET Core CLI komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-149">Run the following .NET Core CLI command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add package Microsoft.TypeScript.MSBuild
```

<span data-ttu-id="6784c-150">Yukarıdaki komut, [Microsoft. TypeScript. MSBuild](https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/) paketini ekler ve projede TypeScript derlemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="6784c-150">The preceding command adds the [Microsoft.TypeScript.MSBuild](https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/) package, enabling TypeScript compilation in the project.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="6784c-151">WebPack ve TypeScript yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6784c-151">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="6784c-152">Aşağıdaki adımlar, TypeScript 'in JavaScript 'e dönüştürülmesini ve istemci tarafı kaynaklarını paketlemeyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-152">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="6784c-153">Bir *Package. JSON* dosyası oluşturmak için proje kökünde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-153">Run the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="6784c-154">Vurgulanan özelliği *Package. JSON* dosyasına ekleyin ve dosya değişikliklerini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="6784c-154">Add the highlighted property to the *package.json* file and save the file changes:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="6784c-155">`private` özelliğinin `true` olarak ayarlanması, sonraki adımda paket yükleme uyarılarını önler.</span><span class="sxs-lookup"><span data-stu-id="6784c-155">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="6784c-156">Gerekli NPM paketlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="6784c-156">Install the required npm packages.</span></span> <span data-ttu-id="6784c-157">Proje kökünden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-157">Run the following command from the project root:</span></span>

    ```console
    npm i -D -E clean-webpack-plugin@3.0.0 css-loader@3.4.2 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.9.0 ts-loader@6.2.1 typescript@3.7.5 webpack@4.41.5 webpack-cli@3.3.10
    ```

    <span data-ttu-id="6784c-158">Aklınızda bazı komut ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="6784c-158">Some command details to note:</span></span>

    * <span data-ttu-id="6784c-159">Sürüm numarası, her paket adı için `@` işaretini izler.</span><span class="sxs-lookup"><span data-stu-id="6784c-159">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="6784c-160">NPM bu özel paket sürümlerini yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="6784c-160">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="6784c-161">`-E` seçeneği, NPM 'nin [semantik sürüm](https://semver.org/) aralığı işleçlerini *Package. JSON*öğesine yazmanın varsayılan davranışını devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="6784c-161">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="6784c-162">Örneğin, `"webpack": "^4.41.5"`yerine `"webpack": "4.41.5"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6784c-162">For example, `"webpack": "4.41.5"` is used instead of `"webpack": "^4.41.5"`.</span></span> <span data-ttu-id="6784c-163">Bu seçenek, daha yeni paket sürümlerine istenmeden yükseltme yapılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="6784c-163">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="6784c-164">Daha fazla ayrıntı için bkz. [NPM-Install](https://docs.npmjs.com/cli/install) docs.</span><span class="sxs-lookup"><span data-stu-id="6784c-164">See the [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="6784c-165">*Package. JSON* dosyasının `scripts` özelliğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6784c-165">Replace the `scripts` property of the *package.json* file with the following code:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="6784c-166">Betiklerin bazı açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="6784c-166">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="6784c-167">`build`: istemci tarafı kaynaklarını geliştirme modunda paketler ve dosya değişikliklerini izler.</span><span class="sxs-lookup"><span data-stu-id="6784c-167">`build`: Bundles the client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="6784c-168">Dosya izleyici, bir proje dosyası her değiştiğinde paketin yeniden oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="6784c-168">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="6784c-169">`mode` seçeneği, Tree gerçekleşmesi ve minbirleşme gibi üretim iyileştirmeleri devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="6784c-169">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="6784c-170">Yalnızca geliştirmede `build` kullanın.</span><span class="sxs-lookup"><span data-stu-id="6784c-170">Only use `build` in development.</span></span>
    * <span data-ttu-id="6784c-171">`release`: istemci tarafı kaynaklarını üretim modunda Paketlayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-171">`release`: Bundles the client-side resources in production mode.</span></span>
    * <span data-ttu-id="6784c-172">`publish`: `release` betiği, istemci tarafı kaynaklarını üretim modunda paketleyip çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-172">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="6784c-173">Uygulamayı yayımlamak için .NET Core CLI [Publish](/dotnet/core/tools/dotnet-publish) komutunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-173">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="6784c-174">Aşağıdaki kodla, proje kökünde *WebPack. config. js*adlı bir dosya oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-174">Create a file named *webpack.config.js*, in the project root, with the following code:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="6784c-175">Yukarıdaki dosya Web paketi derlemesini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-175">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="6784c-176">Aklınızda bazı yapılandırma ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="6784c-176">Some configuration details to note:</span></span>

    * <span data-ttu-id="6784c-177">`output` özelliği, *dağ*'ın varsayılan değerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6784c-177">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="6784c-178">Paket, *Wwwroot* dizininde yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="6784c-178">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="6784c-179">`resolve.extensions` dizisi, SignalR istemci JavaScript 'ı içeri aktarmak için *. js* içerir.</span><span class="sxs-lookup"><span data-stu-id="6784c-179">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="6784c-180">Projenin istemci tarafı varlıklarını depolamak için proje kökünde yeni bir *src* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-180">Create a new *src* directory in the project root to store the project's client-side assets.</span></span>

1. <span data-ttu-id="6784c-181">Aşağıdaki biçimlendirmeyle *src/index.html* oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-181">Create *src/index.html* with the following markup.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="6784c-182">Önceki HTML, giriş sayfasının ortak işaretlemesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6784c-182">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="6784c-183">Yeni bir *src/CSS* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-183">Create a new *src/css* directory.</span></span> <span data-ttu-id="6784c-184">Amacı, projenin *. css* dosyalarını depolamadır.</span><span class="sxs-lookup"><span data-stu-id="6784c-184">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="6784c-185">Şu CSS ile *src/CSS/Main. css* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-185">Create *src/css/main.css* with the following CSS:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="6784c-186">Önceki *Main. css* dosyası uygulamayı stiller.</span><span class="sxs-lookup"><span data-stu-id="6784c-186">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="6784c-187">Şu JSON ile *src/tsconfig. JSON* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-187">Create *src/tsconfig.json* with the following JSON:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="6784c-188">Yukarıdaki kod, TypeScript derleyicisini [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript üretecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-188">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="6784c-189">Aşağıdaki kodla *src/index. TS* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-189">Create *src/index.ts* with the following code:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="6784c-190">Önceki TypeScript, DOM öğelerine başvuruları alır ve iki olay işleyicisini ekler:</span><span class="sxs-lookup"><span data-stu-id="6784c-190">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="6784c-191">`keyup`: Bu olay Kullanıcı `tbMessage`metin kutusuna yazdığında ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-191">`keyup`: This event fires when the user types in the `tbMessage`textbox.</span></span> <span data-ttu-id="6784c-192">`send` işlevi, Kullanıcı **ENTER** tuşuna bastığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6784c-192">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="6784c-193">`click`: Kullanıcı **Gönder** düğmesine tıkladığında bu olay ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-193">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="6784c-194">`send` işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="6784c-194">The `send` function is called.</span></span>

## <a name="configure-the-app"></a><span data-ttu-id="6784c-195">Uygulamayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6784c-195">Configure the app</span></span>

1. <span data-ttu-id="6784c-196">`Startup.Configure`, [Usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)öğesine çağrılar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6784c-196">In `Startup.Configure`, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=9-10)]

   <span data-ttu-id="6784c-197">Yukarıdaki kod, sunucunun *index. html* dosyasını bulmasını ve sunmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6784c-197">The preceding code allows the server to locate and serve the *index.html* file.</span></span>  <span data-ttu-id="6784c-198">Dosya, kullanıcının tam URL 'sini veya Web uygulamasının kök URL 'sini girip girmediğini görür.</span><span class="sxs-lookup"><span data-stu-id="6784c-198">The file is served whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="6784c-199">`Startup.Configure`sonunda, bir */hub* yolunu `ChatHub` hub 'ına eşleyin.</span><span class="sxs-lookup"><span data-stu-id="6784c-199">At the end of `Startup.Configure`, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="6784c-200">Merhaba Dünya görüntülenen kodu değiştirin *!*</span><span class="sxs-lookup"><span data-stu-id="6784c-200">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="6784c-201">Aşağıdaki satırla:</span><span class="sxs-lookup"><span data-stu-id="6784c-201">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="6784c-202">`Startup.ConfigureServices`' de [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)öğesini çağırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-202">In `Startup.ConfigureServices`, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="6784c-203">SignalR hub 'ını depolamak için proje kök *Signalrwebpack/* içindeki *hub* adlı yeni bir dizin oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-203">Create a new directory named *Hubs* in the project root *SignalRWebPack/* to store the SignalR hub.</span></span>

1. <span data-ttu-id="6784c-204">Aşağıdaki kodla hub *hub 'ları/ChatHub. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-204">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="6784c-205">`ChatHub` başvurusunu çözümlemek için aşağıdaki `using` ifadesini *Startup.cs* dosyasının üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-205">Add the following `using` statement at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="6784c-206">İstemci ve sunucu iletişimini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="6784c-206">Enable client and server communication</span></span>

<span data-ttu-id="6784c-207">Uygulama Şu anda ileti göndermek için temel bir form görüntülüyor, ancak henüz işlevsel değil.</span><span class="sxs-lookup"><span data-stu-id="6784c-207">The app currently displays a basic form to send messages, but is not yet functional.</span></span> <span data-ttu-id="6784c-208">Sunucu belirli bir yolu dinliyor, ancak gönderilen iletilerle hiçbir şey yapmıyor.</span><span class="sxs-lookup"><span data-stu-id="6784c-208">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="6784c-209">Proje kökünde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-209">Run the following command at the project root:</span></span>

    ```console
    npm i @microsoft/signalr @types/node
    ```

    <span data-ttu-id="6784c-210">Önceki komut yüklenir:</span><span class="sxs-lookup"><span data-stu-id="6784c-210">The preceding command installs:</span></span>

     * <span data-ttu-id="6784c-211">İstemcinin sunucuya ileti göndermesini sağlayan [SignalR TypeScript istemcisi](https://www.npmjs.com/package/@microsoft/signalr).</span><span class="sxs-lookup"><span data-stu-id="6784c-211">The [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>
     * <span data-ttu-id="6784c-212">Node. js türlerinin derleme zamanı denetimini sağlayan Node. js için TypeScript tür tanımları.</span><span class="sxs-lookup"><span data-stu-id="6784c-212">The TypeScript type definitions for Node.js, which enables compile-time checking of Node.js types.</span></span>

1. <span data-ttu-id="6784c-213">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-213">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="6784c-214">Yukarıdaki kod, sunucudan ileti almayı destekler.</span><span class="sxs-lookup"><span data-stu-id="6784c-214">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="6784c-215">`HubConnectionBuilder` sınıfı, sunucu bağlantısını yapılandırmak için yeni bir Oluşturucu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6784c-215">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="6784c-216">`withUrl` işlevi hub URL 'sini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-216">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="6784c-217">SignalR, istemci ile sunucu arasında ileti alışverişi yapılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="6784c-217">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="6784c-218">Her ileti belirli bir ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6784c-218">Each message has a specific name.</span></span> <span data-ttu-id="6784c-219">Örneğin, `messageReceived` adlı iletiler, ileti bölgesindeki yeni iletiyi görüntülemeden sorumlu mantığı çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-219">For example, messages with the name `messageReceived` can run the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="6784c-220">Belirli bir iletiyi dinlemek `on` işlevi aracılığıyla yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-220">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="6784c-221">Herhangi bir sayıda ileti adı ile listenlebilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-221">Any number of message names can be listened to.</span></span> <span data-ttu-id="6784c-222">Ayrıca, yazarın adı ve alınan iletinin içeriği gibi parametreleri iletiye geçirmek da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6784c-222">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="6784c-223">İstemci bir ileti aldıktan sonra yazarın adı ve `innerHTML` özniteliğinde ileti içeriğiyle yeni bir `div` öğesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6784c-223">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="6784c-224">Bu, iletileri görüntüleyen ana `div` öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-224">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="6784c-225">Artık istemci bir ileti aldığına göre, ileti gönderecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-225">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="6784c-226">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-226">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="6784c-227">WebSockets bağlantısı aracılığıyla bir ileti gönderildiğinde `send` yönteminin çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6784c-227">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="6784c-228">Yöntemin ilk parametresi ileti adıdır.</span><span class="sxs-lookup"><span data-stu-id="6784c-228">The method's first parameter is the message name.</span></span> <span data-ttu-id="6784c-229">İleti verileri diğer parametreleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6784c-229">The message data inhabits the other parameters.</span></span> <span data-ttu-id="6784c-230">Bu örnekte, `newMessage` sunucusuna gönderildiği gibi bir ileti gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-230">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="6784c-231">İleti, bir metin kutusundan Kullanıcı adından ve Kullanıcı girişinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="6784c-231">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="6784c-232">Gönderme işlemi çalışırsa metin kutusu değeri temizlenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-232">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="6784c-233">`NewMessage` yönetimini `ChatHub` sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-233">Add the `NewMessage` method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="6784c-234">Yukarıdaki kod, sunucu onları aldıktan sonra tüm bağlı kullanıcılara ileti almış iletileri yayınlar.</span><span class="sxs-lookup"><span data-stu-id="6784c-234">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="6784c-235">Tüm iletileri almak için genel bir `on` yöntemi olması gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="6784c-235">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="6784c-236">İleti adından sonra adlandırılmış bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="6784c-236">A method named after the message name suffices.</span></span>

    <span data-ttu-id="6784c-237">Bu örnekte, TypeScript istemcisi `newMessage`olarak tanımlanan bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="6784c-237">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="6784c-238">C# `NewMessage` yöntemi, istemci tarafından gönderilen verileri bekler.</span><span class="sxs-lookup"><span data-stu-id="6784c-238">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="6784c-239">Istemcilerde [Sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) çağrısı yapılır [. tümü](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="6784c-239">A call is made to [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="6784c-240">Alınan iletiler, hub 'a bağlı tüm istemcilere gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-240">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="6784c-241">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="6784c-241">Test the app</span></span>

<span data-ttu-id="6784c-242">Uygulamanın aşağıdaki adımlarla çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="6784c-242">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6784c-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6784c-243">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6784c-244">Web paketini *yayın* modunda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-244">Run Webpack in *release* mode.</span></span> <span data-ttu-id="6784c-245">**Paket Yöneticisi konsol** penceresini kullanarak, proje kökünde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-245">Using the **Package Manager Console** window, run the following command in the project root.</span></span> <span data-ttu-id="6784c-246">Proje kökünde değilseniz, komutu girmeden önce `cd SignalRWebPack` girin.</span><span class="sxs-lookup"><span data-stu-id="6784c-246">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6784c-247">Hata ayıklayıcıyı eklemeden uygulamayı bir tarayıcıda başlatmak \*\*için hata ayıklama > \*\* **Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-247">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="6784c-248">*Wwwroot/index.html* dosyası `http://localhost:<port_number>`olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="6784c-248">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="6784c-249">Derleme hataları alırsanız, çözümü kapatıp yeniden açmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="6784c-249">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="6784c-250">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="6784c-250">Open another browser instance (any browser).</span></span> <span data-ttu-id="6784c-251">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-251">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6784c-252">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-252">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6784c-253">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-253">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6784c-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="6784c-255">Proje kökünde aşağıdaki komutu yürüterek Web paketini *yayın* modunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-255">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6784c-256">Proje kökünde aşağıdaki komutu yürüterek uygulamayı derleyin ve çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-256">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="6784c-257">Web sunucusu uygulamayı başlatır ve localhost üzerinde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="6784c-257">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="6784c-258">`http://localhost:<port_number>`için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="6784c-258">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="6784c-259">*Wwwroot/index.html* dosyası sunulur.</span><span class="sxs-lookup"><span data-stu-id="6784c-259">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="6784c-260">Adres çubuğundan URL 'YI kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-260">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="6784c-261">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="6784c-261">Open another browser instance (any browser).</span></span> <span data-ttu-id="6784c-262">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-262">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6784c-263">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-263">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6784c-264">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-264">The unique user name and message are displayed on both pages instantly.</span></span>

---

![ileti hem tarayıcı penceresinde görüntüleniyor](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="6784c-266">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6784c-266">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6784c-267">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6784c-267">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6784c-268">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="6784c-268">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6784c-269">.NET Core SDK 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="6784c-269">.NET Core SDK 2.2 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* <span data-ttu-id="6784c-270">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="6784c-270">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6784c-271">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-271">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6784c-272">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-272">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6784c-273">.NET Core SDK 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="6784c-273">.NET Core SDK 2.2 or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)
* [<span data-ttu-id="6784c-274">C#Visual Studio Code sürüm 1.17.1 veya üzeri için</span><span class="sxs-lookup"><span data-stu-id="6784c-274">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* <span data-ttu-id="6784c-275">[NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="6784c-275">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="6784c-276">ASP.NET Core Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6784c-276">Create the ASP.NET Core web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6784c-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6784c-277">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6784c-278">Visual Studio 'Yu, *Path* ortam değişkeninde NPM için arama yapmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-278">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="6784c-279">Varsayılan olarak, Visual Studio yükleme dizininde bulunan NPM sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="6784c-279">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="6784c-280">Visual Studio 'da şu yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-280">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="6784c-281">**Web Paket Yönetimi** > **dış web araçları**> > > **Araçlar** ve **Seçenekler** **'** e gidin.</span><span class="sxs-lookup"><span data-stu-id="6784c-281">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="6784c-282">Listeden *$ (yol)* girişini seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-282">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="6784c-283">Girdiyi listedeki ikinci konuma taşımak için yukarı oka tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-283">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="6784c-285">Visual Studio yapılandırması tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="6784c-285">Visual Studio configuration is completed.</span></span> <span data-ttu-id="6784c-286">Projeyi oluşturma zamanı.</span><span class="sxs-lookup"><span data-stu-id="6784c-286">It's time to create the project.</span></span>

1. <span data-ttu-id="6784c-287">**Dosya** > **Yeni** > **projesi** menü seçeneğini kullanın ve **ASP.NET Core Web uygulaması** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-287">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="6784c-288">Projeyi *Signalrwebpack*olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-288">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="6784c-289">Hedef çerçeve açılır listesinden *.NET Core* ' u seçin ve çerçeve Seçicisi açılır listesinden *ASP.NET Core 2,2* ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-289">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="6784c-290">**Boş** şablonu seçin ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-290">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6784c-291">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-291">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6784c-292">**Tümleşik terminalde**aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-292">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="6784c-293">.NET Core 'u hedefleyen boş bir ASP.NET Core Web uygulaması, bir *Signalrwebpack* dizininde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6784c-293">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="6784c-294">WebPack ve TypeScript yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6784c-294">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="6784c-295">Aşağıdaki adımlar, TypeScript 'in JavaScript 'e dönüştürülmesini ve istemci tarafı kaynaklarını paketlemeyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-295">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="6784c-296">Bir *Package. JSON* dosyası oluşturmak için proje kökünde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-296">Run the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="6784c-297">Vurgulanan özelliği *Package. JSON* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-297">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="6784c-298">`private` özelliğinin `true` olarak ayarlanması, sonraki adımda paket yükleme uyarılarını önler.</span><span class="sxs-lookup"><span data-stu-id="6784c-298">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="6784c-299">Gerekli NPM paketlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="6784c-299">Install the required npm packages.</span></span> <span data-ttu-id="6784c-300">Proje kökünden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-300">Run the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="6784c-301">Aklınızda bazı komut ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="6784c-301">Some command details to note:</span></span>

    * <span data-ttu-id="6784c-302">Sürüm numarası, her paket adı için `@` işaretini izler.</span><span class="sxs-lookup"><span data-stu-id="6784c-302">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="6784c-303">NPM bu özel paket sürümlerini yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="6784c-303">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="6784c-304">`-E` seçeneği, NPM 'nin [semantik sürüm](https://semver.org/) aralığı işleçlerini *Package. JSON*öğesine yazmanın varsayılan davranışını devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="6784c-304">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="6784c-305">Örneğin, `"webpack": "^4.29.3"`yerine `"webpack": "4.29.3"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6784c-305">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="6784c-306">Bu seçenek, daha yeni paket sürümlerine istenmeden yükseltme yapılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="6784c-306">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="6784c-307">Daha fazla ayrıntı için bkz. [NPM-Install](https://docs.npmjs.com/cli/install) docs.</span><span class="sxs-lookup"><span data-stu-id="6784c-307">See the [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="6784c-308">*Package. JSON* dosyasının `scripts` özelliğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6784c-308">Replace the `scripts` property of the *package.json* file with the following code:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="6784c-309">Betiklerin bazı açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="6784c-309">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="6784c-310">`build`: istemci tarafı kaynaklarını geliştirme modunda paketler ve dosya değişikliklerini izler.</span><span class="sxs-lookup"><span data-stu-id="6784c-310">`build`: Bundles the client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="6784c-311">Dosya izleyici, bir proje dosyası her değiştiğinde paketin yeniden oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="6784c-311">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="6784c-312">`mode` seçeneği, Tree gerçekleşmesi ve minbirleşme gibi üretim iyileştirmeleri devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="6784c-312">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="6784c-313">Yalnızca geliştirmede `build` kullanın.</span><span class="sxs-lookup"><span data-stu-id="6784c-313">Only use `build` in development.</span></span>
    * <span data-ttu-id="6784c-314">`release`: istemci tarafı kaynaklarını üretim modunda Paketlayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-314">`release`: Bundles the client-side resources in production mode.</span></span>
    * <span data-ttu-id="6784c-315">`publish`: `release` betiği, istemci tarafı kaynaklarını üretim modunda paketleyip çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-315">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="6784c-316">Uygulamayı yayımlamak için .NET Core CLI [Publish](/dotnet/core/tools/dotnet-publish) komutunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-316">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="6784c-317">Aşağıdaki kodla, proje kökünde *WebPack. config. js* adlı bir dosya oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-317">Create a file named *webpack.config.js* in the project root, with the following code:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="6784c-318">Yukarıdaki dosya Web paketi derlemesini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-318">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="6784c-319">Aklınızda bazı yapılandırma ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="6784c-319">Some configuration details to note:</span></span>

    * <span data-ttu-id="6784c-320">`output` özelliği, *dağ*'ın varsayılan değerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6784c-320">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="6784c-321">Paket, *Wwwroot* dizininde yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="6784c-321">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="6784c-322">`resolve.extensions` dizisi, SignalR istemci JavaScript 'ı içeri aktarmak için *. js* içerir.</span><span class="sxs-lookup"><span data-stu-id="6784c-322">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="6784c-323">Projenin istemci tarafı varlıklarını depolamak için proje kökünde yeni bir *src* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-323">Create a new *src* directory in the project root to store the project's client-side assets.</span></span>

1. <span data-ttu-id="6784c-324">Aşağıdaki biçimlendirmeyle *src/index.html* oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-324">Create *src/index.html* with the following markup.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="6784c-325">Önceki HTML, giriş sayfasının ortak işaretlemesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6784c-325">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="6784c-326">Yeni bir *src/CSS* dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-326">Create a new *src/css* directory.</span></span> <span data-ttu-id="6784c-327">Amacı, projenin *. css* dosyalarını depolamadır.</span><span class="sxs-lookup"><span data-stu-id="6784c-327">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="6784c-328">Şu biçimlendirmeye sahip *src/CSS/Main. css* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-328">Create *src/css/main.css* with the following markup:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="6784c-329">Önceki *Main. css* dosyası uygulamayı stiller.</span><span class="sxs-lookup"><span data-stu-id="6784c-329">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="6784c-330">Şu JSON ile *src/tsconfig. JSON* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-330">Create *src/tsconfig.json* with the following JSON:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="6784c-331">Yukarıdaki kod, TypeScript derleyicisini [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript üretecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-331">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="6784c-332">Aşağıdaki kodla *src/index. TS* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-332">Create *src/index.ts* with the following code:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="6784c-333">Önceki TypeScript, DOM öğelerine başvuruları alır ve iki olay işleyicisini ekler:</span><span class="sxs-lookup"><span data-stu-id="6784c-333">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="6784c-334">`keyup`: Bu olay Kullanıcı `tbMessage` metin kutusuna yazdığında ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-334">`keyup`: This event fires when the user types in the `tbMessage` textbox.</span></span> <span data-ttu-id="6784c-335">`send` işlevi, Kullanıcı **ENTER** tuşuna bastığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6784c-335">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="6784c-336">`click`: Kullanıcı **Gönder** düğmesine tıkladığında bu olay ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-336">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="6784c-337">`send` işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="6784c-337">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="6784c-338">ASP.NET Core uygulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6784c-338">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="6784c-339">`Startup.Configure` yönteminde belirtilen kod *Merhaba Dünya!* görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="6784c-339">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="6784c-340">`app.Run` yöntemi çağrısını, [Usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)çağrılarıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6784c-340">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="6784c-341">Yukarıdaki kod, kullanıcının tam URL 'sini veya Web uygulamasının kök URL 'sini girmeksizin *Dizin. html* dosyasını bulmasını ve sunmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6784c-341">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="6784c-342">`Startup.ConfigureServices`içinde [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="6784c-342">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6784c-343">SignalR hizmetlerini projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="6784c-343">It adds the SignalR services to the project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="6784c-344">Bir */hub* yolunu `ChatHub` hub 'ına eşleyin.</span><span class="sxs-lookup"><span data-stu-id="6784c-344">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="6784c-345">`Startup.Configure`sonuna aşağıdaki satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-345">Add the following lines at the end of `Startup.Configure`:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="6784c-346">Proje kökünde *hub*olarak adlandırılan yeni bir dizin oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6784c-346">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="6784c-347">Amacı, bir sonraki adımda oluşturulan SignalR hub 'ını deposağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="6784c-347">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="6784c-348">Aşağıdaki kodla hub *hub 'ları/ChatHub. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="6784c-348">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="6784c-349">`ChatHub` başvurusunu çözümlemek için, *Startup.cs* dosyasının en üstüne aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-349">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="6784c-350">İstemci ve sunucu iletişimini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="6784c-350">Enable client and server communication</span></span>

<span data-ttu-id="6784c-351">Uygulama Şu anda ileti göndermek için basit bir form görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="6784c-351">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="6784c-352">Bunu yapmayı denediğinizde hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="6784c-352">Nothing happens when you try to do so.</span></span> <span data-ttu-id="6784c-353">Sunucu belirli bir yolu dinliyor, ancak gönderilen iletilerle hiçbir şey yapmıyor.</span><span class="sxs-lookup"><span data-stu-id="6784c-353">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="6784c-354">Proje kökünde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-354">Run the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="6784c-355">Yukarıdaki komut, istemcisinin sunucuya ileti göndermesini sağlayan [SignalR TypeScript istemcisini](https://www.npmjs.com/package/@microsoft/signalr)yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="6784c-355">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="6784c-356">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-356">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="6784c-357">Yukarıdaki kod, sunucudan ileti almayı destekler.</span><span class="sxs-lookup"><span data-stu-id="6784c-357">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="6784c-358">`HubConnectionBuilder` sınıfı, sunucu bağlantısını yapılandırmak için yeni bir Oluşturucu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6784c-358">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="6784c-359">`withUrl` işlevi hub URL 'sini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6784c-359">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="6784c-360">SignalR, istemci ile sunucu arasında ileti alışverişi yapılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="6784c-360">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="6784c-361">Her ileti belirli bir ada sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6784c-361">Each message has a specific name.</span></span> <span data-ttu-id="6784c-362">Örneğin, `messageReceived` adlı iletiler, ileti bölgesindeki yeni iletiyi görüntülemeden sorumlu mantığı çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-362">For example, messages with the name `messageReceived` can run the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="6784c-363">Belirli bir iletiyi dinlemek `on` işlevi aracılığıyla yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-363">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="6784c-364">Herhangi bir sayıda ileti adını dinleyebilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6784c-364">You can listen to any number of message names.</span></span> <span data-ttu-id="6784c-365">Ayrıca, yazarın adı ve alınan iletinin içeriği gibi parametreleri iletiye geçirmek da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6784c-365">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="6784c-366">İstemci bir ileti aldıktan sonra yazarın adı ve `innerHTML` özniteliğinde ileti içeriğiyle yeni bir `div` öğesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6784c-366">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="6784c-367">Yeni ileti, iletileri görüntüleyen ana `div` öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-367">The new message is added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="6784c-368">Artık istemci bir ileti aldığına göre, ileti gönderecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-368">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="6784c-369">Vurgulanan kodu *src/index. TS* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-369">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="6784c-370">WebSockets bağlantısı aracılığıyla bir ileti gönderildiğinde `send` yönteminin çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6784c-370">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="6784c-371">Yöntemin ilk parametresi ileti adıdır.</span><span class="sxs-lookup"><span data-stu-id="6784c-371">The method's first parameter is the message name.</span></span> <span data-ttu-id="6784c-372">İleti verileri diğer parametreleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6784c-372">The message data inhabits the other parameters.</span></span> <span data-ttu-id="6784c-373">Bu örnekte, `newMessage` sunucusuna gönderildiği gibi bir ileti gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-373">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="6784c-374">İleti, bir metin kutusundan Kullanıcı adından ve Kullanıcı girişinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="6784c-374">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="6784c-375">Gönderme işlemi çalışırsa metin kutusu değeri temizlenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-375">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="6784c-376">`NewMessage` yönetimini `ChatHub` sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6784c-376">Add the `NewMessage` method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="6784c-377">Yukarıdaki kod, sunucu onları aldıktan sonra tüm bağlı kullanıcılara ileti almış iletileri yayınlar.</span><span class="sxs-lookup"><span data-stu-id="6784c-377">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="6784c-378">Tüm iletileri almak için genel bir `on` yöntemi olması gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="6784c-378">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="6784c-379">İleti adından sonra adlandırılmış bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="6784c-379">A method named after the message name suffices.</span></span>

    <span data-ttu-id="6784c-380">Bu örnekte, TypeScript istemcisi `newMessage`olarak tanımlanan bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="6784c-380">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="6784c-381">C# `NewMessage` yöntemi, istemci tarafından gönderilen verileri bekler.</span><span class="sxs-lookup"><span data-stu-id="6784c-381">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="6784c-382">Istemcilerde [Sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) çağrısı yapılır [. tümü](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="6784c-382">A call is made to [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="6784c-383">Alınan iletiler, hub 'a bağlı tüm istemcilere gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6784c-383">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="6784c-384">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="6784c-384">Test the app</span></span>

<span data-ttu-id="6784c-385">Uygulamanın aşağıdaki adımlarla çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="6784c-385">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6784c-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6784c-386">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6784c-387">Web paketini *yayın* modunda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-387">Run Webpack in *release* mode.</span></span> <span data-ttu-id="6784c-388">**Paket Yöneticisi konsol** penceresini kullanarak, proje kökünde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-388">Using the **Package Manager Console** window, run the following command in the project root.</span></span> <span data-ttu-id="6784c-389">Proje kökünde değilseniz, komutu girmeden önce `cd SignalRWebPack` girin.</span><span class="sxs-lookup"><span data-stu-id="6784c-389">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6784c-390">Hata ayıklayıcıyı eklemeden uygulamayı bir tarayıcıda başlatmak \*\*için hata ayıklama > \*\* **Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="6784c-390">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="6784c-391">*Wwwroot/index.html* dosyası `http://localhost:<port_number>`olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="6784c-391">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="6784c-392">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="6784c-392">Open another browser instance (any browser).</span></span> <span data-ttu-id="6784c-393">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-393">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6784c-394">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-394">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6784c-395">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-395">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6784c-396">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6784c-396">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="6784c-397">Proje kökünde aşağıdaki komutu yürüterek Web paketini *yayın* modunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-397">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6784c-398">Proje kökünde aşağıdaki komutu yürüterek uygulamayı derleyin ve çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6784c-398">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="6784c-399">Web sunucusu uygulamayı başlatır ve localhost üzerinde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="6784c-399">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="6784c-400">`http://localhost:<port_number>`için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="6784c-400">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="6784c-401">*Wwwroot/index.html* dosyası sunulur.</span><span class="sxs-lookup"><span data-stu-id="6784c-401">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="6784c-402">Adres çubuğundan URL 'YI kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-402">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="6784c-403">Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın.</span><span class="sxs-lookup"><span data-stu-id="6784c-403">Open another browser instance (any browser).</span></span> <span data-ttu-id="6784c-404">URL 'YI adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="6784c-404">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6784c-405">Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6784c-405">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6784c-406">Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6784c-406">The unique user name and message are displayed on both pages instantly.</span></span>

---

![ileti hem tarayıcı penceresinde görüntüleniyor](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6784c-408">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6784c-408">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
