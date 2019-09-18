---
title: 'Öğretici: ASP.NET Core Razor Pages kullanmaya başlama'
author: rick-anderson
description: Bu öğretici dizisinde Razor Pages ASP.NET Core nasıl kullanılacağı gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturma, veri erişimi için Entity Framework Core ve SQL Server kullanma, arama işlevselliği ekleme, giriş doğrulaması ekleme ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 0cc00cb85b6054752417b82c783cfd4c306aeda5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082571"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="ac44c-104">Öğretici: ASP.NET Core Razor Pages kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ac44c-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ac44c-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ac44c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="ac44c-106">Bu, bir serinin ASP.NET Core Razor Pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="ac44c-107">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="ac44c-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="ac44c-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ac44c-109">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac44c-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="ac44c-110">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-110">Run the app.</span></span>
> * <span data-ttu-id="ac44c-111">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-111">Examine the project files.</span></span>

<span data-ttu-id="ac44c-112">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac44c-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="ac44c-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ac44c-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac44c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac44c-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac44c-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac44c-117">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="ac44c-118">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ac44c-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac44c-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ac44c-120">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="ac44c-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ac44c-121">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="ac44c-122">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="ac44c-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="ac44c-123">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ac44c-124">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="ac44c-125">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="ac44c-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="ac44c-126">Açılan **Web uygulamasındaki** **ASP.NET Core 3,0** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="ac44c-128">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="ac44c-128">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac44c-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac44c-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ac44c-131">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ac44c-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="ac44c-132">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="ac44c-133">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac44c-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="ac44c-134">Komut RazorPagesMovie klasöründe yeni bir Razor Pages projesi oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="ac44c-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="ac44c-135">Komut, Visual Studio Code geçerli örneğindeki RazorPagesMovie klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="ac44c-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="ac44c-136">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, gerekli varlıkların derleme **ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="ac44c-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="ac44c-137">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-137">Select **Yes**.</span></span>

  <span data-ttu-id="ac44c-138">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac44c-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ac44c-140">**Dosya** > **yeni çözüm**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-140">Select **File** > **New Solution**.</span></span>

![Yeni çözüm macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ac44c-142">**Daha sonra** **.NET Core** > **App** > **Web** uygulaması> ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="ac44c-144">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** Iletişim kutusunda **hedef Framework 'ü** **.NET Core 3,0**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![macOS .NET Core 3,0 seçimi](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="ac44c-146">Projeyi **RazorPagesMovie**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="ac44c-148">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="ac44c-148">Open the project</span></span>

<span data-ttu-id="ac44c-149">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="ac44c-150">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ac44c-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac44c-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ac44c-152">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="ac44c-153">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ac44c-154">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ac44c-155">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="ac44c-156">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ac44c-157">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac44c-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac44c-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="ac44c-159">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="ac44c-160">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ac44c-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="ac44c-161">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ac44c-162">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ac44c-163">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac44c-164">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ac44c-165">Hata ayıklayıcı olmadan çalıştırmak için **alt-cmd-ENTER** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="ac44c-166">Alternatif olarak, menü çubuğuna gidin ve hata ayıklama olmadan Başlat > Çalıştır ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="ac44c-167">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ac44c-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="ac44c-168">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="ac44c-168">Examine the project files</span></span>

<span data-ttu-id="ac44c-169">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="ac44c-170">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="ac44c-170">Pages folder</span></span>

<span data-ttu-id="ac44c-171">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="ac44c-172">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="ac44c-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="ac44c-173">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="ac44c-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="ac44c-174">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="ac44c-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="ac44c-175">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="ac44c-176">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="ac44c-177">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ac44c-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="ac44c-178">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="ac44c-179">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="ac44c-179">wwwroot folder</span></span>

<span data-ttu-id="ac44c-180">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="ac44c-181">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="ac44c-182">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="ac44c-182">appSettings.json</span></span>

<span data-ttu-id="ac44c-183">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="ac44c-184">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="ac44c-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="ac44c-185">Program.cs</span></span>

<span data-ttu-id="ac44c-186">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-186">Contains the entry point for the program.</span></span> <span data-ttu-id="ac44c-187">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="ac44c-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ac44c-188">Startup.cs</span></span>

<span data-ttu-id="ac44c-189">Uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="ac44c-190">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac44c-191">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ac44c-191">Next steps</span></span>

<span data-ttu-id="ac44c-192">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="ac44c-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ac44c-193">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="ac44c-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac44c-194">Bu, bir serinin ilk öğreticisidir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="ac44c-195">[Seriler](xref:tutorials/razor-pages/index) , bir ASP.NET Core Razor pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="ac44c-196">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="ac44c-197">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="ac44c-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ac44c-198">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ac44c-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="ac44c-199">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-199">Run the app.</span></span>
> * <span data-ttu-id="ac44c-200">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-200">Examine the project files.</span></span>

<span data-ttu-id="ac44c-201">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac44c-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="ac44c-203">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ac44c-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac44c-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac44c-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac44c-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac44c-206">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="ac44c-207">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ac44c-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac44c-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ac44c-209">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="ac44c-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="ac44c-210">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="ac44c-212">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ac44c-213">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/config.png)

* <span data-ttu-id="ac44c-215">Açılan **Web uygulamasındaki** **ASP.NET Core 2,2** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="ac44c-217">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="ac44c-217">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac44c-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac44c-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ac44c-220">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ac44c-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="ac44c-221">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="ac44c-222">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac44c-222">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="ac44c-223">Komut RazorPagesMovie klasöründe yeni bir Razor Pages projesi oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="ac44c-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="ac44c-224">Komut, Visual Studio Code geçerli örneğindeki RazorPagesMovie klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="ac44c-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="ac44c-225">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, gerekli varlıkların derleme **ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="ac44c-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="ac44c-226">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-226">Select **Yes**.</span></span>

  <span data-ttu-id="ac44c-227">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac44c-228">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ac44c-229">Terminalden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ac44c-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="ac44c-230">Yukarıdaki komutlar, bir Razor Pages projesi oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="ac44c-231">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="ac44c-231">Open the project</span></span>

<span data-ttu-id="ac44c-232">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="ac44c-233">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ac44c-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ac44c-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ac44c-235">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="ac44c-236">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ac44c-237">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ac44c-238">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="ac44c-239">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ac44c-240">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="ac44c-241">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ac44c-242">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="ac44c-244">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ac44c-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ac44c-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac44c-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="ac44c-247">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="ac44c-248">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ac44c-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="ac44c-249">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ac44c-250">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ac44c-251">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="ac44c-252">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ac44c-253">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="ac44c-255">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ac44c-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ac44c-257">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac44c-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ac44c-258">Hata ayıklayıcı olmadan çalıştırmak için **cmd-opt-F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="ac44c-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="ac44c-259">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ac44c-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="ac44c-260">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ac44c-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ac44c-261">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="ac44c-263">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ac44c-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="ac44c-265">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="ac44c-265">Examine the project files</span></span>

<span data-ttu-id="ac44c-266">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="ac44c-267">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="ac44c-267">Pages folder</span></span>

<span data-ttu-id="ac44c-268">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="ac44c-269">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="ac44c-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="ac44c-270">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="ac44c-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="ac44c-271">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="ac44c-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="ac44c-272">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="ac44c-273">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ac44c-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="ac44c-274">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ac44c-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="ac44c-275">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="ac44c-276">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="ac44c-276">wwwroot folder</span></span>

<span data-ttu-id="ac44c-277">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="ac44c-278">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="ac44c-279">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="ac44c-279">appSettings.json</span></span>

<span data-ttu-id="ac44c-280">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="ac44c-281">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="ac44c-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="ac44c-282">Program.cs</span></span>

<span data-ttu-id="ac44c-283">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-283">Contains the entry point for the program.</span></span> <span data-ttu-id="ac44c-284">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="ac44c-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ac44c-285">Startup.cs</span></span>

<span data-ttu-id="ac44c-286">Tanımlama bilgilerinin onayını gerektirip gerektirmediğini belirten uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="ac44c-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="ac44c-287">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ac44c-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac44c-288">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ac44c-288">Additional resources</span></span>

* [<span data-ttu-id="ac44c-289">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="ac44c-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="ac44c-290">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ac44c-290">Next steps</span></span>

<span data-ttu-id="ac44c-291">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="ac44c-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ac44c-292">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="ac44c-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
