---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: e70384a6f20f3ef06059ed6b51c76e923187c317
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354932"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="27a26-103">ASP.NET Core MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="27a26-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="27a26-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="27a26-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="27a26-105">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="27a26-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="27a26-106">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="27a26-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="27a26-107">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="27a26-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27a26-108">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="27a26-108">Create a web app.</span></span>
> * <span data-ttu-id="27a26-109">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="27a26-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="27a26-110">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="27a26-110">Work with a database.</span></span>
> * <span data-ttu-id="27a26-111">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="27a26-111">Add search and validation.</span></span>

<span data-ttu-id="27a26-112">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="27a26-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="27a26-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="27a26-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27a26-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27a26-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27a26-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27a26-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="27a26-117">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="27a26-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27a26-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="27a26-119">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="27a26-120">**ASP.NET Core Web uygulaması** ' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="27a26-122">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="27a26-123">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="27a26-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)

* <span data-ttu-id="27a26-125">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="27a26-126">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="27a26-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="27a26-127">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="27a26-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="27a26-128">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="27a26-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="27a26-129">Bu, temel bir başlatıcı projem.</span><span class="sxs-lookup"><span data-stu-id="27a26-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27a26-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27a26-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="27a26-131">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="27a26-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="27a26-132">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="27a26-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="27a26-133">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="27a26-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="27a26-134">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="27a26-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="27a26-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="27a26-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="27a26-136">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' Mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="27a26-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="27a26-137">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="27a26-137">Select **Yes**</span></span>

  * <span data-ttu-id="27a26-138">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27a26-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="27a26-139">`code -r MvcMovie`: *Mvcmovie. csproj* proje dosyasını Visual Studio Code yükler.</span><span class="sxs-lookup"><span data-stu-id="27a26-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27a26-140">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="27a26-141">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-141">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="27a26-143">Bir **sonraki**> **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="27a26-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="27a26-145">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 3,1**'nin **hedef çerçevesini** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="27a26-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="27a26-146">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="27a26-147">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="27a26-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27a26-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27a26-149">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="27a26-150">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="27a26-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="27a26-151">Adres çubuğunun `example.com`gibi `localhost:port#` gösterdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="27a26-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="27a26-152">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="27a26-153">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="27a26-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="27a26-154">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="27a26-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="27a26-155">Birçok geliştirici uygulamayı hemen başlatmak ve değişiklikleri görüntülemek için hata ayıklamasız modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="27a26-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="27a26-156">**Hata ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="27a26-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="27a26-158">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="27a26-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="27a26-160">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="27a26-160">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27a26-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27a26-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="27a26-163">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="27a26-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="27a26-164">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `https://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="27a26-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="27a26-165">Adres çubuğu `example.com`gibi değil `localhost:port:5001` gösterir.</span><span class="sxs-lookup"><span data-stu-id="27a26-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="27a26-166">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="27a26-167">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="27a26-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="27a26-168">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="27a26-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="27a26-169">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="27a26-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27a26-171">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="27a26-172">Uygulamayı başlatmak için **çalıştır** > **hata ayıklama olmadan Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="27a26-173">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="27a26-174">Adres çubuğu `example.com`gibi değil `localhost:port#` gösterir.</span><span class="sxs-lookup"><span data-stu-id="27a26-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="27a26-175">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="27a26-176">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="27a26-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="27a26-177">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="27a26-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="27a26-178">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27a26-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="27a26-179">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="27a26-179">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="27a26-181">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27a26-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="27a26-182">Next</span><span class="sxs-lookup"><span data-stu-id="27a26-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="27a26-183">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="27a26-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="27a26-184">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="27a26-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="27a26-185">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="27a26-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27a26-186">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="27a26-186">Create a web app.</span></span>
> * <span data-ttu-id="27a26-187">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="27a26-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="27a26-188">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="27a26-188">Work with a database.</span></span>
> * <span data-ttu-id="27a26-189">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="27a26-189">Add search and validation.</span></span>

<span data-ttu-id="27a26-190">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="27a26-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="27a26-191">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="27a26-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27a26-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27a26-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27a26-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27a26-194">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="27a26-195">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="27a26-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27a26-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="27a26-197">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="27a26-198">**ASP.NET Core Web uygulaması** ' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-198">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="27a26-200">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="27a26-201">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="27a26-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)


* <span data-ttu-id="27a26-203">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="27a26-204">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="27a26-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="27a26-205">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="27a26-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="27a26-206">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="27a26-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="27a26-207">Bu, temel bir başlatıcı projem ve başlamak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="27a26-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27a26-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27a26-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="27a26-209">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="27a26-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="27a26-210">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="27a26-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="27a26-211">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="27a26-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="27a26-212">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="27a26-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="27a26-213">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="27a26-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="27a26-214">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' Mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="27a26-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="27a26-215">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="27a26-215">Select **Yes**</span></span>

  * <span data-ttu-id="27a26-216">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27a26-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="27a26-217">`code -r MvcMovie`: *Mvcmovie. csproj* proje dosyasını Visual Studio Code yükler.</span><span class="sxs-lookup"><span data-stu-id="27a26-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27a26-218">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="27a26-219">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-219">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="27a26-221">Bir **sonraki**> **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="27a26-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="27a26-223">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 2,2**'nin varsayılan **hedef çerçevesini** kabul edin.</span><span class="sxs-lookup"><span data-stu-id="27a26-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2,2 seçimi](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="27a26-225">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="27a26-226">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="27a26-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27a26-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27a26-228">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="27a26-229">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="27a26-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="27a26-230">Adres çubuğunun `example.com`gibi `localhost:port#` gösterdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="27a26-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="27a26-231">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="27a26-232">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="27a26-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="27a26-233">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="27a26-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="27a26-234">Birçok geliştirici uygulamayı hemen başlatmak ve değişiklikleri görüntülemek için hata ayıklamasız modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="27a26-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="27a26-235">**Hata ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="27a26-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="27a26-237">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="27a26-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="27a26-239">İzlemeye onay vermek için **Kabul Et**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="27a26-240">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="27a26-240">This app doesn't track personal information.</span></span> <span data-ttu-id="27a26-241">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="27a26-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="27a26-243">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="27a26-243">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="27a26-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="27a26-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="27a26-246">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="27a26-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="27a26-247">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `https://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="27a26-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="27a26-248">Adres çubuğu `example.com`gibi değil `localhost:port:5001` gösterir.</span><span class="sxs-lookup"><span data-stu-id="27a26-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="27a26-249">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="27a26-250">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="27a26-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="27a26-251">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="27a26-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="27a26-252">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="27a26-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="27a26-253">İzlemeye onay vermek için **Kabul Et**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="27a26-254">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="27a26-254">This app doesn't track personal information.</span></span> <span data-ttu-id="27a26-255">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="27a26-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="27a26-257">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="27a26-257">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="27a26-259">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27a26-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="27a26-260">Uygulamayı başlatmak için **çalıştır** > **hata ayıklama olmadan Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="27a26-261">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="27a26-262">Adres çubuğu `example.com`gibi değil `localhost:port#` gösterir.</span><span class="sxs-lookup"><span data-stu-id="27a26-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="27a26-263">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="27a26-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="27a26-264">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="27a26-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="27a26-265">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="27a26-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="27a26-266">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27a26-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="27a26-267">İzlemeye onay vermek için **Kabul Et**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="27a26-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="27a26-268">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="27a26-268">This app doesn't track personal information.</span></span> <span data-ttu-id="27a26-269">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="27a26-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="27a26-271">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="27a26-271">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="27a26-273">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27a26-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="27a26-274">Next</span><span class="sxs-lookup"><span data-stu-id="27a26-274">Next</span></span>](adding-controller.md)

::: moniker-end
