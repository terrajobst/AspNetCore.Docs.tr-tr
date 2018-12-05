---
title: ASP.NET Core Razor sayfaları kullanmaya başlama
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Bu öğretici serisinde, ASP.NET Core Razor sayfaları kullanma işlemi gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturmak, veri erişimi için Entity Framework Core ve SQL Server kullanmak, arama işlevi eklemek, giriş doğrulaması eklemek ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861634"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="71f23-104">Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="71f23-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="71f23-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71f23-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71f23-106">Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="71f23-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="71f23-107">Uygulama bir veritabanı başlık yönetir.</span><span class="sxs-lookup"><span data-stu-id="71f23-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="71f23-108">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="71f23-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="71f23-109">Razor sayfaları web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="71f23-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="71f23-110">Ekleme ve bir modeli iskelesini.</span><span class="sxs-lookup"><span data-stu-id="71f23-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="71f23-111">Bir veritabanı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="71f23-111">Work with a database.</span></span>
> * <span data-ttu-id="71f23-112">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="71f23-112">Add search and validation.</span></span>

<span data-ttu-id="71f23-113">Sonunda, yönetebilir ve film başlıkları öğeleri görüntülemek bir uygulamaya sahip.</span><span class="sxs-lookup"><span data-stu-id="71f23-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="71f23-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="71f23-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="71f23-115">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="71f23-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71f23-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71f23-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71f23-117">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="71f23-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="71f23-118">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="71f23-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="71f23-119">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="71f23-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="71f23-120">Projeyi adlandırın önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod olduğunda eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="71f23-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="71f23-121">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="71f23-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="71f23-122">Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="71f23-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="71f23-124">Aşağıdaki başlangıç projesini oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="71f23-124">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="71f23-126">Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="71f23-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="71f23-127">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="71f23-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="71f23-128">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="71f23-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="71f23-129">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="71f23-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="71f23-130">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="71f23-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="71f23-131">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="71f23-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="71f23-132">Yukarıdaki görüntüde, 5001 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="71f23-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="71f23-133">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="71f23-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="71f23-134">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="71f23-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="71f23-135">Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="71f23-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71f23-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71f23-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71f23-137">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="71f23-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="71f23-138">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="71f23-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="71f23-139">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="71f23-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="71f23-140">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'RazorPagesMovie' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="71f23-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="71f23-141">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="71f23-141">Select **Yes**</span></span>

  * <span data-ttu-id="71f23-142">`dotnet new webapp -o RazorPagesMovie`: yeni bir Razor sayfaları projesindeki oluşturur *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="71f23-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="71f23-143">`code -r RazorPagesMovie`: Yükleyen *RazorPagesMovie.csproj* proje dosyası Visual Studio code'da.</span><span class="sxs-lookup"><span data-stu-id="71f23-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="71f23-144">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="71f23-144">Launch the app</span></span>

* <span data-ttu-id="71f23-145">Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="71f23-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="71f23-146">Visual Studio Code başlatıldığında başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="71f23-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="71f23-147">Adres çubuğu gösterir `localhost:port:5001` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="71f23-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="71f23-148">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="71f23-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="71f23-149">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="71f23-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="71f23-150">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="71f23-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="71f23-151">Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="71f23-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71f23-152">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71f23-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71f23-153">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="71f23-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="71f23-154">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) oluşturup bir Razor sayfaları projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="71f23-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="71f23-155">Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="71f23-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="71f23-156">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="71f23-156">Open the project</span></span>

<span data-ttu-id="71f23-157">Uygulamayı kapatmak için CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="71f23-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="71f23-158">Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="71f23-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="71f23-159">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="71f23-159">Launch the app</span></span>

<span data-ttu-id="71f23-160">Seçin **çalıştırın > hata ayıklama olmadan Başlat** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="71f23-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="71f23-161">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="71f23-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="71f23-162">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="71f23-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="71f23-163">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="71f23-163">This app doesn't track personal information.</span></span> <span data-ttu-id="71f23-164">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="71f23-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="71f23-166">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="71f23-166">The following image shows the app after accepting tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="71f23-168">Proje dosyaları ve klasörleri</span><span class="sxs-lookup"><span data-stu-id="71f23-168">Project files and folders</span></span>

<span data-ttu-id="71f23-169">Aşağıdaki tabloda, proje klasörleri ve dosyaları listeler.</span><span class="sxs-lookup"><span data-stu-id="71f23-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="71f23-170">Öğreticinin bu noktasında *Startup.cs* dosyasıdır anlamak en önemli.</span><span class="sxs-lookup"><span data-stu-id="71f23-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="71f23-171">Aşağıda sağlanan her bir bağlantısını incelemeniz gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="71f23-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="71f23-172">Bir dosya veya klasör proje hakkında daha fazla bilgi gerektiğinde bağlantılar bir başvuru olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="71f23-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="71f23-173">Dosya veya klasör</span><span class="sxs-lookup"><span data-stu-id="71f23-173">File or folder</span></span>              | <span data-ttu-id="71f23-174">Amaç</span><span class="sxs-lookup"><span data-stu-id="71f23-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="71f23-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="71f23-175">*wwwroot*</span></span> | <span data-ttu-id="71f23-176">Statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="71f23-176">Contains static files.</span></span> <span data-ttu-id="71f23-177">Bkz: [statik dosyalar](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="71f23-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="71f23-178">*Sayfalar*</span><span class="sxs-lookup"><span data-stu-id="71f23-178">*Pages*</span></span> | <span data-ttu-id="71f23-179">Klasör [Razor sayfaları](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="71f23-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="71f23-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="71f23-180">*appsettings.json*</span></span> | [<span data-ttu-id="71f23-181">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="71f23-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="71f23-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="71f23-182">*Program.cs*</span></span> | <span data-ttu-id="71f23-183">[Konaklar](xref:fundamentals/host/index) ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="71f23-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="71f23-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="71f23-184">*Startup.cs*</span></span> | <span data-ttu-id="71f23-185">Hizmetler ve istek ardışık düzenini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="71f23-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="71f23-186">Bkz: [başlangıç](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="71f23-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="71f23-187">Sayfalar klasöründe</span><span class="sxs-lookup"><span data-stu-id="71f23-187">The Pages folder</span></span>

<span data-ttu-id="71f23-188">*_Layout.cshtml* dosya ortak HTML öğeleri (betikleri ve stil sayfalarını) içerir ve uygulama düzenini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="71f23-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="71f23-189">Örneğin, tıkladığınızda **RazorPagesMovie**, **giriş**, veya **gizlilik**, aynı öğelere bakın.</span><span class="sxs-lookup"><span data-stu-id="71f23-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="71f23-190">Ortak öğeler, üst ve alt pencerenin üst gezinti menüsünde içerir.</span><span class="sxs-lookup"><span data-stu-id="71f23-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="71f23-191">Bkz: [Düzen](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="71f23-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="71f23-192">*_Viewımports.cshtml* dosyası her bir Razor sayfası alınan Razor yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="71f23-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="71f23-193">Bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="71f23-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="71f23-194">*_ViewStart.cshtml* Razor sayfaları ayarlar `Layout` kullanılacak özellik *_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="71f23-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="71f23-195">Bkz: [Düzen](xref:mvc/views/layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="71f23-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="71f23-196">*_ValidationScriptsPartial.cshtml* dosyası bir başvuru sağlar [jQuery](https://jquery.com/) doğrulama komut.</span><span class="sxs-lookup"><span data-stu-id="71f23-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="71f23-197">Zaman `Create` ve `Edit` sayfaları öğreticide daha sonra eklenen *_ValidationScriptsPartial.cshtml* dosya kullanılır.</span><span class="sxs-lookup"><span data-stu-id="71f23-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="71f23-198">`Index`, `Error`, ve `Privacy` sayfaları için sağlanır:</span><span class="sxs-lookup"><span data-stu-id="71f23-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="71f23-199">`Index`: Bir uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="71f23-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="71f23-200">`Error`: Hata bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="71f23-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="71f23-201">`Privacy`: Sitenin gizlilik ilkesi hakkındaki ayrıntıları belirtin.</span><span class="sxs-lookup"><span data-stu-id="71f23-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="71f23-202">Bu öğreticide, önceki sayfaları kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="71f23-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71f23-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71f23-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="71f23-204">Bir Razor sayfası ve PageModel arasında geçiş yapmak için F7 kullanın</span><span class="sxs-lookup"><span data-stu-id="71f23-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="71f23-205">F7 bir Razor sayfası arasında geçiş yapar (*\*.cshtml* dosyası) ve C# dosyası (*\*. cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="71f23-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71f23-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71f23-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="71f23-207">Kural olarak, Razor sayfası (*\*.cshtml* dosyası) ve ilişkili `PageModel` aynı kök dosya adını içerir.</span><span class="sxs-lookup"><span data-stu-id="71f23-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71f23-208">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71f23-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71f23-209">Kural olarak, Razor sayfası (*\*.cshtml* dosyası) ve ilişkili `PageModel` aynı kök dosya adını içerir.</span><span class="sxs-lookup"><span data-stu-id="71f23-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="71f23-210">Sonraki: model ekleme</span><span class="sxs-lookup"><span data-stu-id="71f23-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)