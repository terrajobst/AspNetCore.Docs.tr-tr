---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dc3499c89860190b76d6be7b8abeeaef827880d6
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491254"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="0dc18-103">ASP.NET Core MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="0dc18-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="0dc18-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0dc18-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="0dc18-105">Bu öğreticide bir ASP.NET Core MVC web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="0dc18-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="0dc18-106">Uygulama bir veritabanı başlık yönetir.</span><span class="sxs-lookup"><span data-stu-id="0dc18-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="0dc18-107">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="0dc18-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0dc18-108">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0dc18-108">Create a web app.</span></span>
> * <span data-ttu-id="0dc18-109">Ekleme ve bir modeli iskelesini.</span><span class="sxs-lookup"><span data-stu-id="0dc18-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="0dc18-110">Bir veritabanı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-110">Work with a database.</span></span>
> * <span data-ttu-id="0dc18-111">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0dc18-111">Add search and validation.</span></span>

<span data-ttu-id="0dc18-112">Sonunda, yönetmek ve film verileri görüntüleyen bir uygulama vardır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="0dc18-113">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0dc18-113">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0dc18-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0dc18-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0dc18-115">Visual Studio seçin **yeni bir proje oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-115">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="0dc18-116">Selecct **ASP.NET Core Web uygulaması** seçip **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-116">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="0dc18-118">Projeyi adlandırın **MvcMovie** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-118">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="0dc18-119">Projeyi adlandırın önemlidir **MvcMovie** kod kopyaladığınızda, ad alanı eşleşmesi.</span><span class="sxs-lookup"><span data-stu-id="0dc18-119">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)


* <span data-ttu-id="0dc18-121">Seçin **Web Application(Model-View-Controller)** ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-121">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="0dc18-122">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .NET Core</span><span class="sxs-lookup"><span data-stu-id="0dc18-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="0dc18-123">Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan şablonu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-123">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="0dc18-124">Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde.</span><span class="sxs-lookup"><span data-stu-id="0dc18-124">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="0dc18-125">Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="0dc18-125">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0dc18-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0dc18-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0dc18-127">Bu öğretici, VS Code ile familarity varsayar.</span><span class="sxs-lookup"><span data-stu-id="0dc18-127">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0dc18-128">Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0dc18-128">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="0dc18-129">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0dc18-129">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0dc18-130">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="0dc18-130">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0dc18-131">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0dc18-131">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="0dc18-132">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'MvcMovie' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="0dc18-132">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="0dc18-133">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="0dc18-133">Select **Yes**</span></span>

  * <span data-ttu-id="0dc18-134">`dotnet new mvc -o MvcMovie`: yeni bir ASP.NET Core MVC projesindeki oluşturur *MvcMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="0dc18-134">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="0dc18-135">`code -r MvcMovie`: Yükleri *MvcMovie.csproj* proje dosyası Visual Studio code'da.</span><span class="sxs-lookup"><span data-stu-id="0dc18-135">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0dc18-136">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0dc18-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0dc18-137">Seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-137">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="0dc18-139">Seçin **.NET Core** > **uygulama** > **Web uygulaması (Model-View-Controller)**  > **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-139">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="0dc18-141">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-141">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 seçimi](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="0dc18-143">Projeyi adlandırın **MvcMovie**ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0dc18-143">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="0dc18-144">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0dc18-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0dc18-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0dc18-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0dc18-146">Seçin **Ctrl-F5** uygulamayı olmayan hata ayıklama modunda çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="0dc18-146">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="0dc18-147">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0dc18-148">Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0dc18-148">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0dc18-149">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-149">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0dc18-150">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-150">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="0dc18-151">Ctrl + F5 (hata ayıklama olmayan mod) ile uygulamayı çalıştırdığınızda, kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-151">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0dc18-152">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0dc18-152">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="0dc18-153">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="0dc18-153">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="0dc18-155">Seçerek uygulama hatalarını ayıklayabilir **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="0dc18-155">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="0dc18-157">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="0dc18-157">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0dc18-158">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="0dc18-158">This app doesn't track personal information.</span></span> <span data-ttu-id="0dc18-159">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0dc18-159">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="0dc18-161">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="0dc18-161">The following image shows the app after accepting tracking:</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0dc18-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0dc18-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0dc18-164">Hata Ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="0dc18-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="0dc18-165">Visual Studio Code başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0dc18-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="0dc18-166">Adres çubuğu gösterir `localhost:port:5001` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0dc18-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="0dc18-167">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0dc18-168">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="0dc18-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="0dc18-169">Ctrl + F5 (hata ayıklama olmayan mod) ile uygulamayı çalıştırdığınızda, kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0dc18-170">Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0dc18-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="0dc18-171">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="0dc18-171">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0dc18-172">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="0dc18-172">This app doesn't track personal information.</span></span> <span data-ttu-id="0dc18-173">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0dc18-173">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="0dc18-175">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="0dc18-175">The following image shows the app after accepting tracking:</span></span>

  ![Giriş ya da dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0dc18-177">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0dc18-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0dc18-178">Seçin **çalıştırma** > **hata ayıklama olmadan Başlat** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="0dc18-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0dc18-179">Başlatıldığında Mac için Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel) sunucusunda, bir tarayıcı başlatır ve gider `http://localhost:port`burada *bağlantı noktası* bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0dc18-180">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0dc18-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0dc18-181">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0dc18-182">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0dc18-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0dc18-183">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="0dc18-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0dc18-184">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **çalıştırma** menüsü.</span><span class="sxs-lookup"><span data-stu-id="0dc18-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="0dc18-185">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="0dc18-185">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0dc18-186">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="0dc18-186">This app doesn't track personal information.</span></span> <span data-ttu-id="0dc18-187">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0dc18-187">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="0dc18-189">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="0dc18-189">The following image shows the app after accepting tracking:</span></span>

  ![Giriş ya da dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="0dc18-191">Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi ve biraz kod yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="0dc18-191">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0dc18-192">Next</span><span class="sxs-lookup"><span data-stu-id="0dc18-192">Next</span></span>](adding-controller.md)
