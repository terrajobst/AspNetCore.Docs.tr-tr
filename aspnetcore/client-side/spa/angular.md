---
title: ASP.NET Core ile Angular proje şablonu kullanın
author: SteveSandersonMS
description: ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile Angular ve Angular CLI'yi kullanmaya nasıl başlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 8283fe9e96acb57942040dd4c90fabd204a19663
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326049"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a><span data-ttu-id="7d123-103">ASP.NET Core ile Angular proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="7d123-103">Use the Angular project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="7d123-104">Bu belge hakkında Angular proje şablonu, ASP.NET Core 2.0 sürümünde yer almıyor.</span><span class="sxs-lookup"><span data-stu-id="7d123-104">This documentation isn't about the Angular project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="7d123-105">Bu, el ile güncelleştirebilirsiniz yeni Angular hakkında şablonudur.</span><span class="sxs-lookup"><span data-stu-id="7d123-105">It's about the newer Angular template to which you can update manually.</span></span> <span data-ttu-id="7d123-106">Şablon içinde ASP.NET Core 2.1 varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="7d123-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="7d123-107">Güncelleştirilmiş Angular proje şablonu, Angular ve Angular CLI'yi zengin, istemci tarafı kullanıcı arabirimini (UI) kullanarak uygulamaları ASP.NET Core için uygun bir başlama noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="7d123-107">The updated Angular project template provides a convenient starting point for ASP.NET Core apps using Angular and the Angular CLI to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="7d123-108">Şablon, bir API arka ucu görev yapacak bir ASP.NET Core projesi ve bir kullanıcı Arabirimi görev yapacak bir Angular CLI'yi projesi oluşturmaya eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="7d123-108">The template is equivalent to creating an ASP.NET Core project to act as an API backend and an Angular CLI project to act as a UI.</span></span> <span data-ttu-id="7d123-109">Şablon iki proje türü tek bir uygulama projesinde barındırma kolaylık sunar.</span><span class="sxs-lookup"><span data-stu-id="7d123-109">The template offers the convenience of hosting both project types in a single app project.</span></span> <span data-ttu-id="7d123-110">Sonuç olarak, uygulama projesi oluşturulabilen ve tek bir birim olarak yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="7d123-110">Consequently, the app project can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="7d123-111">Yeni uygulama oluştur</span><span class="sxs-lookup"><span data-stu-id="7d123-111">Create a new app</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7d123-112">ASP.NET Core 2.0 kullanıyorsanız, seçtiğiniz olun [güncelleştirilmiş Angular proje şablonu yüklü](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="7d123-112">If using ASP.NET Core 2.0, ensure you've [installed the updated Angular project template](xref:spa/index#installation).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7d123-113">ASP.NET Core 2.1 yüklü varsa, Angular proje şablonu yüklemek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="7d123-113">If you have ASP.NET Core 2.1 installed, there's no need to install the Angular project template.</span></span>

::: moniker-end

<span data-ttu-id="7d123-114">Yeni Proje Oluştur komutunu kullanarak bir komut isteminden `dotnet new angular` boş bir dizin içinde.</span><span class="sxs-lookup"><span data-stu-id="7d123-114">Create a new project from a command prompt using the command `dotnet new angular` in an empty directory.</span></span> <span data-ttu-id="7d123-115">Örneğin, aşağıdaki komutları uygulamada oluşturma bir *-yeni-Uygulamam* dizini ve bu dizine geçin:</span><span class="sxs-lookup"><span data-stu-id="7d123-115">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new angular -o my-new-app
cd my-new-app
```

<span data-ttu-id="7d123-116">Visual Studio veya .NET Core CLI uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7d123-116">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7d123-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d123-117">Visual Studio</span></span>](#tab/visual-studio/)

<span data-ttu-id="7d123-118">Oluşturulan açın *.csproj* dosya ve uygulama normal olarak oradan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7d123-118">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="7d123-119">Derleme işlemi birkaç dakika sürebilir ilk çalıştırma npm bağımlılıkları yükler.</span><span class="sxs-lookup"><span data-stu-id="7d123-119">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="7d123-120">Ardışık derlemeler çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="7d123-120">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7d123-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d123-121">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="7d123-122">Adlı bir ortam değişkeni sahip olduğunuzdan emin olun `ASPNETCORE_Environment` değeriyle `Development`.</span><span class="sxs-lookup"><span data-stu-id="7d123-122">Ensure you have an environment variable called `ASPNETCORE_Environment` with a value of `Development`.</span></span> <span data-ttu-id="7d123-123">(PowerShell olmayan istemleri içinde) Windows üzerinde çalıştırma `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="7d123-123">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="7d123-124">Linux veya macOS üzerinde çalıştıran `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="7d123-124">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="7d123-125">Çalıştırma [dotnet derleme](/dotnet/core/tools/dotnet-build) uygulamayı doğrulamak için yapılar doğru.</span><span class="sxs-lookup"><span data-stu-id="7d123-125">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify the app builds correctly.</span></span> <span data-ttu-id="7d123-126">İlk çalıştırılmasında oluşturma işlemi birkaç dakika sürebilir npm bağımlılıkları, geri yükler.</span><span class="sxs-lookup"><span data-stu-id="7d123-126">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="7d123-127">Ardışık derlemeler çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="7d123-127">Subsequent builds are much faster.</span></span>

<span data-ttu-id="7d123-128">Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulamasını başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="7d123-128">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span> <span data-ttu-id="7d123-129">Aşağıdakine benzer bir ileti günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="7d123-129">A message similar to the following is logged:</span></span>

```console
Now listening on: http://localhost:<port>
```

<span data-ttu-id="7d123-130">Bir tarayıcıda bu URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="7d123-130">Navigate to this URL in a browser.</span></span>

<span data-ttu-id="7d123-131">Uygulama arka planda Angular CLI'yi sunucusu olan bir örneğini başlatır.</span><span class="sxs-lookup"><span data-stu-id="7d123-131">The app starts up an instance of the Angular CLI server in the background.</span></span> <span data-ttu-id="7d123-132">Aşağıdakine benzer bir ileti günlüğe kaydedilir: *Live geliştirme sunucu komut localhost üzerinde dinleme:&lt;otherport&gt;, tarayıcınızı açmak http://localhost:&lt; otherport&gt; /*  .</span><span class="sxs-lookup"><span data-stu-id="7d123-132">A message similar to the following is logged: *NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*.</span></span> <span data-ttu-id="7d123-133">Bu iletiyi yoksayın&mdash;sahip **değil** birleşik ASP.NET Core ve Angular CLI'yi app URL'si.</span><span class="sxs-lookup"><span data-stu-id="7d123-133">Ignore this message&mdash;it's **not** the URL for the combined ASP.NET Core and Angular CLI app.</span></span>

---

<span data-ttu-id="7d123-134">Proje şablonu, ASP.NET Core uygulaması ve Angular uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7d123-134">The project template creates an ASP.NET Core app and an Angular app.</span></span> <span data-ttu-id="7d123-135">ASP.NET Core uygulaması, veri erişimi, yetkilendirme ve diğer sunucu tarafı sorunları için kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7d123-135">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="7d123-136">Bulunan uygulamayı Angular *ClientApp* alt dizini kullanır, tüm kullanıcı Arabirimi sorunları için kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7d123-136">The Angular app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="7d123-137">Sayfaları, görüntüler, stil, modüller, vb. ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7d123-137">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="7d123-138">*ClientApp* dizini bir standart Angular CLI'yi uygulaması içerir.</span><span class="sxs-lookup"><span data-stu-id="7d123-138">The *ClientApp* directory contains a standard Angular CLI app.</span></span> <span data-ttu-id="7d123-139">Resmi görmek [Angular belgeleri](https://github.com/angular/angular-cli/wiki) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7d123-139">See the official [Angular documentation](https://github.com/angular/angular-cli/wiki) for more information.</span></span>

<span data-ttu-id="7d123-140">Bu şablonla oluşturulan Angular uygulama ve Angular CLI tarafından kendi oluşturduğunuz bir arasında küçük farklar vardır (aracılığıyla `ng new`); ancak, uygulama özelliklerini değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="7d123-140">There are slight differences between the Angular app created by this template and the one created by Angular CLI itself (via `ng new`); however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="7d123-141">Şablon tarafından oluşturulan uygulama içeren bir [önyükleme](https://getbootstrap.com/)-temel düzen ve basit bir yönlendirme örneği.</span><span class="sxs-lookup"><span data-stu-id="7d123-141">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="run-ng-commands"></a><span data-ttu-id="7d123-142">NG komutları çalıştırın</span><span class="sxs-lookup"><span data-stu-id="7d123-142">Run ng commands</span></span>

<span data-ttu-id="7d123-143">Komut isteminde, geçiş *ClientApp* alt dizini:</span><span class="sxs-lookup"><span data-stu-id="7d123-143">In a command prompt, switch to the *ClientApp* subdirectory:</span></span>

```console
cd ClientApp
```

<span data-ttu-id="7d123-144">Varsa `ng` genel yüklü aracı çalıştırabilirsiniz, komutlardan herhangi birine.</span><span class="sxs-lookup"><span data-stu-id="7d123-144">If you have the `ng` tool installed globally, you can run any of its commands.</span></span> <span data-ttu-id="7d123-145">Örneğin, çalıştırabileceğiniz `ng lint`, `ng test`, veya herhangi başka bir [Angular CLI komutları](https://github.com/angular/angular-cli/wiki#additional-commands).</span><span class="sxs-lookup"><span data-stu-id="7d123-145">For example, you can run `ng lint`, `ng test`, or any of the other [Angular CLI commands](https://github.com/angular/angular-cli/wiki#additional-commands).</span></span> <span data-ttu-id="7d123-146">Çalıştırılacak gerek yoktur `ng serve` , uygulamanızın sunucu tarafı hem istemci tarafı bölümlerini hizmet ile ASP.NET Core uygulamanızı oluşturulmasıyla ilgili olduğu için.</span><span class="sxs-lookup"><span data-stu-id="7d123-146">There's no need to run `ng serve` though, because your ASP.NET Core app deals with serving both server-side and client-side parts of your app.</span></span> <span data-ttu-id="7d123-147">Dahili olarak kullandığı `ng serve` geliştirme.</span><span class="sxs-lookup"><span data-stu-id="7d123-147">Internally, it uses `ng serve` in development.</span></span>

<span data-ttu-id="7d123-148">Öğeniz yoksa `ng` yüklü aracı çalıştırma `npm run ng` bunun yerine.</span><span class="sxs-lookup"><span data-stu-id="7d123-148">If you don't have the `ng` tool installed, run `npm run ng` instead.</span></span> <span data-ttu-id="7d123-149">Örneğin, çalıştırabileceğiniz `npm run ng lint` veya `npm run ng test`.</span><span class="sxs-lookup"><span data-stu-id="7d123-149">For example, you can run `npm run ng lint` or `npm run ng test`.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="7d123-150">Npm paketlerini yükle</span><span class="sxs-lookup"><span data-stu-id="7d123-150">Install npm packages</span></span>

<span data-ttu-id="7d123-151">Üçüncü taraf npm paketlerini yüklemek için bir komut isteminde kullanmak *ClientApp* alt.</span><span class="sxs-lookup"><span data-stu-id="7d123-151">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="7d123-152">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7d123-152">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="7d123-153">Yayımlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="7d123-153">Publish and deploy</span></span>

<span data-ttu-id="7d123-154">Geliştirmede uygulama geliştiriciye kolaylık sağlamak için en iyi duruma getirilmiş bir modda çalışır.</span><span class="sxs-lookup"><span data-stu-id="7d123-154">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="7d123-155">Örneğin, (hata ayıklama sırasında özgün TypeScript kodunuzu görebilmeniz için) kaynak eşlemeleri JavaScript paketleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7d123-155">For example, JavaScript bundles include source maps (so that when debugging, you can see your original TypeScript code).</span></span> <span data-ttu-id="7d123-156">Uygulama diskteki TypeScript, HTML ve CSS dosyası değişiklikler için izleyen ve otomatik olarak yeniden derler ve bu dosyaları değiştirmek gördüğünde yeniden yükler.</span><span class="sxs-lookup"><span data-stu-id="7d123-156">The app watches for TypeScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="7d123-157">Üretim ortamında, uygulamanızın performansını en iyi duruma getirilmiş bir sürümünü işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="7d123-157">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="7d123-158">Bu, otomatik olarak gerçekleştirilmesi için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="7d123-158">This is configured to happen automatically.</span></span> <span data-ttu-id="7d123-159">Yayımladığınızda, derleme yapılandırmasını bir küçültülmüş yayar, istemci tarafı kodunuzun derleme tamamlanan-ın-time (AoT) derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="7d123-159">When you publish, the build configuration emits a minified, ahead-of-time (AoT) compiled build of your client-side code.</span></span> <span data-ttu-id="7d123-160">Geliştirme derleme, üretim yapı sunucusunda yüklü olması Node.js gerektirmeyen (etkinleştirilmiş olduğu sürece [sunucu tarafı prerendering](#server-side-rendering)).</span><span class="sxs-lookup"><span data-stu-id="7d123-160">Unlike the development build, the production build doesn't require Node.js to be installed on the server (unless you have enabled [server-side prerendering](#server-side-rendering)).</span></span>

<span data-ttu-id="7d123-161">Standart kullanabileceğiniz [ASP.NET Core barındırma ve dağıtma yöntemleri](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="7d123-161">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-ng-serve-independently"></a><span data-ttu-id="7d123-162">"Ng hizmet vermemesini" bağımsız olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7d123-162">Run "ng serve" independently</span></span>

<span data-ttu-id="7d123-163">Proje, ASP.NET Core uygulaması geliştirme modunda başlatıldığında, arka planda Angular CLI'yi sunucunun kendi örneğini başlatmak için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="7d123-163">The project is configured to start its own instance of the Angular CLI server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="7d123-164">Bu, ayrı bir sunucuya el ile çalıştırmak zorunda olmadığınız için uygundur.</span><span class="sxs-lookup"><span data-stu-id="7d123-164">This is convenient because you don't have to run a separate server manually.</span></span>

<span data-ttu-id="7d123-165">Bu varsayılan ayarı bir dezavantajı vardır.</span><span class="sxs-lookup"><span data-stu-id="7d123-165">There's a drawback to this default setup.</span></span> <span data-ttu-id="7d123-166">C# kodunuzu ve ASP.NET Core uygulamasını yeniden başlatmak için gereken her değiştirdiğinizde, Angular CLI'yi sunucuyu yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="7d123-166">Each time you modify your C# code and your ASP.NET Core app needs to restart, the Angular CLI server restarts.</span></span> <span data-ttu-id="7d123-167">Yaklaşık 10 saniye yedekleme başlatmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7d123-167">Around 10 seconds is required to start back up.</span></span> <span data-ttu-id="7d123-168">Sık kullanılan C# kod düzenleme yapıyorsanız ve yeniden başlatmak Angular CLI için beklemek istemiyorsanız, Angular CLI'yi sunucu dışarıdan, ASP.NET Core işlemden bağımsız olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7d123-168">If you're making frequent C# code edits and don't want to wait for Angular CLI to restart, run the Angular CLI server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="7d123-169">Bunu yapmak için:</span><span class="sxs-lookup"><span data-stu-id="7d123-169">To do so:</span></span>

1. <span data-ttu-id="7d123-170">Komut isteminde, geçiş *ClientApp* alt ve Angular CLI'yi geliştirme sunucusu başlatma:</span><span class="sxs-lookup"><span data-stu-id="7d123-170">In a command prompt, switch to the *ClientApp* subdirectory, and launch the Angular CLI development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="7d123-171">Kullanım `npm start` Angular CLI'yi geliştirme sunucusu başlatılamıyor değil `ng serve`, böylece yapılandırmada *package.json* uyulduğundan.</span><span class="sxs-lookup"><span data-stu-id="7d123-171">Use `npm start` to launch the Angular CLI development server, not `ng serve`, so that the configuration in *package.json* is respected.</span></span> <span data-ttu-id="7d123-172">Ek parametreleri Angular CLI'yi sunucuya geçirmek için bunları ilgili ekleme `scripts` satırına, *package.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7d123-172">To pass additional parameters to the Angular CLI server, add them to the relevant `scripts` line in your *package.json* file.</span></span>

2. <span data-ttu-id="7d123-173">ASP.NET Core uygulamanızı biri kendi başlatma yerine dış Angular CLI örneği kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7d123-173">Modify your ASP.NET Core app to use the external Angular CLI instance instead of launching one of its own.</span></span> <span data-ttu-id="7d123-174">İçinde *başlangıç* sınıfı, yerine `spa.UseAngularCliServer` aşağıdaki çağrı:</span><span class="sxs-lookup"><span data-stu-id="7d123-174">In your *Startup* class, replace the `spa.UseAngularCliServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

<span data-ttu-id="7d123-175">ASP.NET Core uygulamanızı başlattığınızda, Angular CLI'yi sunucusu başlatma olmaz.</span><span class="sxs-lookup"><span data-stu-id="7d123-175">When you start your ASP.NET Core app, it won't launch an Angular CLI server.</span></span> <span data-ttu-id="7d123-176">El ile başlatılan örneği yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7d123-176">The instance you started manually is used instead.</span></span> <span data-ttu-id="7d123-177">Bu başlatma ve hızlı yeniden başlatma için sağlar.</span><span class="sxs-lookup"><span data-stu-id="7d123-177">This enables it to start and restart faster.</span></span> <span data-ttu-id="7d123-178">İstemci uygulamanızı her seferinde yeniden oluşturmasıyla Angular CLI için artık bekliyor.</span><span class="sxs-lookup"><span data-stu-id="7d123-178">It's no longer waiting for Angular CLI to rebuild your client app each time.</span></span>

## <a name="server-side-rendering"></a><span data-ttu-id="7d123-179">Sunucu tarafı işleme</span><span class="sxs-lookup"><span data-stu-id="7d123-179">Server-side rendering</span></span>

<span data-ttu-id="7d123-180">Bir performans özelliği, sunucu hem de istemcide çalışan Angular uygulamanızı önceden işlenecek seçim yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d123-180">As a performance feature, you can choose to pre-render your Angular app on the server as well as running it on the client.</span></span> <span data-ttu-id="7d123-181">Bu tarayıcılar, hatta indiriliyor ve, JavaScript paketleri yürütmeden önce görüntülemek için uygulamanızın ilk kullanıcı Arabirimi temsil eden HTML biçimlendirmesi aldığınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="7d123-181">This means that browsers receive HTML markup representing your app's initial UI, so they display it even before downloading and executing your JavaScript bundles.</span></span> <span data-ttu-id="7d123-182">Çoğu uygulama bu adlı bir Angular özellikten gelen [Angular Evrensel](https://universal.angular.io/).</span><span class="sxs-lookup"><span data-stu-id="7d123-182">Most of the implementation of this comes from an Angular feature called [Angular Universal](https://universal.angular.io/).</span></span>

> [!TIP]
> <span data-ttu-id="7d123-183">Sunucu tarafı işleme etkinleştirme (SSR) birkaç fazladan zorluk hem geliştirme ve dağıtım sırasında ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="7d123-183">Enabling server-side rendering (SSR) introduces a number of extra complications both during development and deployment.</span></span> <span data-ttu-id="7d123-184">Okuma [SSR dezavantajları](#drawbacks-of-ssr) SSR gereksinimleriniz için uygun olup olmadığını belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="7d123-184">Read [drawbacks of SSR](#drawbacks-of-ssr) to determine if SSR is a good fit for your requirements.</span></span>

<span data-ttu-id="7d123-185">SSR etkinleştirmek için projenizi eklemeleri bir dizi yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7d123-185">To enable SSR, you need to make a number of additions to your project.</span></span>

<span data-ttu-id="7d123-186">İçinde *başlangıç* sınıfı *sonra* yapılandırır satırı `spa.Options.SourcePath`, ve *önce* çağrısı `UseAngularCliServer` veya `UseProxyToSpaDevelopmentServer`, aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7d123-186">In the *Startup* class, *after* the line that configures `spa.Options.SourcePath`, and *before* the call to `UseAngularCliServer` or `UseProxyToSpaDevelopmentServer`, add the following:</span></span>

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

<span data-ttu-id="7d123-187">Geliştirme modunda, betik çalıştırarak SSR paket oluşturmak Bu kod çalışır `build:ssr`, tanımlanan *ClientApp\package.json*.</span><span class="sxs-lookup"><span data-stu-id="7d123-187">In development mode, this code attempts to build the SSR bundle by running the script `build:ssr`, which is defined in *ClientApp\package.json*.</span></span> <span data-ttu-id="7d123-188">Bu adlı bir Angular uygulama derlemeleri `ssr`, henüz tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="7d123-188">This builds an Angular app named `ssr`, which isn't yet defined.</span></span>

<span data-ttu-id="7d123-189">Sonunda `apps` içindeki dizi *ClientApp/.angular-cli.json*, ek bir uygulama adıyla tanımlama `ssr`.</span><span class="sxs-lookup"><span data-stu-id="7d123-189">At the end of the `apps` array in *ClientApp/.angular-cli.json*, define an extra app with name `ssr`.</span></span> <span data-ttu-id="7d123-190">Aşağıdaki seçenekleri kullanın:</span><span class="sxs-lookup"><span data-stu-id="7d123-190">Use the following options:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

<span data-ttu-id="7d123-191">Bu yeni SSR özellikli uygulama yapılandırmasını iki ek dosyalar gerektirir: *tsconfig.server.json* ve *main.server.ts*.</span><span class="sxs-lookup"><span data-stu-id="7d123-191">This new SSR-enabled app configuration requires two further files: *tsconfig.server.json* and *main.server.ts*.</span></span> <span data-ttu-id="7d123-192">*Tsconfig.server.json* dosyasını TypeScript derleme seçenekleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="7d123-192">The *tsconfig.server.json* file specifies TypeScript compilation options.</span></span> <span data-ttu-id="7d123-193">*Main.server.ts* dosya SSR sırasında kod giriş noktası olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="7d123-193">The *main.server.ts* file serves as the code entry point during SSR.</span></span>

<span data-ttu-id="7d123-194">Adlı yeni bir dosya ekleme *tsconfig.server.json* içinde *ClientApp/src* (varolan yanı sıra *tsconfig.app.json*), aşağıdakileri içeren:</span><span class="sxs-lookup"><span data-stu-id="7d123-194">Add a new file called *tsconfig.server.json* inside *ClientApp/src* (alongside the existing *tsconfig.app.json*), containing the following:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

<span data-ttu-id="7d123-195">Bu dosya adlı bir modül için aramak için Angular AoT derleyici yapılandırır `app.server.module`.</span><span class="sxs-lookup"><span data-stu-id="7d123-195">This file configures Angular's AoT compiler to look for a module called `app.server.module`.</span></span> <span data-ttu-id="7d123-196">Bu, yeni bir dosya oluşturarak ekleyin *ClientApp/src/app/app.server.module.ts* (varolan yanı sıra *app.module.ts*) aşağıdakileri içeren:</span><span class="sxs-lookup"><span data-stu-id="7d123-196">Add this by creating a new file at *ClientApp/src/app/app.server.module.ts* (alongside the existing *app.module.ts*) containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

<span data-ttu-id="7d123-197">Bu modül istemci-tarafı devralan `app.module` ve fazladan Angular modüllerine SSR sırasında kullanılabilir tanımlar.</span><span class="sxs-lookup"><span data-stu-id="7d123-197">This module inherits from your client-side `app.module` and defines which extra Angular modules are available during SSR.</span></span>

<span data-ttu-id="7d123-198">Bu geri çağırma yeni `ssr` girişi *.angular cli.json* adlı bir giriş noktası dosyası başvurulan *main.server.ts*.</span><span class="sxs-lookup"><span data-stu-id="7d123-198">Recall that the new `ssr` entry in *.angular-cli.json* referenced an entry point file called *main.server.ts*.</span></span> <span data-ttu-id="7d123-199">Bu dosya henüz eklemediniz ve artık bunu yapmak için zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="7d123-199">You haven't yet added that file, and now is time to do so.</span></span> <span data-ttu-id="7d123-200">En yeni bir dosya oluşturun *ClientApp/src/main.server.ts* (varolan yanı sıra *main.ts*), aşağıdakileri içeren:</span><span class="sxs-lookup"><span data-stu-id="7d123-200">Create a new file at *ClientApp/src/main.server.ts* (alongside the existing *main.ts*), containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

<span data-ttu-id="7d123-201">Bu dosyanın kodu çalıştığında ne ASP.NET Core için her bir isteği yürütür `UseSpaPrerendering` eklediğiniz bir ara yazılım *başlangıç* sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7d123-201">This file's code is what ASP.NET Core executes for each request when it runs the `UseSpaPrerendering` middleware that you added to the *Startup* class.</span></span> <span data-ttu-id="7d123-202">Alma ile ilgilenen `params` (örneğin, istenen URL) .NET kodu ve sonuçta elde edilen HTML almak için Angular SSR API çağrıları yapma.</span><span class="sxs-lookup"><span data-stu-id="7d123-202">It deals with receiving `params` from the .NET code (such as the URL being requested), and making calls to Angular SSR APIs to get the resulting HTML.</span></span>

<span data-ttu-id="7d123-203">Kesinlikle konuşulur, bu SSR geliştirme modunda etkinleştirmek yeterli olur.</span><span class="sxs-lookup"><span data-stu-id="7d123-203">Strictly-speaking, this is sufficient to enable SSR in development mode.</span></span> <span data-ttu-id="7d123-204">Böylece uygulamanızın düzgün çalıştığını yayımlandığında bir son değişiklik yapmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7d123-204">It's essential to make one final change so that your app works correctly when published.</span></span> <span data-ttu-id="7d123-205">Uygulamanızın ana içinde *.csproj* dosya, ayarlama `BuildServerSideRenderer` özellik değerini `true`:</span><span class="sxs-lookup"><span data-stu-id="7d123-205">In your app's main *.csproj* file, set the `BuildServerSideRenderer` property value to `true`:</span></span>

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

<span data-ttu-id="7d123-206">Bu yapı işlemini çalıştırın yapılandırır `build:ssr` yayımlama sırasında ve SSR dosya sunucusuna dağıtın.</span><span class="sxs-lookup"><span data-stu-id="7d123-206">This configures the build process to run `build:ssr` during publishing and deploy the SSR files to the server.</span></span> <span data-ttu-id="7d123-207">Bu etkinleştirme SSR üretimde başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="7d123-207">If you don't enable this, SSR fails in production.</span></span>

<span data-ttu-id="7d123-208">Angular kod HTML olarak sunucuda uygulamanızı geliştirme veya üretim modunda çalışırken, önceden işler.</span><span class="sxs-lookup"><span data-stu-id="7d123-208">When your app runs in either development or production mode, the Angular code pre-renders as HTML on the server.</span></span> <span data-ttu-id="7d123-209">İstemci tarafı kod, normal olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="7d123-209">The client-side code executes as normal.</span></span>

### <a name="pass-data-from-net-code-into-typescript-code"></a><span data-ttu-id="7d123-210">TypeScript koduna .NET kodundan verisini geçirin</span><span class="sxs-lookup"><span data-stu-id="7d123-210">Pass data from .NET code into TypeScript code</span></span>

<span data-ttu-id="7d123-211">SSR sırasında istek başına veri Angular uygulamanıza ASP.NET Core uygulamanızı geçirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d123-211">During SSR, you might want to pass per-request data from your ASP.NET Core app into your Angular app.</span></span> <span data-ttu-id="7d123-212">Örneğin, tanımlama bilgilerini geçirebiliriz veya bir şey bir veritabanından okuyamadı.</span><span class="sxs-lookup"><span data-stu-id="7d123-212">For example, you could pass cookie information or something read from a database.</span></span> <span data-ttu-id="7d123-213">Bunu yapmak için Düzenle, *başlangıç* sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7d123-213">To do this, edit your *Startup* class.</span></span> <span data-ttu-id="7d123-214">İçin geri çağırma içinde `UseSpaPrerendering`, ayarlamak için bir değer `options.SupplyData` aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="7d123-214">In the callback for `UseSpaPrerendering`, set a value for `options.SupplyData` such as the following:</span></span>

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

<span data-ttu-id="7d123-215">`SupplyData` Rastgele geçirdiğiniz geri çağırma sağlar, istek başına, JSON seri hale getirilebilir veri (için örnek, dizeler, Boole değerlerini veya sayı).</span><span class="sxs-lookup"><span data-stu-id="7d123-215">The `SupplyData` callback lets you pass arbitrary, per-request, JSON-serializable data (for example, strings, booleans, or numbers).</span></span> <span data-ttu-id="7d123-216">*Main.server.ts* kod olarak aldığı `params.data`.</span><span class="sxs-lookup"><span data-stu-id="7d123-216">Your *main.server.ts* code receives this as `params.data`.</span></span> <span data-ttu-id="7d123-217">Örneğin, yukarıdaki kod örneğinde bir Boole değeri olarak geçirir `params.data.isHttpsRequest` içine `createServerRenderer` geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="7d123-217">For example, the preceding code sample passes a boolean value as `params.data.isHttpsRequest` into the `createServerRenderer` callback.</span></span> <span data-ttu-id="7d123-218">Angular tarafından desteklenen herhangi bir şekilde uygulamanızın diğer kısımlarına geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d123-218">You can pass this to other parts of your app in any way supported by Angular.</span></span> <span data-ttu-id="7d123-219">Örneğin, nasıl *main.server.ts* geçirir `BASE_URL` alması, Oluşturucusu bildirilen herhangi bir bileşen değeri.</span><span class="sxs-lookup"><span data-stu-id="7d123-219">For example, see how *main.server.ts* passes the `BASE_URL` value to any component whose constructor is declared to receive it.</span></span>

### <a name="drawbacks-of-ssr"></a><span data-ttu-id="7d123-220">SSR dezavantajları</span><span class="sxs-lookup"><span data-stu-id="7d123-220">Drawbacks of SSR</span></span>

<span data-ttu-id="7d123-221">Tüm uygulamalar, SSR ' yararlanabilir.</span><span class="sxs-lookup"><span data-stu-id="7d123-221">Not all apps benefit from SSR.</span></span> <span data-ttu-id="7d123-222">Birincil avantajı algılanan performansı.</span><span class="sxs-lookup"><span data-stu-id="7d123-222">The primary benefit is perceived performance.</span></span> <span data-ttu-id="7d123-223">JavaScript paketleri ayrıştıramadık ya da fetch biraz sürdüğünü olsa bile uygulamanız yavaş ağ bağlantısı üzerinden veya yavaş mobil cihazlarına ulaşma ziyaretçiler ilk kullanıcı Arabirimi hızlı bir şekilde bakın.</span><span class="sxs-lookup"><span data-stu-id="7d123-223">Visitors reaching your app over a slow network connection or on slow mobile devices see the initial UI quickly, even if it takes a while to fetch or parse the JavaScript bundles.</span></span> <span data-ttu-id="7d123-224">Ancak, birçok Spa'lar çoğunlukla hızlı bilgisayarlarda hızlı, iç şirket ağları üzerinden uygulama neredeyse anında göründüğü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7d123-224">However, many SPAs are mainly used over fast, internal company networks on fast computers where the app appears almost instantly.</span></span>

<span data-ttu-id="7d123-225">Aynı zamanda, SSR etkinleştirmek için önemli engelleri vardır.</span><span class="sxs-lookup"><span data-stu-id="7d123-225">At the same time, there are significant drawbacks to enabling SSR.</span></span> <span data-ttu-id="7d123-226">Geliştirme sürecinizin karmaşıklık ekler.</span><span class="sxs-lookup"><span data-stu-id="7d123-226">It adds complexity to your development process.</span></span> <span data-ttu-id="7d123-227">İki farklı ortamlarda kodunuzun çalıştırmanız gerekir: istemci tarafı ve sunucu tarafı (ortamında ASP.NET çekirdek çağrılan bir Node.js).</span><span class="sxs-lookup"><span data-stu-id="7d123-227">Your code must run in two different environments: client-side and server-side (in a Node.js environment invoked from ASP.NET Core).</span></span> <span data-ttu-id="7d123-228">Aklınızda bulundurun gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="7d123-228">Here are some things to bear in mind:</span></span>

* <span data-ttu-id="7d123-229">SSR, üretim sunucularında bir Node.js yükleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7d123-229">SSR requires a Node.js installation on your production servers.</span></span> <span data-ttu-id="7d123-230">Otomatik olarak çalışması için Azure Service Fabric gibi diğer bazı dağıtım senaryolarında, Azure App Services gibi ancak budur.</span><span class="sxs-lookup"><span data-stu-id="7d123-230">This is automatically the case for some deployment scenarios, such as Azure App Services, but not for others, such as Azure Service Fabric.</span></span>
* <span data-ttu-id="7d123-231">Etkinleştirme `BuildServerSideRenderer` bayrağı nedenleri derleme, *node_modules* yayımlamak için dizin.</span><span class="sxs-lookup"><span data-stu-id="7d123-231">Enabling the `BuildServerSideRenderer` build flag causes your *node_modules* directory to publish.</span></span> <span data-ttu-id="7d123-232">Bu klasör, dağıtım süresini artırır 20.000 + dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="7d123-232">This folder contains 20,000+ files, which increases deployment time.</span></span>
* <span data-ttu-id="7d123-233">Bir Node.js ortamda kodunuzu çalıştırmak için tarayıcı özel JavaScript API varlığı üzerinde gibi güvenemezsiniz `window` veya `localStorage`.</span><span class="sxs-lookup"><span data-stu-id="7d123-233">To run your code in a Node.js environment, it can't rely on the existence of browser-specific JavaScript APIs such as `window` or `localStorage`.</span></span> <span data-ttu-id="7d123-234">Bu API'leri kullanmak kodunuzu (veya başvuru bazı üçüncü taraf kitaplık) çalışırsa, SSR sırasında bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="7d123-234">If your code (or some third-party library you reference) tries to use these APIs, you'll get an error during SSR.</span></span> <span data-ttu-id="7d123-235">Örneğin, tarayıcı özel API'leri çeşitli yerlerde başvurduğu için jQuery kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="7d123-235">For example, don't use jQuery because it references browser-specific APIs in many places.</span></span> <span data-ttu-id="7d123-236">Hataları önlemek için SSR önlemek veya tarayıcı özel API'leri veya kitaplıklarını kaçınmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="7d123-236">To prevent errors, you must either avoid SSR or avoid browser-specific APIs or libraries.</span></span> <span data-ttu-id="7d123-237">Bu API çağrıları SSR sırasında çağrılan olmayan çoğaltılmadığını denetler kaydırın.</span><span class="sxs-lookup"><span data-stu-id="7d123-237">You can wrap any calls to such APIs in checks to ensure they aren't invoked during SSR.</span></span> <span data-ttu-id="7d123-238">Örneğin, aşağıdaki gibi bir denetimi, JavaScript veya TypeScript kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="7d123-238">For example, use a check such as the following in JavaScript or TypeScript code:</span></span>

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
