---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f07afb15c0a31110c20d96a5db5c02030cefe5d5
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519094"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="afdcf-103">ASP.NET Core MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="afdcf-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="afdcf-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="afdcf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="afdcf-105">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="afdcf-106">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="afdcf-107">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="afdcf-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="afdcf-108">Bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="afdcf-108">Create a web app.</span></span>
> * <span data-ttu-id="afdcf-109">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="afdcf-110">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-110">Work with a database.</span></span>
> * <span data-ttu-id="afdcf-111">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-111">Add search and validation.</span></span>

<span data-ttu-id="afdcf-112">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="afdcf-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="afdcf-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="afdcf-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="afdcf-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="afdcf-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="afdcf-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="afdcf-117">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="afdcf-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="afdcf-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="afdcf-119">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="afdcf-120">**ASP.NET Core Web uygulaması** ' nı seçin ve ardından **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="afdcf-122">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="afdcf-123">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)

* <span data-ttu-id="afdcf-125">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="afdcf-126">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="afdcf-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="afdcf-127">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="afdcf-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="afdcf-128">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="afdcf-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="afdcf-129">Bu, temel bir başlatıcı projem.</span><span class="sxs-lookup"><span data-stu-id="afdcf-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="afdcf-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="afdcf-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="afdcf-131">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="afdcf-132">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="afdcf-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="afdcf-133">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="afdcf-134">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="afdcf-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="afdcf-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="afdcf-136">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' Mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="afdcf-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="afdcf-137">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="afdcf-137">Select **Yes**</span></span>

  * <span data-ttu-id="afdcf-138">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="afdcf-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="afdcf-139">`code -r MvcMovie`: *Mvcmovie. csproj* proje dosyasını Visual Studio Code yükler.</span><span class="sxs-lookup"><span data-stu-id="afdcf-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="afdcf-140">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="afdcf-141">**Dosya** > **yeni çözüm**seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-141">Select **File** > **New Solution**.</span></span>

  ![macOS yeni çözüm](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="afdcf-143">**.NET Core** > **uygulama** > **Web uygulaması (Model-View-Controller)** @no__t **-5 '** i seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="afdcf-145">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 3,0**'nin **hedef çerçevesini** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="afdcf-146">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="afdcf-147">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="afdcf-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="afdcf-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="afdcf-149">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="afdcf-150">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="afdcf-151">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey gösterilmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="afdcf-152">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="afdcf-153">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="afdcf-154">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="afdcf-155">Birçok geliştirici, uygulamayı hızlı bir şekilde başlatmak ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="afdcf-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="afdcf-156">**Hata ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="afdcf-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="afdcf-158">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="afdcf-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="afdcf-160">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="afdcf-160">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="afdcf-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="afdcf-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="afdcf-163">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="afdcf-164">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `https://localhost:5001` ' e gider.</span><span class="sxs-lookup"><span data-stu-id="afdcf-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="afdcf-165">Adres çubuğunda `localhost:port:5001` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="afdcf-166">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="afdcf-167">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="afdcf-168">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="afdcf-169">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="afdcf-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="afdcf-171">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="afdcf-172">Uygulamayı başlatmak için @no__t **Çalıştır**' ı veya**hata ayıklama olmadan Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="afdcf-173">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port` ' e gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="afdcf-174">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="afdcf-175">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="afdcf-176">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="afdcf-177">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="afdcf-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="afdcf-178">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afdcf-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="afdcf-179">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="afdcf-179">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="afdcf-181">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afdcf-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="afdcf-182">Next</span><span class="sxs-lookup"><span data-stu-id="afdcf-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="afdcf-183">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="afdcf-184">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="afdcf-185">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="afdcf-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="afdcf-186">Bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="afdcf-186">Create a web app.</span></span>
> * <span data-ttu-id="afdcf-187">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="afdcf-188">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-188">Work with a database.</span></span>
> * <span data-ttu-id="afdcf-189">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-189">Add search and validation.</span></span>

<span data-ttu-id="afdcf-190">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="afdcf-191">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="afdcf-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="afdcf-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="afdcf-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="afdcf-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="afdcf-194">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="afdcf-195">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="afdcf-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="afdcf-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="afdcf-197">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="afdcf-198">**ASP.NET Core Web uygulamasını** seçip **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="afdcf-200">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="afdcf-201">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)


* <span data-ttu-id="afdcf-203">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="afdcf-204">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="afdcf-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="afdcf-205">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="afdcf-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="afdcf-206">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="afdcf-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="afdcf-207">Bu, temel bir başlatıcı projem ve başlamak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="afdcf-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="afdcf-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="afdcf-209">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="afdcf-210">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="afdcf-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="afdcf-211">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="afdcf-212">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="afdcf-213">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="afdcf-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="afdcf-214">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' Mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="afdcf-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="afdcf-215">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="afdcf-215">Select **Yes**</span></span>

  * <span data-ttu-id="afdcf-216">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="afdcf-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="afdcf-217">`code -r MvcMovie`: *Mvcmovie. csproj* proje dosyasını Visual Studio Code yükler.</span><span class="sxs-lookup"><span data-stu-id="afdcf-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="afdcf-218">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="afdcf-219">**Dosya** > **yeni çözüm**seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-219">Select **File** > **New Solution**.</span></span>

  ![macOS yeni çözüm](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="afdcf-221">**.NET Core** > **uygulama** > **Web uygulaması (Model-View-Controller)** @no__t **-5 '** i seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="afdcf-223">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 2,2**'nin varsayılan **hedef çerçevesini** kabul edin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2,2 seçimi](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="afdcf-225">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="afdcf-226">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="afdcf-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="afdcf-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="afdcf-228">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="afdcf-229">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="afdcf-230">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey gösterilmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="afdcf-231">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="afdcf-232">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="afdcf-233">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="afdcf-234">Birçok geliştirici, uygulamayı hızlı bir şekilde başlatmak ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="afdcf-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="afdcf-235">**Hata ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="afdcf-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="afdcf-237">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="afdcf-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="afdcf-239">İzlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="afdcf-240">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="afdcf-240">This app doesn't track personal information.</span></span> <span data-ttu-id="afdcf-241">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="afdcf-243">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="afdcf-243">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="afdcf-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="afdcf-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="afdcf-246">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="afdcf-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="afdcf-247">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `https://localhost:5001` ' e gider.</span><span class="sxs-lookup"><span data-stu-id="afdcf-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="afdcf-248">Adres çubuğunda `localhost:port:5001` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="afdcf-249">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="afdcf-250">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="afdcf-251">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="afdcf-252">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="afdcf-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="afdcf-253">İzlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="afdcf-254">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="afdcf-254">This app doesn't track personal information.</span></span> <span data-ttu-id="afdcf-255">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="afdcf-257">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="afdcf-257">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="afdcf-259">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="afdcf-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="afdcf-260">Uygulamayı başlatmak için @no__t **Çalıştır**' ı veya**hata ayıklama olmadan Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="afdcf-261">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port` ' e gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="afdcf-262">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="afdcf-263">Bunun nedeni, `localhost` yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="afdcf-264">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afdcf-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="afdcf-265">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="afdcf-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="afdcf-266">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afdcf-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="afdcf-267">İzlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="afdcf-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="afdcf-268">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="afdcf-268">This app doesn't track personal information.</span></span> <span data-ttu-id="afdcf-269">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="afdcf-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="afdcf-271">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="afdcf-271">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="afdcf-273">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afdcf-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="afdcf-274">Next</span><span class="sxs-lookup"><span data-stu-id="afdcf-274">Next</span></span>](adding-controller.md)

::: moniker-end
