---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 901257efdfbc7b36249233745175f5ed253da2c7
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722910"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="0ff42-103">ASP.NET Core MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="0ff42-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="0ff42-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ff42-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="0ff42-105">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="0ff42-106">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="0ff42-107">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="0ff42-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ff42-108">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0ff42-108">Create a web app.</span></span>
> * <span data-ttu-id="0ff42-109">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="0ff42-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="0ff42-110">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="0ff42-110">Work with a database.</span></span>
> * <span data-ttu-id="0ff42-111">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-111">Add search and validation.</span></span>

<span data-ttu-id="0ff42-112">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="0ff42-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="0ff42-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ff42-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ff42-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ff42-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ff42-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="0ff42-117">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0ff42-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ff42-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ff42-119">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="0ff42-120">**ASP.NET Core Web uygulaması** ' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="0ff42-122">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="0ff42-123">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)

* <span data-ttu-id="0ff42-125">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="0ff42-126">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="0ff42-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="0ff42-127">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="0ff42-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="0ff42-128">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="0ff42-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="0ff42-129">Bu, temel bir başlatıcı projem.</span><span class="sxs-lookup"><span data-stu-id="0ff42-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ff42-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ff42-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ff42-131">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0ff42-132">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="0ff42-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="0ff42-133">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0ff42-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0ff42-134">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0ff42-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0ff42-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="0ff42-136">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' Mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="0ff42-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="0ff42-137">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="0ff42-137">Select **Yes**</span></span>

  * <span data-ttu-id="0ff42-138">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0ff42-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="0ff42-139">`code -r MvcMovie`: *Mvcmovie. csproj* proje dosyasını Visual Studio Code yükler.</span><span class="sxs-lookup"><span data-stu-id="0ff42-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ff42-140">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0ff42-141">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-141">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="0ff42-143">Bir **sonraki**> **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="0ff42-145">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 3,1**'nin **hedef çerçevesini** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="0ff42-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

  ![macOS .NET Core 3,1 seçimi](./start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="0ff42-147">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="0ff42-148">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0ff42-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ff42-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0ff42-150">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="0ff42-151">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0ff42-152">Adres çubuğunun `example.com`gibi `localhost:port#` gösterdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ff42-153">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ff42-154">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="0ff42-155">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ff42-156">Birçok geliştirici uygulamayı hemen başlatmak ve değişiklikleri görüntülemek için hata ayıklamasız modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0ff42-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="0ff42-157">**Hata ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0ff42-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="0ff42-159">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="0ff42-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="0ff42-161">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0ff42-161">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ff42-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ff42-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ff42-164">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="0ff42-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="0ff42-165">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `https://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="0ff42-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="0ff42-166">Adres çubuğu `example.com`gibi değil `localhost:port:5001` gösterir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="0ff42-167">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0ff42-168">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="0ff42-169">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ff42-170">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0ff42-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ff42-172">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0ff42-173">Uygulamayı başlatmak için **çalıştır** > **hata ayıklama olmadan Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0ff42-174">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0ff42-175">Adres çubuğu `example.com`gibi değil `localhost:port#` gösterir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ff42-176">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ff42-177">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0ff42-178">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="0ff42-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0ff42-179">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ff42-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="0ff42-180">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0ff42-180">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="0ff42-182">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ff42-182">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ff42-183">Next</span><span class="sxs-lookup"><span data-stu-id="0ff42-183">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="0ff42-184">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-184">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="0ff42-185">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-185">The app manages a database of movie titles.</span></span> <span data-ttu-id="0ff42-186">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="0ff42-186">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ff42-187">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0ff42-187">Create a web app.</span></span>
> * <span data-ttu-id="0ff42-188">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="0ff42-188">Add and scaffold a model.</span></span>
> * <span data-ttu-id="0ff42-189">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="0ff42-189">Work with a database.</span></span>
> * <span data-ttu-id="0ff42-190">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-190">Add search and validation.</span></span>

<span data-ttu-id="0ff42-191">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-191">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="0ff42-192">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="0ff42-192">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ff42-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-193">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ff42-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ff42-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ff42-195">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="0ff42-196">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0ff42-196">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ff42-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ff42-198">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-198">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="0ff42-199">**ASP.NET Core Web uygulaması** ' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-199">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="0ff42-201">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-201">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="0ff42-202">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-202">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)


* <span data-ttu-id="0ff42-204">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-204">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="0ff42-205">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="0ff42-205">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="0ff42-206">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="0ff42-206">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="0ff42-207">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="0ff42-207">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="0ff42-208">Bu, temel bir başlatıcı projem ve başlamak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-208">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ff42-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ff42-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ff42-210">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-210">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0ff42-211">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="0ff42-211">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="0ff42-212">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0ff42-212">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0ff42-213">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-213">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0ff42-214">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0ff42-214">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="0ff42-215">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' Mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="0ff42-215">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="0ff42-216">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="0ff42-216">Select **Yes**</span></span>

  * <span data-ttu-id="0ff42-217">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0ff42-217">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="0ff42-218">`code -r MvcMovie`: *Mvcmovie. csproj* proje dosyasını Visual Studio Code yükler.</span><span class="sxs-lookup"><span data-stu-id="0ff42-218">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ff42-219">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0ff42-220">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-220">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="0ff42-222">Bir **sonraki**> **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-222">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="0ff42-224">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 2,2**'nin varsayılan **hedef çerçevesini** kabul edin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-224">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2,2 seçimi](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="0ff42-226">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-226">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="0ff42-227">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0ff42-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ff42-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0ff42-229">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-229">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="0ff42-230">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0ff42-231">Adres çubuğunun `example.com`gibi `localhost:port#` gösterdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-231">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ff42-232">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-232">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ff42-233">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="0ff42-234">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-234">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ff42-235">Birçok geliştirici uygulamayı hemen başlatmak ve değişiklikleri görüntülemek için hata ayıklamasız modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0ff42-235">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="0ff42-236">**Hata ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0ff42-236">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="0ff42-238">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="0ff42-238">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="0ff42-240">İzlemeye onay vermek için **Kabul Et**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-240">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0ff42-241">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="0ff42-241">This app doesn't track personal information.</span></span> <span data-ttu-id="0ff42-242">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-242">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="0ff42-244">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0ff42-244">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ff42-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ff42-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ff42-247">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="0ff42-247">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="0ff42-248">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `https://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="0ff42-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="0ff42-249">Adres çubuğu `example.com`gibi değil `localhost:port:5001` gösterir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-249">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="0ff42-250">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0ff42-251">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-251">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="0ff42-252">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-252">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ff42-253">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="0ff42-253">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="0ff42-254">İzlemeye onay vermek için **Kabul Et**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-254">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0ff42-255">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="0ff42-255">This app doesn't track personal information.</span></span> <span data-ttu-id="0ff42-256">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-256">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="0ff42-258">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0ff42-258">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ff42-260">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ff42-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0ff42-261">Uygulamayı başlatmak için **çalıştır** > **hata ayıklama olmadan Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-261">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0ff42-262">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-262">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0ff42-263">Adres çubuğu `example.com`gibi değil `localhost:port#` gösterir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-263">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ff42-264">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-264">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ff42-265">Visual Studio bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ff42-265">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0ff42-266">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="0ff42-266">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0ff42-267">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ff42-267">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="0ff42-268">İzlemeye onay vermek için **Kabul Et**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="0ff42-268">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0ff42-269">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="0ff42-269">This app doesn't track personal information.</span></span> <span data-ttu-id="0ff42-270">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="0ff42-270">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="0ff42-272">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0ff42-272">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="0ff42-274">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ff42-274">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ff42-275">Next</span><span class="sxs-lookup"><span data-stu-id="0ff42-275">Next</span></span>](adding-controller.md)

::: moniker-end
