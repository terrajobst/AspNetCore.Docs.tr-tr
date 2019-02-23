---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 5403123abed2db6951adc3eac1bfa67b73e48ada
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744085"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="dd67a-103">ASP.NET Core MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="dd67a-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="dd67a-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dd67a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="dd67a-105">Bu öğreticide bir ASP.NET Core MVC web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="dd67a-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="dd67a-106">Uygulama bir veritabanı başlık yönetir.</span><span class="sxs-lookup"><span data-stu-id="dd67a-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="dd67a-107">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="dd67a-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dd67a-108">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dd67a-108">Create a web app.</span></span>
> * <span data-ttu-id="dd67a-109">Ekleme ve bir modeli iskelesini.</span><span class="sxs-lookup"><span data-stu-id="dd67a-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="dd67a-110">Bir veritabanı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-110">Work with a database.</span></span>
> * <span data-ttu-id="dd67a-111">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="dd67a-111">Add search and validation.</span></span>

<span data-ttu-id="dd67a-112">Sonunda, yönetmek ve film verileri görüntüleyen bir uygulama vardır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="dd67a-113">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="dd67a-113">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dd67a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd67a-114">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dd67a-115">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="dd67a-115">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="dd67a-117">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="dd67a-117">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="dd67a-118">Sol bölmede seçin **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="dd67a-118">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="dd67a-119">Orta bölmede seçin **ASP.NET Core Web uygulaması (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="dd67a-119">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="dd67a-120">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="dd67a-120">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="dd67a-121">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="dd67a-121">select **OK**</span></span>

![<span data-ttu-id="dd67a-122">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="dd67a-122">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="dd67a-123">Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="dd67a-123">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="dd67a-124">Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.2**</span><span class="sxs-lookup"><span data-stu-id="dd67a-124">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="dd67a-125">Seçin **Web uygulaması (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="dd67a-125">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="dd67a-126">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="dd67a-126">select **OK**.</span></span>

![<span data-ttu-id="dd67a-127">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="dd67a-127">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="dd67a-128">Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-128">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="dd67a-129">Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde.</span><span class="sxs-lookup"><span data-stu-id="dd67a-129">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="dd67a-130">Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="dd67a-130">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="dd67a-131">Seçin **Ctrl-F5** uygulamayı olmayan hata ayıklama modunda çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="dd67a-131">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

* <span data-ttu-id="dd67a-132">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-132">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="dd67a-133">Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dd67a-133">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dd67a-134">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-134">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="dd67a-135">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-135">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="dd67a-136">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="dd67a-136">In the image above, the port number is 5000.</span></span> <span data-ttu-id="dd67a-137">Tarayıcı gösterir URL'de `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dd67a-137">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="dd67a-138">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="dd67a-138">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="dd67a-139">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd67a-139">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="dd67a-140">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="dd67a-140">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="dd67a-141">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="dd67a-141">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="dd67a-143">Seçerek uygulama hatalarını ayıklayabilir **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="dd67a-143">You can debug the app by selecting the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dd67a-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dd67a-145">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dd67a-146">Bu öğretici, VS Code ile familarity varsayar.</span><span class="sxs-lookup"><span data-stu-id="dd67a-146">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="dd67a-147">Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="dd67a-147">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="dd67a-148">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dd67a-148">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="dd67a-149">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="dd67a-149">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="dd67a-150">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="dd67a-150">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="dd67a-151">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'MvcMovie' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="dd67a-151">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="dd67a-152">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="dd67a-152">Select **Yes**</span></span>

  * <span data-ttu-id="dd67a-153">`dotnet new mvc -o MvcMovie`: yeni bir ASP.NET Core MVC projesindeki oluşturur *MvcMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="dd67a-153">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="dd67a-154">`code -r MvcMovie`: Yükleri *MvcMovie.csproj* proje dosyası Visual Studio code'da.</span><span class="sxs-lookup"><span data-stu-id="dd67a-154">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="dd67a-155">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="dd67a-155">Launch the app</span></span>

* <span data-ttu-id="dd67a-156">Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="dd67a-156">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="dd67a-157">Visual Studio Code başlatıldığında başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dd67a-157">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="dd67a-158">Adres çubuğu gösterir `localhost:port:5001` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dd67a-158">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="dd67a-159">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-159">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="dd67a-160">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="dd67a-160">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="dd67a-161">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd67a-161">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="dd67a-162">Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="dd67a-162">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dd67a-163">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd67a-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dd67a-164">Seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="dd67a-164">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="dd67a-166">Seçin **.NET Core uygulaması** > **ASP.NET Core** > **ASP.NET Core Web uygulaması (MVC)** > **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="dd67a-166">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="dd67a-168">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="dd67a-168">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="dd67a-169">Projeyi adlandırın **MvcMovie**ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="dd67a-169">Name the project **MvcMovie**, and then select **Create**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="dd67a-170">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="dd67a-170">Launch the app</span></span>

<span data-ttu-id="dd67a-171">Seçin **çalıştırma** > **hata ayıklama olmadan Başlat** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="dd67a-171">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="dd67a-172">Başlatıldığında Mac için Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel) sunucusunda, bir tarayıcı başlatır ve gider `http://localhost:port`burada *bağlantı noktası* bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-172">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

* <span data-ttu-id="dd67a-173">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dd67a-173">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dd67a-174">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-174">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="dd67a-175">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd67a-175">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="dd67a-176">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="dd67a-176">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="dd67a-177">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **çalıştırma** menüsü.</span><span class="sxs-lookup"><span data-stu-id="dd67a-177">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---  
<!-- End of VS tabs -->

* <span data-ttu-id="dd67a-178">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="dd67a-178">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="dd67a-179">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="dd67a-179">This app doesn't track personal information.</span></span> <span data-ttu-id="dd67a-180">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dd67a-180">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="dd67a-182">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="dd67a-182">The following image shows the app after accepting tracking:</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="dd67a-184">Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi ve biraz kod yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="dd67a-184">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dd67a-185">Next</span><span class="sxs-lookup"><span data-stu-id="dd67a-185">Next</span></span>](adding-controller.md)  
