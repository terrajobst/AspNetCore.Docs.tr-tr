---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1bc5da7dcab5a5070260ca464a82b650cc4838bf
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978464"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="0934b-103">ASP.NET Core MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="0934b-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="0934b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0934b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="0934b-105">Bu öğreticide bir ASP.NET Core MVC web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="0934b-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="0934b-106">Uygulama bir veritabanı başlık yönetir.</span><span class="sxs-lookup"><span data-stu-id="0934b-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="0934b-107">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="0934b-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0934b-108">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0934b-108">Create a web app.</span></span>
> * <span data-ttu-id="0934b-109">Ekleme ve bir modeli iskelesini.</span><span class="sxs-lookup"><span data-stu-id="0934b-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="0934b-110">Bir veritabanı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="0934b-110">Work with a database.</span></span>
> * <span data-ttu-id="0934b-111">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0934b-111">Add search and validation.</span></span>

<span data-ttu-id="0934b-112">Sonunda, yönetmek ve film verileri görüntüleyen bir uygulama vardır.</span><span class="sxs-lookup"><span data-stu-id="0934b-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="0934b-113">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0934b-113">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0934b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0934b-114">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0934b-115">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="0934b-115">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="0934b-117">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="0934b-117">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="0934b-118">Sol bölmede seçin **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="0934b-118">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="0934b-119">Orta bölmede seçin **ASP.NET Core Web uygulaması (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="0934b-119">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="0934b-120">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="0934b-120">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="0934b-121">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="0934b-121">select **OK**</span></span>

![<span data-ttu-id="0934b-122">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .NET Core</span><span class="sxs-lookup"><span data-stu-id="0934b-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="0934b-123">Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="0934b-123">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="0934b-124">Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.2**</span><span class="sxs-lookup"><span data-stu-id="0934b-124">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="0934b-125">Seçin **Web uygulaması (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="0934b-125">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="0934b-126">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="0934b-126">select **OK**.</span></span>

![<span data-ttu-id="0934b-127">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .NET Core</span><span class="sxs-lookup"><span data-stu-id="0934b-127">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="0934b-128">Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0934b-128">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="0934b-129">Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde.</span><span class="sxs-lookup"><span data-stu-id="0934b-129">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="0934b-130">Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="0934b-130">This is a basic starter project, and it's a good place to start.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0934b-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0934b-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0934b-132">Bu öğretici, VS Code ile familarity varsayar.</span><span class="sxs-lookup"><span data-stu-id="0934b-132">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0934b-133">Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0934b-133">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="0934b-134">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0934b-134">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0934b-135">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="0934b-135">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0934b-136">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0934b-136">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="0934b-137">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'MvcMovie' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="0934b-137">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="0934b-138">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="0934b-138">Select **Yes**</span></span>

  * <span data-ttu-id="0934b-139">`dotnet new mvc -o MvcMovie`: yeni bir ASP.NET Core MVC projesindeki oluşturur *MvcMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="0934b-139">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="0934b-140">`code -r MvcMovie`: Yükleri *MvcMovie.csproj* proje dosyası Visual Studio code'da.</span><span class="sxs-lookup"><span data-stu-id="0934b-140">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0934b-141">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0934b-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0934b-142">Seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="0934b-142">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="0934b-144">Seçin **.NET Core uygulaması** > **ASP.NET Core** > **ASP.NET Core Web uygulaması (MVC)** > **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0934b-144">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="0934b-146">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="0934b-146">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="0934b-147">Projeyi adlandırın **MvcMovie**ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0934b-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---  
<!-- End of VS tabs -->

### <a name="run-the-app"></a><span data-ttu-id="0934b-148">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0934b-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0934b-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0934b-149">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0934b-150">Seçin **Ctrl-F5** uygulamayı olmayan hata ayıklama modunda çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="0934b-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="0934b-151">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="0934b-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0934b-152">Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0934b-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0934b-153">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0934b-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0934b-154">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0934b-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="0934b-155">Ctrl + F5 (hata ayıklama olmayan mod) ile uygulamayı çalıştırdığınızda, kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0934b-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0934b-156">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0934b-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="0934b-157">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="0934b-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="0934b-159">Seçerek uygulama hatalarını ayıklayabilir **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="0934b-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0934b-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0934b-161">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="0934b-162">Hata Ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="0934b-162">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="0934b-163">Visual Studio Code başlatıldığında başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0934b-163">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="0934b-164">Adres çubuğu gösterir `localhost:port:5001` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0934b-164">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="0934b-165">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0934b-165">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0934b-166">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="0934b-166">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="0934b-167">Ctrl + F5 (hata ayıklama olmayan mod) ile uygulamayı çalıştırdığınızda, kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0934b-167">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0934b-168">Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0934b-168">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0934b-169">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0934b-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0934b-170">Seçin **çalıştırma** > **hata ayıklama olmadan Başlat** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="0934b-170">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0934b-171">Başlatıldığında Mac için Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel) sunucusunda, bir tarayıcı başlatır ve gider `http://localhost:port`burada *bağlantı noktası* bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="0934b-171">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0934b-172">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0934b-172">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0934b-173">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0934b-173">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0934b-174">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0934b-174">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0934b-175">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="0934b-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0934b-176">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **çalıştırma** menüsü.</span><span class="sxs-lookup"><span data-stu-id="0934b-176">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

------

* <span data-ttu-id="0934b-177">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="0934b-177">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0934b-178">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="0934b-178">This app doesn't track personal information.</span></span> <span data-ttu-id="0934b-179">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0934b-179">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="0934b-181">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="0934b-181">The following image shows the app after accepting tracking:</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="0934b-183">Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi ve biraz kod yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="0934b-183">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0934b-184">Next</span><span class="sxs-lookup"><span data-stu-id="0934b-184">Next</span></span>](adding-controller.md)  
