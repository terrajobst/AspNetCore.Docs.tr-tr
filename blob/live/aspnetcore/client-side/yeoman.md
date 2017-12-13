---
title: "ASP.NET Core Yeoman ile proje oluşturma"
author: spboyer
description: "Bu makalede bir ASP.NET Core Yeoman kullanarak web uygulaması oluşturma sürecinde anlatılmaktadır macOS üzerinde Oluşturucu."
keywords: "ASP.NET Core, Yeoman, Çapraz Platform yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="03705-104">ASP.NET Core Yeoman ile projeler derleme giriş</span><span class="sxs-lookup"><span data-stu-id="03705-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="03705-105">[Yeoman](http://yeoman.io/) birçok türdeki uygulamalar oluşturmak için bir proje yapı iskelesi sistemidir.</span><span class="sxs-lookup"><span data-stu-id="03705-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="03705-106">Yeoman için ASP.NET Core oluşturucuyu yeni web, MVC veya konsol uygulaması başlatmak için proje şablonları çeşitli içerir.</span><span class="sxs-lookup"><span data-stu-id="03705-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="03705-107">Node.js, npm ve Yeoman yükleyin</span><span class="sxs-lookup"><span data-stu-id="03705-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="03705-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="03705-108">Prerequisites</span></span>

<span data-ttu-id="03705-109">Node.js ve npm Yeoman için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="03705-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="03705-110">İndirin [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="03705-110">Download from [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="03705-111">Yükleyici içerir [Node.js](https://nodejs.org/) ve [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="03705-111">The installer includes [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="03705-112">Bower önyükleme gibi UI çerçeveleri yüklemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="03705-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="03705-113">Yeoman ve Bower yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="03705-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="03705-114">Hata alırsanız `npm ERR! Please try running this command again as root/Administrator.` macOS üzerinde aşağıdaki komutu kullanarak çalıştırmak [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="03705-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="03705-115">Bir komut isteminden ASP.NET Oluşturucu yükleyin:</span><span class="sxs-lookup"><span data-stu-id="03705-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="03705-116">İzin hatası alırsanız, altında komutu çalıştırın `sudo` yukarıda açıklandığı gibi.</span><span class="sxs-lookup"><span data-stu-id="03705-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="03705-117">`–g` Böylece herhangi bir yol kullanılabilir bayrak oluşturucunun genel olarak yükler.</span><span class="sxs-lookup"><span data-stu-id="03705-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="03705-118">Bir ASP.NET uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="03705-118">Create an ASP.NET app</span></span>

<span data-ttu-id="03705-119">Yeoman tabanlı ASP.NET Oluşturucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="03705-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="03705-120">Oluşturucu bir menüsünü görüntüler.</span><span class="sxs-lookup"><span data-stu-id="03705-120">The generator displays a menu.</span></span> <span data-ttu-id="03705-121">Aşağı ok **Web uygulaması temel [olmadan, üyelik ve yetkilendirme]** proje ve dokunun **Enter**:</span><span class="sxs-lookup"><span data-stu-id="03705-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Komut penceresi: ne tür bir uygulama oluşturmak istiyor musunuz?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="03705-124">Önyükleme UI çerçevesi seçin ve dokunun **Enter**.</span><span class="sxs-lookup"><span data-stu-id="03705-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="03705-125">Kullan "**mywebapp şeklindedir**" uygulama adı ve ardından dokunun **Enter**.</span><span class="sxs-lookup"><span data-stu-id="03705-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="03705-126">Yeoman proje ve Tamamlayıcı dosyaları iskelesi.</span><span class="sxs-lookup"><span data-stu-id="03705-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="03705-127">Önerilen sonraki adımlar da komutları biçiminde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="03705-127">Suggested next steps are also provided in the form of commands.</span></span>

![Komut penceresi: ASP.NET uygulamanızın adı nedir?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="03705-130">[ASP.NET Oluşturucu](https://www.npmjs.com/package/generator-aspnet) Visual Studio, Visual Studio Code yüklenmesi veya komut satırından çalıştırma projeler ASP.NET Core oluşturur.</span><span class="sxs-lookup"><span data-stu-id="03705-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="03705-131">Geri yükleme, derleme ve çalıştırma</span><span class="sxs-lookup"><span data-stu-id="03705-131">Restore, build, and run</span></span>

<span data-ttu-id="03705-132">Önerilen komutları dizinleri değiştirerek izleyin `MyWebApp` dizin.</span><span class="sxs-lookup"><span data-stu-id="03705-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="03705-133">Ardından çalıştırın `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="03705-133">Then run `dotnet restore`.</span></span>

![Komut penceresi](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="03705-135">Derleme ve Çalıştırma uygulamasını kullanarak `dotnet build` ve `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="03705-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![Komut penceresi](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="03705-137">Bu noktada, yeni oluşturulan ASP.NET Core uygulama test etmek için gösterilen URL'yi gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="03705-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="03705-138">İstemci tarafı paketleri</span><span class="sxs-lookup"><span data-stu-id="03705-138">Client-side packages</span></span>

<span data-ttu-id="03705-139">Ön uç kaynakları Yeoman şablonlar tarafından sağlanan oluşturucusunu kullanarak [Bower](xref:client-side/bower) ekleme, istemci-tarafı paket Yöneticisi *bower.json* ve *.bowerrc* Bower kullanarak istemci tarafı paketleri geri yüklenecek dosyaları.</span><span class="sxs-lookup"><span data-stu-id="03705-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="03705-140">[BundlerMinifier](xref:client-side/bundling-and-minification) bileşeni (paketleme) birleştirme kolaylığı ve CSS, JavaScript ve HTML küçültme için varsayılan olarak da bulunur.</span><span class="sxs-lookup"><span data-stu-id="03705-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="03705-141">Oluşturma ve Visual Studio'dan çalıştırma</span><span class="sxs-lookup"><span data-stu-id="03705-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="03705-142">Oluşturulan ASP.NET Core web projeniz Visual Studio'ya doğrudan yük sonra oluşturun ve projenizin oradan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="03705-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="03705-143">Yeoman kullanarak yeni bir ASP.NET Core uygulaması iskele için yukarıdaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="03705-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="03705-144">Bu saati seçin **Web uygulaması** ad uygulaması ve menüden `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="03705-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="03705-145">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="03705-145">Open Visual Studio.</span></span> <span data-ttu-id="03705-146">Dosya menüsünde açık ‣ proje/çözüm seçin.</span><span class="sxs-lookup"><span data-stu-id="03705-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="03705-147">Proje Aç iletişim kutusunda gidin *.csproj* dosyasını seçin ve tıklatın **açık** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="03705-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="03705-148">Çözüm Gezgini'nde proje aşağıdaki ekran görüntüsüne benzer görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="03705-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![Dosya ve klasörleri Çözüm Gezgini'nde yeni proje](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="03705-150">Bir MVC web uygulaması yeoman scaffolds, hem tam sunucu ve istemci tarafı yapı desteği.</span><span class="sxs-lookup"><span data-stu-id="03705-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="03705-151">Sunucu tarafı bağımlılıkları altında listelenen **bağımlılıkları/NuGet** düğümü ve istemci tarafı bağımlılıkları **bağımlılıkları/Bower** Çözüm Gezgini düğümünün.</span><span class="sxs-lookup"><span data-stu-id="03705-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="03705-152">Proje yüklendiğinde bağımlılıkları otomatik olarak geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="03705-152">Dependencies are restored automatically when the project is loaded.</span></span>

![Çözüm Gezgini ağaç görünümünde bağımlılıklar düğümü altında Bower klasörü bağımlılıklarını listeleme açıktır.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="03705-154">Tüm bağımlılıkları geri yüklendiğinde basın **F5** projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="03705-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="03705-155">Varsayılan giriş sayfasını tarayıcıda görüntüler.</span><span class="sxs-lookup"><span data-stu-id="03705-155">The default home page displays in the browser.</span></span>

![Web uygulaması Microsoft Edge'de Aç](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="03705-157">Geri yükleme, oluşturma ve bir komut satırından barındırma</span><span class="sxs-lookup"><span data-stu-id="03705-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="03705-158">Hazırlama ve .NET Core CLI kullanarak web uygulamanızı barındırmak.</span><span class="sxs-lookup"><span data-stu-id="03705-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="03705-159">Bir komut isteminde, geçerli dizin projeyi içeren klasöre geçin (diğer bir deyişle, klasör içeren *.csproj* dosyası):</span><span class="sxs-lookup"><span data-stu-id="03705-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="03705-160">Projenin NuGet Paket bağımlılıklarını geri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="03705-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="03705-161">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="03705-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="03705-162">Platformlar arası [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu 5000 numaralı bağlantı noktasını dinlemeye başlayacak.</span><span class="sxs-lookup"><span data-stu-id="03705-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="03705-163">Bir web tarayıcısı açın ve gidin `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="03705-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Web uygulaması Microsoft Edge'de Aç](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="03705-165">Sub Oluşturucuları ile projenize ekleme</span><span class="sxs-lookup"><span data-stu-id="03705-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="03705-166">Yeoman kullanarak [sub oluşturucuları](https://github.com/omnisharp/generator-aspnet), ya da ekleyebilirsiniz bir `nuget.config` veya `web.config` Proje oluşturulduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="03705-166">Using Yeoman [sub generators](https://github.com/omnisharp/generator-aspnet), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="03705-167">Örneğin, dosya oluşturulmalı dizininden aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="03705-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="03705-168">Adlı bir NuGet yapılandırma dosyası sonucudur `nuget.config` aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="03705-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="03705-169">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="03705-169">Additional resources</span></span>

* [<span data-ttu-id="03705-170">Sunucuları (Kestrel ve WebListener)</span><span class="sxs-lookup"><span data-stu-id="03705-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="03705-171">Temelleri</span><span class="sxs-lookup"><span data-stu-id="03705-171">Fundamentals</span></span>](xref:fundamentals/index)
