---
title: ASP.NET Core ile React proje şablonu kullanın
author: SteveSandersonMS
description: React ve oluşturma-react-uygulama için ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react
ms.openlocfilehash: d83bff8abcd5b59d8bc4a51a101510755394f0c4
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667693"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="2d463-103">ASP.NET Core ile React proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="2d463-103">Use the React project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="2d463-104">Bu belge hakkında React proje şablonu, ASP.NET Core 2.0 sürümünde dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="2d463-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="2d463-105">Bu, el ile güncelleştirebilirsiniz yeni React hakkında şablonudur.</span><span class="sxs-lookup"><span data-stu-id="2d463-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="2d463-106">Şablon içinde ASP.NET Core 2.1 varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="2d463-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="2d463-107">Güncelleştirilmiş React proje şablonu uygun bir başlama noktası ASP.NET Core için React kullanan uygulamalar sağlar ve [oluşturma-react-app](https://github.com/facebookincubator/create-react-app) (CRA) kuralları, istemci tarafı zengin kullanıcı arabirimi (UI) uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="2d463-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="2d463-108">Şablon, hem bir API arka ucu görev yapacak bir ASP.NET Core projesi ve bir kullanıcı Arabirimi, ancak her ikisi de oluşturduğu ve tek bir birim olarak yayımlanan tek uygulama projesinde barındırma birlikte yapması standart bir CRA React proje oluşturmaya eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2d463-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="2d463-109">Yeni bir uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="2d463-109">Create a new app</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2d463-110">ASP.NET Core 2.0 kullanıyorsanız, seçtiğiniz olun [güncelleştirilmiş React proje şablonu yüklü](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="2d463-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2d463-111">ASP.NET Core 2.1 yüklü varsa, React proje şablonu yüklemek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="2d463-111">If you have ASP.NET Core 2.1 installed, there's no need to install the React project template.</span></span>

::: moniker-end

<span data-ttu-id="2d463-112">Yeni Proje Oluştur komutunu kullanarak bir komut isteminden `dotnet new react` boş bir dizin içinde.</span><span class="sxs-lookup"><span data-stu-id="2d463-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="2d463-113">Örneğin, aşağıdaki komutları uygulamada oluşturma bir *-yeni-Uygulamam* dizini ve bu dizine geçin:</span><span class="sxs-lookup"><span data-stu-id="2d463-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="2d463-114">Visual Studio veya .NET Core CLI uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2d463-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d463-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d463-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d463-116">Oluşturulan açın *.csproj* dosya ve uygulama normal olarak oradan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2d463-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="2d463-117">Derleme işlemi birkaç dakika sürebilir ilk çalıştırma npm bağımlılıkları yükler.</span><span class="sxs-lookup"><span data-stu-id="2d463-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="2d463-118">Ardışık derlemeler çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="2d463-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2d463-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2d463-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2d463-120">Adlı bir ortam değişkeni sahip olduğunuzdan emin olun `ASPNETCORE_Environment` değeriyle `Development`.</span><span class="sxs-lookup"><span data-stu-id="2d463-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="2d463-121">(PowerShell olmayan istemleri içinde) Windows üzerinde çalıştırma `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="2d463-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="2d463-122">Linux veya macOS üzerinde çalıştıran `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="2d463-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="2d463-123">Çalıştırma [dotnet derleme](/dotnet/core/tools/dotnet-build) uygulamanızı doğrulamak için yapılar doğru.</span><span class="sxs-lookup"><span data-stu-id="2d463-123">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="2d463-124">İlk çalıştırılmasında oluşturma işlemi birkaç dakika sürebilir npm bağımlılıkları, geri yükler.</span><span class="sxs-lookup"><span data-stu-id="2d463-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="2d463-125">Ardışık derlemeler çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="2d463-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="2d463-126">Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulamasını başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="2d463-126">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="2d463-127">Proje şablonu, ASP.NET Core uygulaması ve bir React uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2d463-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="2d463-128">ASP.NET Core uygulaması, veri erişimi, yetkilendirme ve diğer sunucu tarafı sorunları için kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2d463-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="2d463-129">Bulunan React uygulama *ClientApp* alt dizini kullanır, tüm kullanıcı Arabirimi sorunları için kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2d463-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="2d463-130">Sayfaları, görüntüler, stil, modüller, vb. ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2d463-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="2d463-131">*ClientApp* dizindir CRA React standart bir uygulaması.</span><span class="sxs-lookup"><span data-stu-id="2d463-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="2d463-132">Resmi görmek [CRA belgeleri](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2d463-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="2d463-133">Bu şablonla oluşturulan React uygulama ve CRA tarafından oluşturulan arasında küçük farklar vardır; Ancak, uygulama özelliklerini değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="2d463-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="2d463-134">Şablon tarafından oluşturulan uygulama içeren bir [önyükleme](https://getbootstrap.com/)-temel düzen ve basit bir yönlendirme örneği.</span><span class="sxs-lookup"><span data-stu-id="2d463-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="2d463-135">Npm paketlerini yükle</span><span class="sxs-lookup"><span data-stu-id="2d463-135">Install npm packages</span></span>

<span data-ttu-id="2d463-136">Üçüncü taraf npm paketlerini yüklemek için bir komut isteminde kullanmak *ClientApp* alt.</span><span class="sxs-lookup"><span data-stu-id="2d463-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="2d463-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2d463-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="2d463-138">Yayımlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="2d463-138">Publish and deploy</span></span>

<span data-ttu-id="2d463-139">Geliştirmede uygulama geliştiriciye kolaylık sağlamak için en iyi duruma getirilmiş bir modda çalışır.</span><span class="sxs-lookup"><span data-stu-id="2d463-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="2d463-140">Örneğin, (hata ayıklama sırasında özgün kaynak kodunuzu görebilmeniz için) kaynak eşlemeleri JavaScript paketleri içerir.</span><span class="sxs-lookup"><span data-stu-id="2d463-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="2d463-141">Uygulama JavaScript, HTML ve CSS diskte dosya değişiklikleri izler ve otomatik olarak yeniden derler ve bu dosyaları değiştirmek gördüğünde yeniden yükler.</span><span class="sxs-lookup"><span data-stu-id="2d463-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="2d463-142">Üretim ortamında, uygulamanızın performansını en iyi duruma getirilmiş bir sürümünü işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="2d463-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="2d463-143">Bu, otomatik olarak gerçekleştirilmesi için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="2d463-143">This is configured to happen automatically.</span></span> <span data-ttu-id="2d463-144">Yayımladığınızda, istemci tarafı kodunuzu küçültülmüş, transpiled yapısı derleme yapılandırmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="2d463-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="2d463-145">Geliştirme derleme, üretim yapı sunucusunda yüklü olması Node.js gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="2d463-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="2d463-146">Standart kullanabileceğiniz [ASP.NET Core barındırma ve dağıtma yöntemleri](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="2d463-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="2d463-147">CRA sunucunun bağımsız olarak çalıştırın</span><span class="sxs-lookup"><span data-stu-id="2d463-147">Run the CRA server independently</span></span>

<span data-ttu-id="2d463-148">Proje, ASP.NET Core uygulaması geliştirme modunda başlatıldığında, arka planda CRA geliştirme sunucusunu kendi örneğini başlatmak için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="2d463-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="2d463-149">Bu, ayrı bir sunucuya el ile çalıştırmak zorunda olmadığınız anlamına gelir çünkü uygundur.</span><span class="sxs-lookup"><span data-stu-id="2d463-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="2d463-150">Bu varsayılan ayarı bir dezavantajı vardır.</span><span class="sxs-lookup"><span data-stu-id="2d463-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="2d463-151">C# kodunuzu ve ASP.NET Core uygulamasını yeniden başlatmak için gereken her değiştirdiğinizde CRA sunucuyu yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="2d463-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="2d463-152">Birkaç saniyede yedekleme başlatmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2d463-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="2d463-153">Sık C# kod düzenleme yapıyorsanız ve yeniden başlatmak için CRA sunucusu beklemek istemiyorsanız, harici olarak CRA sunucunun ASP.NET Core işlemden bağımsız olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2d463-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="2d463-154">Bunu yapmak için:</span><span class="sxs-lookup"><span data-stu-id="2d463-154">To do so:</span></span>

1. <span data-ttu-id="2d463-155">Ekleme bir *.env* dosyasını *ClientApp* aşağıdaki ayar alt dizini:</span><span class="sxs-lookup"><span data-stu-id="2d463-155">Add a *.env* file to the *ClientApp* subdirectory with the following setting:</span></span>

    ```
    BROWSER=none
    ```
    
    <span data-ttu-id="2d463-156">Harici olarak CRA sunucunun başlatırken bu web tarayıcınız açılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="2d463-156">This will prevent your web browser from opening when starting the CRA server externally.</span></span>

2. <span data-ttu-id="2d463-157">Komut isteminde, geçiş *ClientApp* alt ve CRA geliştirme sunucusu başlatma:</span><span class="sxs-lookup"><span data-stu-id="2d463-157">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

3. <span data-ttu-id="2d463-158">ASP.NET Core uygulamanızı biri kendi başlatma yerine dış CRA server örneğini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2d463-158">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="2d463-159">İçinde *başlangıç* sınıfı, yerine `spa.UseReactDevelopmentServer` aşağıdaki çağrı:</span><span class="sxs-lookup"><span data-stu-id="2d463-159">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="2d463-160">ASP.NET Core uygulamanızı başlattığınızda CRA sunucusu başlatma olmaz.</span><span class="sxs-lookup"><span data-stu-id="2d463-160">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="2d463-161">El ile başlatılan örneği yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2d463-161">The instance you started manually is used instead.</span></span> <span data-ttu-id="2d463-162">Bu başlatma ve hızlı yeniden başlatma için sağlar.</span><span class="sxs-lookup"><span data-stu-id="2d463-162">This enables it to start and restart faster.</span></span> <span data-ttu-id="2d463-163">Artık her seferinde yeniden oluşturmasıyla React uygulamanız için bekliyor.</span><span class="sxs-lookup"><span data-stu-id="2d463-163">It's no longer waiting for your React app to rebuild each time.</span></span>
