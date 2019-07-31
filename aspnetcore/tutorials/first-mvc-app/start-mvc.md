---
title: ASP.NET Core MVC ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: b3544769b0e40fc27f5b6c939c6d7b5f9082f854
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682805"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="b04f4-103">ASP.NET Core MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b04f4-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="b04f4-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b04f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="b04f4-105">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="b04f4-106">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="b04f4-107">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="b04f4-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b04f4-108">Bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b04f4-108">Create a web app.</span></span>
> * <span data-ttu-id="b04f4-109">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="b04f4-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="b04f4-110">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="b04f4-110">Work with a database.</span></span>
> * <span data-ttu-id="b04f4-111">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-111">Add search and validation.</span></span>

<span data-ttu-id="b04f4-112">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="b04f4-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b04f4-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b04f4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b04f4-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b04f4-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b04f4-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="b04f4-117">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="b04f4-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b04f4-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b04f4-119">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="b04f4-120">**ASP.NET Core Web uygulamasını** seçip **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-120">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="b04f4-122">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="b04f4-123">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)

* <span data-ttu-id="b04f4-125">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="b04f4-126">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="b04f4-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="b04f4-127">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="b04f4-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="b04f4-128">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="b04f4-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="b04f4-129">Bu, temel bir başlatıcı projem.</span><span class="sxs-lookup"><span data-stu-id="b04f4-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b04f4-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b04f4-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b04f4-131">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="b04f4-132">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="b04f4-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="b04f4-133">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b04f4-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b04f4-134">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b04f4-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b04f4-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="b04f4-136">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="b04f4-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="b04f4-137">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="b04f4-137">Select **Yes**</span></span>

  * <span data-ttu-id="b04f4-138">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b04f4-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="b04f4-139">`code -r MvcMovie`: Visual Studio Code ' de *Mvcmovie. csproj* proje dosyasını yükler.</span><span class="sxs-lookup"><span data-stu-id="b04f4-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b04f4-140">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b04f4-141">**Dosya** > **yeni çözüm**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-141">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="b04f4-143">**Daha sonra** **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** > öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="b04f4-145">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 3,0**'nin **hedef çerçevesini** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b04f4-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="b04f4-146">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="b04f4-147">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b04f4-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b04f4-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b04f4-149">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="b04f4-150">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="b04f4-151">Adres çubuğunda `localhost:port#` , gibi `example.com`bir şey gösterilmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b04f4-152">Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="b04f4-153">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="b04f4-154">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b04f4-155">Birçok geliştirici, uygulamayı hızlı bir şekilde başlatmak ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="b04f4-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="b04f4-156">Hata **ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b04f4-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="b04f4-158">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="b04f4-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)


  <span data-ttu-id="b04f4-160">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b04f4-160">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b04f4-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b04f4-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b04f4-163">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="b04f4-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="b04f4-164">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b04f4-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="b04f4-165">Adres çubuğu gibi `example.com`bir `localhost:port:5001` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="b04f4-166">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="b04f4-167">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="b04f4-168">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b04f4-169">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="b04f4-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="b04f4-170">İzlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-170">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="b04f4-171">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="b04f4-171">This app doesn't track personal information.</span></span> <span data-ttu-id="b04f4-172">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-172">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="b04f4-174">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b04f4-174">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b04f4-176">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b04f4-177">Uygulamayı başlatmak için**hata ayıklama olmadan Başlat** ' **ı seçin.**  > </span><span class="sxs-lookup"><span data-stu-id="b04f4-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="b04f4-178">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="b04f4-179">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b04f4-180">Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="b04f4-181">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b04f4-182">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b04f4-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="b04f4-183">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b04f4-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="b04f4-184">Aşağıdaki görüntüde uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b04f4-184">The following image shows the app:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="b04f4-186">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b04f4-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b04f4-187">Next</span><span class="sxs-lookup"><span data-stu-id="b04f4-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="b04f4-188">Bu öğretici, ASP.NET Core MVC web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="b04f4-189">Uygulama, bir film başlıkları veritabanını yönetir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="b04f4-190">Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="b04f4-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b04f4-191">Bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b04f4-191">Create a web app.</span></span>
> * <span data-ttu-id="b04f4-192">Bir modeli ekleyin ve yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="b04f4-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="b04f4-193">Bir veritabanıyla çalışın.</span><span class="sxs-lookup"><span data-stu-id="b04f4-193">Work with a database.</span></span>
> * <span data-ttu-id="b04f4-194">Arama ve doğrulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-194">Add search and validation.</span></span>

<span data-ttu-id="b04f4-195">Sonunda, film verilerini yönetebilen ve görüntüleyebilen bir uygulamanız vardır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="b04f4-196">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b04f4-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b04f4-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b04f4-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b04f4-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b04f4-199">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="b04f4-200">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="b04f4-200">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b04f4-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b04f4-202">Visual Studio 'da **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="b04f4-203">**ASP.NET Core Web uygulamasını** seçip **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-203">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="b04f4-205">Projeyi **Mvcfilmi** olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="b04f4-206">Kodu kopyaladığınızda, ad alanının eşleşmesi için, projeyi **Mvcfilmi** olarak adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](start-mvc/_static/config.png)


* <span data-ttu-id="b04f4-208">**Web uygulaması (Model-View-Controller)** öğesini seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="b04f4-209">Yeni proje iletişim kutusu, sol bölmede .NET Core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="b04f4-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="b04f4-210">Visual Studio, az önce oluşturduğunuz MVC projesi için varsayılan şablonu kullandı.</span><span class="sxs-lookup"><span data-stu-id="b04f4-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="b04f4-211">Şimdi bir proje adı girip birkaç seçenek belirleyerek, çalışan bir uygulamanız var.</span><span class="sxs-lookup"><span data-stu-id="b04f4-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="b04f4-212">Bu, temel bir başlatıcı projem ve başlamak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b04f4-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b04f4-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b04f4-214">Öğreticide familar, VS Code ile varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="b04f4-215">Daha fazla bilgi için bkz. [vs Code kullanmaya](https://code.visualstudio.com/docs) başlama ve [Yardım Visual Studio Code](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="b04f4-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="b04f4-216">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b04f4-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b04f4-217">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b04f4-218">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b04f4-218">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="b04f4-219">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' mvcfilmi ' içinde eksik olan bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="b04f4-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="b04f4-220">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="b04f4-220">Select **Yes**</span></span>

  * <span data-ttu-id="b04f4-221">`dotnet new mvc -o MvcMovie`: *Mvcmovie* klasöründe yeni BIR ASP.NET Core MVC projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b04f4-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="b04f4-222">`code -r MvcMovie`: Visual Studio Code ' de *Mvcmovie. csproj* proje dosyasını yükler.</span><span class="sxs-lookup"><span data-stu-id="b04f4-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b04f4-223">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b04f4-224">**Dosya** > **yeni çözüm**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-224">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="b04f4-226">**Daha sonra** **.NET Core** > **App** > **Web uygulaması (Model-View-Controller)** > öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-226">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="b04f4-228">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 2,2**'nin varsayılan **hedef çerçevesini** kabul edin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-228">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2,2 seçimi](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="b04f4-230">Projeyi **Mvcfilmi**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-230">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="b04f4-231">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b04f4-231">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b04f4-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b04f4-233">Uygulamayı hata ayıklamasız modda çalıştırmak için **CTRL-F5** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-233">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="b04f4-234">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-234">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="b04f4-235">Adres çubuğunda `localhost:port#` , gibi `example.com`bir şey gösterilmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-235">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b04f4-236">Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-236">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="b04f4-237">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-237">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="b04f4-238">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-238">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b04f4-239">Birçok geliştirici, uygulamayı hızlı bir şekilde başlatmak ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="b04f4-239">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="b04f4-240">Hata **ayıklama menü öğesinden** uygulamayı hata ayıklama veya hata ayıklama olmayan modda başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b04f4-240">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Hata ayıklama menüsü](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="b04f4-242">**IIS Express** düğmesini seçerek uygulamada hata ayıklaması yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="b04f4-242">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="b04f4-244">İzlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-244">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="b04f4-245">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="b04f4-245">This app doesn't track personal information.</span></span> <span data-ttu-id="b04f4-246">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-246">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="b04f4-248">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b04f4-248">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b04f4-250">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b04f4-250">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b04f4-251">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="b04f4-251">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="b04f4-252">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b04f4-252">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="b04f4-253">Adres çubuğu gibi `example.com`bir `localhost:port:5001` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-253">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="b04f4-254">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-254">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="b04f4-255">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-255">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="b04f4-256">Uygulamayı CTRL + F5 (hata ayıklama modu) ile başlatmak, kod değişiklikleri yapmanıza, dosyayı kaydetmenize, tarayıcıyı yenilemanıza ve kod değişikliklerini görmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-256">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b04f4-257">Birçok geliştirici, sayfayı yenilemek ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="b04f4-257">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="b04f4-258">İzlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-258">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="b04f4-259">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="b04f4-259">This app doesn't track personal information.</span></span> <span data-ttu-id="b04f4-260">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-260">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/privacy.png)

  <span data-ttu-id="b04f4-262">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b04f4-262">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b04f4-264">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b04f4-264">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b04f4-265">Uygulamayı başlatmak için**hata ayıklama olmadan Başlat** ' **ı seçin.**  > </span><span class="sxs-lookup"><span data-stu-id="b04f4-265">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="b04f4-266">Mac için Visual Studio, [Kestrel](xref:fundamentals/servers/index#kestrel) Server 'ı başlatır, bir tarayıcı başlatır ve `http://localhost:port`' a gider ve *bağlantı noktası* rastgele seçilmiş bir bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-266">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="b04f4-267">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-267">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b04f4-268">Bunun nedeni `localhost` , Yerel bilgisayarınız için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-268">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="b04f4-269">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b04f4-269">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b04f4-270">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası numarası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b04f4-270">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="b04f4-271">Uygulamayı **Çalıştır** menüsünden Hata Ayıkla veya hata ayıklama olmayan modda başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b04f4-271">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="b04f4-272">İzlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b04f4-272">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="b04f4-273">Bu uygulama kişisel bilgileri izlemez.</span><span class="sxs-lookup"><span data-stu-id="b04f4-273">This app doesn't track personal information.</span></span> <span data-ttu-id="b04f4-274">Şablon tarafından oluşturulan kod, [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)buluşmanıza yardımcı olan varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="b04f4-274">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="b04f4-276">Aşağıdaki görüntüde izlemeyi kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b04f4-276">The following image shows the app after accepting tracking:</span></span>

  ![Giriş veya dizin sayfası](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="b04f4-278">Bu öğreticinin bir sonraki bölümünde, MVC hakkında bilgi edinirsiniz ve kod yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b04f4-278">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b04f4-279">Next</span><span class="sxs-lookup"><span data-stu-id="b04f4-279">Next</span></span>](adding-controller.md)

::: moniker-end
