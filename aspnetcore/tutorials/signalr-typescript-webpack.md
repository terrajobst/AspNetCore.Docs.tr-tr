---
title: ASP.NET Core SignalR TypeScript ve Webpack ile kullanma
author: ssougnez
description: Bu öğreticide, paket ve, istemci TypeScript içinde yazılmış bir ASP.NET Core SignalR web uygulaması oluşturmak için Webpack yapılandırın.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126431"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="d6526-103">ASP.NET Core SignalR TypeScript ve Webpack ile kullanma</span><span class="sxs-lookup"><span data-stu-id="d6526-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="d6526-104">Tarafından [Sébastien Sougnez](https://twitter.com/ssougnez) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d6526-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d6526-105">[Webpack](https://webpack.js.org/) paketini ve web uygulamasının istemci-tarafı kaynakları derlemeleri geliştiricileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6526-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="d6526-106">Bu öğretici, istemci yazılmış bir ASP.NET Core SignalR web uygulamasında Webpack kullanarak gösterir [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="d6526-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="d6526-107">Bu öğreticide, bilgi nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="d6526-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d6526-108">Bir başlangıç ASP.NET Core SignalR uygulaması iskele</span><span class="sxs-lookup"><span data-stu-id="d6526-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="d6526-109">SignalR TypeScript istemciyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6526-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="d6526-110">Webpack kullanarak bir yapı ardışık düzenini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="d6526-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="d6526-111">SignalR sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="d6526-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="d6526-112">İstemci ve sunucu arasındaki iletişimi etkinleştirmek</span><span class="sxs-lookup"><span data-stu-id="d6526-112">Enable communication between client and server</span></span>

<span data-ttu-id="d6526-113">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6526-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6526-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d6526-114">Prerequisites</span></span>

<span data-ttu-id="d6526-115">Aşağıdaki yazılımı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d6526-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6526-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6526-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="d6526-117">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d6526-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d6526-118">[Node.js](https://nodejs.org/) ile [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="d6526-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="d6526-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) sürüm 15.7 veya üstü **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="d6526-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d6526-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d6526-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="d6526-121">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d6526-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d6526-122">[Node.js](https://nodejs.org/) ile [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="d6526-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="d6526-123">ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="d6526-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6526-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6526-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d6526-125">Npm aramak için Visual Studio'yu yapılandırma *yolu* ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d6526-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="d6526-126">Varsayılan olarak, Visual Studio kendi yükleme dizininde bulunan npm sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="d6526-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="d6526-127">Visual Studio'da bu yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="d6526-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="d6526-128">Gidin **Araçları** > **seçenekleri** > **projeler ve çözümler** > **Web paket Yönetimi**  >  **Dış Web Araçları**.</span><span class="sxs-lookup"><span data-stu-id="d6526-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="d6526-129">Seçin *$(PATH)* listeden girişi.</span><span class="sxs-lookup"><span data-stu-id="d6526-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="d6526-130">Giriş listede ikinci konuma taşımak için yukarı oka tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d6526-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="d6526-131">Bir kenara ilk giriş projenin yerel paketlerine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="d6526-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="d6526-133">Visual Studio yapılandırması tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="d6526-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="d6526-134">Projeyi oluşturmak için zaman yapılır.</span><span class="sxs-lookup"><span data-stu-id="d6526-134">It's time to create the project.</span></span>

1. <span data-ttu-id="d6526-135">Kullanım **dosya** > **yeni** > **proje** menü seçeneğini ve seçin **ASP.NET çekirdek Web uygulaması** Şablon.</span><span class="sxs-lookup"><span data-stu-id="d6526-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="d6526-136">Proje adı *SignalRWebPack*, tıklatıp **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d6526-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="d6526-137">Seçin *.NET Core* açılır ve select hedef çerçeveden *ASP.NET Core 2.1* framework Seçici açılan'den.</span><span class="sxs-lookup"><span data-stu-id="d6526-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="d6526-138">Seçin **boş** şablonu ve tıklatın **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d6526-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d6526-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d6526-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d6526-140">Aşağıdaki komutu çalıştırın **tümleşik Terminal**:</span><span class="sxs-lookup"><span data-stu-id="d6526-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="d6526-141">Boş bir ASP.NET Core web uygulaması .NET Core hedefleme oluşturulan bir *SignalRWebPack* dizini.</span><span class="sxs-lookup"><span data-stu-id="d6526-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="d6526-142">Webpack ve TypeScript yapılandırın</span><span class="sxs-lookup"><span data-stu-id="d6526-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="d6526-143">Aşağıdaki adımları TypeScript JavaScript dönüştürülmesi ve istemci tarafı kaynaklarını paketleme yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d6526-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="d6526-144">Proje kök dizininde oluşturmak için aşağıdaki komutu yürütün bir *package.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d6526-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="d6526-145">Vurgulanan özelliği Ekle *package.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d6526-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="d6526-146">Ayarı `private` özelliğine `true` paket yükleme uyarıların bir sonraki adımda engeller.</span><span class="sxs-lookup"><span data-stu-id="d6526-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="d6526-147">Gerekli npm paketlerini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d6526-147">Install the required npm packages.</span></span> <span data-ttu-id="d6526-148">Proje kökünden aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6526-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="d6526-149">Dikkat edilecek bazı komut ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="d6526-149">Some command details to note:</span></span>

    * <span data-ttu-id="d6526-150">Bir sürüm numarası izleyen `@` imzalamak için her paket adı.</span><span class="sxs-lookup"><span data-stu-id="d6526-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="d6526-151">npm, bu özel paketi sürümleri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d6526-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="d6526-152">`-E` Seçeneği, yazma npm'ın varsayılan davranışı devre dışı bırakır [anlamsal sürüm oluşturma](https://semver.org/) aralığı işleçleri *package.json*.</span><span class="sxs-lookup"><span data-stu-id="d6526-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="d6526-153">Örneğin, `"webpack": "4.12.0"` yerine kullanılan `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="d6526-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="d6526-154">Bu seçenek, yeni paketi sürümleri istenmeyen yükseltmeler önler.</span><span class="sxs-lookup"><span data-stu-id="d6526-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="d6526-155">Resmi görmek [npm yükleme](https://docs.npmjs.com/cli/install) daha fazla ayrıntı için belgeleri.</span><span class="sxs-lookup"><span data-stu-id="d6526-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="d6526-156">Değiştir `scripts` özelliği *package.json* aşağıdaki kod parçacığıyla dosyası:</span><span class="sxs-lookup"><span data-stu-id="d6526-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="d6526-157">Komut dosyaları bazı açıklaması:</span><span class="sxs-lookup"><span data-stu-id="d6526-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="d6526-158">`build`: Sunmaktadır istemci-tarafı kaynaklarınızı geliştirme modunda ve için dosya değişiklikleri izler.</span><span class="sxs-lookup"><span data-stu-id="d6526-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="d6526-159">Dosya İzleyici proje dosya değişiklikleri her zaman yeniden oluşturmak paket neden olur.</span><span class="sxs-lookup"><span data-stu-id="d6526-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="d6526-160">`mode` Seçeneği, ağaç sallayabilir ve küçültme gibi üretim iyileştirmeleri devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="d6526-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="d6526-161">Yalnızca `build` geliştirme.</span><span class="sxs-lookup"><span data-stu-id="d6526-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="d6526-162">`release`: İstemci-tarafı kaynaklarınızı üretim modunda sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d6526-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="d6526-163">`publish`: Çalışan `release` üretim modunda istemci-tarafı kaynaklar gruplanacağını komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="d6526-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="d6526-164">.NET Core CLI çağırır [yayımlama](/dotnet/core/tools/dotnet-publish) uygulama yayımlamak için komutu.</span><span class="sxs-lookup"><span data-stu-id="d6526-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="d6526-165">Adlı bir dosya oluşturun *webpack.config.js*, proje kök aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="d6526-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="d6526-166">Önceki dosya Webpack derleme yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6526-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="d6526-167">Dikkat edilecek bazı yapılandırma ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="d6526-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="d6526-168">`output` Özelliği varsayılan değerini geçersiz kılar *dağ*.</span><span class="sxs-lookup"><span data-stu-id="d6526-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="d6526-169">Paket bunun yerine, gösterilen *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="d6526-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="d6526-170">`resolve.extensions` Dizi içeriyor *.js* SignalR istemci JavaScript almak için.</span><span class="sxs-lookup"><span data-stu-id="d6526-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="d6526-171">Yeni bir *src* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="d6526-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="d6526-172">Amacı, projenin istemci-tarafı varlıkları depolamaktır.</span><span class="sxs-lookup"><span data-stu-id="d6526-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="d6526-173">Oluşturma *src/index.html* aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="d6526-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="d6526-174">Önceki HTML giriş sayfası'nın Demirbaş biçimlendirme tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d6526-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="d6526-175">Yeni bir *src/css* dizin.</span><span class="sxs-lookup"><span data-stu-id="d6526-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="d6526-176">Amacı projenin depolamaktır *.css* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="d6526-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="d6526-177">Oluşturma *src/css/main.css* aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="d6526-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="d6526-178">Yukarıdaki *main.css* dosya stiller uygulama.</span><span class="sxs-lookup"><span data-stu-id="d6526-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="d6526-179">Oluşturma *src/tsconfig.json* aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="d6526-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="d6526-180">Önceki kod üretmek için TypeScript derleyici yapılandırır [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d6526-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="d6526-181">Oluşturma *src/index.ts* aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="d6526-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="d6526-182">Önceki TypeScript DOM öğelere başvurular alır ve iki olay işleyicileri ekler:</span><span class="sxs-lookup"><span data-stu-id="d6526-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="d6526-183">`keyup`: Kullanıcı bir şey olarak tanımlanan metin kutusuna yazdığında bu olay harekete `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="d6526-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="d6526-184">`send` Çağrıldığında kullanıcı bastığında **Enter** anahtarı.</span><span class="sxs-lookup"><span data-stu-id="d6526-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="d6526-185">`click`: Bu olay kullanıcı tıklattığında ateşlenir **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d6526-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="d6526-186">`send` İşlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d6526-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="d6526-187">ASP.NET Core uygulamayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6526-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="d6526-188">Sağlanan kod `Startup.Configure` yöntemi görüntüler *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="d6526-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="d6526-189">Değiştir `app.Run` yöntemi çağrısı çağrıları ile [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="d6526-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="d6526-190">Önceki kod bulun ve hizmet sunucuya verir *index.html* tam URL'sini veya web uygulamasının kök URL'si kullanıcının girdiği olup olmadığını dosya.</span><span class="sxs-lookup"><span data-stu-id="d6526-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="d6526-191">Çağrı [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) içinde `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d6526-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d6526-192">SignalR Hizmetleri projenize ekler.</span><span class="sxs-lookup"><span data-stu-id="d6526-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="d6526-193">Harita bir */hub* yönlendirmek `ChatHub` hub.</span><span class="sxs-lookup"><span data-stu-id="d6526-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="d6526-194">Sonuna şu satırları ekleyin `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d6526-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="d6526-195">Adlı yeni bir dizin oluşturma *hub*, proje kök.</span><span class="sxs-lookup"><span data-stu-id="d6526-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="d6526-196">Amacı sonraki adımda oluşturduğunuz SignalR hub depolamaktır.</span><span class="sxs-lookup"><span data-stu-id="d6526-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="d6526-197">Hub'ı oluşturma *Hubs/ChatHub.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="d6526-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="d6526-198">En üstte aşağıdaki kodu ekleyin *haline* çözümlemek için dosya `ChatHub` başvurusu:</span><span class="sxs-lookup"><span data-stu-id="d6526-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="d6526-199">İstemci ve sunucu iletişimi etkinleştir</span><span class="sxs-lookup"><span data-stu-id="d6526-199">Enable client and server communication</span></span>

<span data-ttu-id="d6526-200">Uygulama şu anda iletileri göndermek için basit bir form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d6526-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="d6526-201">Bunu yapmak denediğinizde hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="d6526-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="d6526-202">Sunucu, özel bir yol dinleme ancak gönderilen iletilerle hiçbir şey yapmaz.</span><span class="sxs-lookup"><span data-stu-id="d6526-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="d6526-203">Proje kökündeki aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6526-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="d6526-204">Yukarıdaki komut yükler [SignalR TypeScript istemci](https://www.npmjs.com/package/@aspnet/signalr), sunucuya iletileri göndermek istemci sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6526-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="d6526-205">Vurgulanmış kodu ekleyin *src/index.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d6526-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="d6526-206">Önceki kod iletileri sunucudan alma destekler.</span><span class="sxs-lookup"><span data-stu-id="d6526-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="d6526-207">`HubConnectionBuilder` Sınıfı sunucu bağlantısı yapılandırmak için yeni bir oluşturucusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d6526-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="d6526-208">`withUrl` İşlevi hub URL'sini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d6526-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="d6526-209">SignalR iletileri bir istemci ve sunucu arasında exchange sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6526-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="d6526-210">Her ileti belirli bir adı vardır.</span><span class="sxs-lookup"><span data-stu-id="d6526-210">Each message has a specific name.</span></span> <span data-ttu-id="d6526-211">Örneğin, iletileri ada sahip olabilir `messageReceived` yeni ileti iletileri bölgesinde görüntülemek için sorumlu mantığı yürütün.</span><span class="sxs-lookup"><span data-stu-id="d6526-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="d6526-212">Aracılığıyla belirli bir ileti dinleme yapılabilir `on` işlevi.</span><span class="sxs-lookup"><span data-stu-id="d6526-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="d6526-213">Herhangi bir sayıda iletisi adları dinleme.</span><span class="sxs-lookup"><span data-stu-id="d6526-213">You can listen to any number of message names.</span></span> <span data-ttu-id="d6526-214">Yazar adı ve alınan ileti içeriği gibi iletisi parametreleri geçirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d6526-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="d6526-215">İstemcisi bir iletiyi aldıktan sonra yeni bir `div` öğesi oluşturulduğunda yazar adı ve ileti içeriği kendi `innerHTML` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d6526-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="d6526-216">Ana eklenen `div` iletilerin görüntüleme öğesi.</span><span class="sxs-lookup"><span data-stu-id="d6526-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="d6526-217">İstemci bir ileti alabilir, iletileri göndermek için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d6526-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="d6526-218">Vurgulanmış kodu ekleyin *src/index.ts* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d6526-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="d6526-219">WebSockets bağlantısı üzerinden ileti gönderme gerektirir arama `send` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d6526-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="d6526-220">Yöntemin ilk parametresi ileti adıdır.</span><span class="sxs-lookup"><span data-stu-id="d6526-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="d6526-221">İleti verileri diğer parametreler inhabits.</span><span class="sxs-lookup"><span data-stu-id="d6526-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="d6526-222">Bu örnekte, bir ileti tanımlanması `newMessage` sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d6526-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="d6526-223">Kullanıcı adı ve kullanıcı bir metin kutusunda girişi, ileti oluşur.</span><span class="sxs-lookup"><span data-stu-id="d6526-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="d6526-224">Gönderme çalışırsa, metin kutusu değerini temizlenir.</span><span class="sxs-lookup"><span data-stu-id="d6526-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="d6526-225">Vurgulanan yöntemine ekleyin `ChatHub` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="d6526-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="d6526-226">Sunucu bunları aldıktan sonra tüm bağlı kullanıcılara alınan iletiler önceki kod yayınlar.</span><span class="sxs-lookup"><span data-stu-id="d6526-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="d6526-227">Genel sahip gereksizdir `on` tüm iletileri almak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="d6526-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="d6526-228">İleti adı son eklerini sonra adlı bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="d6526-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="d6526-229">Bu örnekte, TypeScript istemci olarak tanımlanan bir ileti gönderir `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="d6526-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="d6526-230">C# `NewMessage` yöntemi istemci tarafından gönderilen verileri bekliyor.</span><span class="sxs-lookup"><span data-stu-id="d6526-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="d6526-231">İçin bir çağrı yapılır [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="d6526-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="d6526-232">Alınan iletileri hub'ına bağlı tüm istemcilere gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d6526-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="d6526-233">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d6526-233">Test the app</span></span>

<span data-ttu-id="d6526-234">Uygulamanın aşağıdaki adımlarla çalıştığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="d6526-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6526-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6526-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d6526-236">Webpack Çalıştır *yayın* modu.</span><span class="sxs-lookup"><span data-stu-id="d6526-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="d6526-237">Kullanarak **Paket Yöneticisi Konsolu** penceresinde, proje kök dizininde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d6526-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d6526-238">Seçin **hata ayıklama** > **Başlat hata ayıklama olmadan** hata ayıklayıcı eklemeden bir tarayıcıda uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="d6526-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="d6526-239">*Wwwroot/index.html* adresindeki dosya sunulan `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="d6526-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="d6526-240">Başka bir tarayıcı örneği (herhangi bir tarayıcıda) açın.</span><span class="sxs-lookup"><span data-stu-id="d6526-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="d6526-241">URL'yi adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6526-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d6526-242">Ya da tarayıcı seçin, bir **ileti** metin kutusu ve tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d6526-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d6526-243">Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d6526-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d6526-244">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d6526-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="d6526-245">Webpack Çalıştır *yayın* proje kök dizininde aşağıdaki komutu çalıştırarak modu:</span><span class="sxs-lookup"><span data-stu-id="d6526-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d6526-246">Derleme ve proje kök dizininde aşağıdaki komutu çalıştırarak uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d6526-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="d6526-247">Web sunucusu uygulaması başlatır ve localhost üzerinde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="d6526-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="d6526-248">Bir tarayıcıda `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="d6526-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="d6526-249">*Wwwroot/index.html* dosya hizmet.</span><span class="sxs-lookup"><span data-stu-id="d6526-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="d6526-250">Adres çubuğundan URL'yi kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="d6526-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="d6526-251">Başka bir tarayıcı örneği (herhangi bir tarayıcıda) açın.</span><span class="sxs-lookup"><span data-stu-id="d6526-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="d6526-252">URL'yi adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="d6526-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d6526-253">Ya da tarayıcı seçin, bir **ileti** metin kutusu ve tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d6526-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d6526-254">Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d6526-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Her iki tarayıcı pencerelerini görüntülenen iletisi](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="d6526-256">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d6526-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>