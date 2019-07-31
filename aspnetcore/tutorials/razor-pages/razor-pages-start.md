---
title: 'Öğretici: ASP.NET Core Razor Pages kullanmaya başlama'
author: rick-anderson
description: Bu öğretici dizisinde Razor Pages ASP.NET Core nasıl kullanılacağı gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturma, veri erişimi için Entity Framework Core ve SQL Server kullanma, arama işlevselliği ekleme, giriş doğrulaması ekleme ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 57a10895c718c539ece280afcb27cb4033c7fb45
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682801"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="86a9e-104">Öğretici: ASP.NET Core Razor Pages kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="86a9e-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="86a9e-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86a9e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="86a9e-106">Bu, bir serinin ASP.NET Core Razor Pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="86a9e-107">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="86a9e-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="86a9e-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86a9e-109">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86a9e-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="86a9e-110">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-110">Run the app.</span></span>
> * <span data-ttu-id="86a9e-111">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-111">Examine the project files.</span></span>

<span data-ttu-id="86a9e-112">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="86a9e-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="86a9e-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="86a9e-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86a9e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86a9e-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86a9e-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86a9e-117">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="86a9e-118">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="86a9e-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86a9e-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="86a9e-120">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="86a9e-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="86a9e-121">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="86a9e-122">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="86a9e-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="86a9e-123">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="86a9e-124">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="86a9e-125">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="86a9e-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="86a9e-126">Açılan **Web uygulamasındaki** **ASP.NET Core 3,0** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="86a9e-128">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="86a9e-128">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86a9e-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86a9e-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="86a9e-131">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="86a9e-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="86a9e-132">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="86a9e-133">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86a9e-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="86a9e-134">Komut RazorPagesMovie klasöründe yeni bir Razor Pages projesi oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="86a9e-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="86a9e-135">Komut, Visual Studio Code geçerli örneğindeki RazorPagesMovie klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="86a9e-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="86a9e-136">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, gerekli varlıkların derleme **ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="86a9e-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="86a9e-137">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-137">Select **Yes**.</span></span>

  <span data-ttu-id="86a9e-138">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86a9e-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="86a9e-140">Terminalden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86a9e-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="86a9e-141">Yukarıdaki komutlar, bir Razor Pages projesi oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="86a9e-142">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="86a9e-142">Open the project</span></span>

<span data-ttu-id="86a9e-143">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="86a9e-144">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="86a9e-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86a9e-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="86a9e-146">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="86a9e-147">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="86a9e-148">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="86a9e-149">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="86a9e-150">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="86a9e-151">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86a9e-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86a9e-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="86a9e-153">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="86a9e-154">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="86a9e-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="86a9e-155">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="86a9e-156">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="86a9e-157">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86a9e-158">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="86a9e-159">Hata ayıklayıcı olmadan çalıştırmak için **alt-cmd-ENTER** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-159">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="86a9e-160">Alternatif olarak, menü çubuğuna gidin ve hata ayıklama olmadan Başlat > Çalıştır ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-160">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="86a9e-161">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="86a9e-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="86a9e-162">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="86a9e-162">Examine the project files</span></span>

<span data-ttu-id="86a9e-163">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-163">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="86a9e-164">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="86a9e-164">Pages folder</span></span>

<span data-ttu-id="86a9e-165">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-165">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="86a9e-166">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="86a9e-166">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="86a9e-167">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="86a9e-167">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="86a9e-168">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="86a9e-168">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="86a9e-169">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-169">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="86a9e-170">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-170">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="86a9e-171">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="86a9e-171">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="86a9e-172">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-172">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="86a9e-173">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="86a9e-173">wwwroot folder</span></span>

<span data-ttu-id="86a9e-174">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-174">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="86a9e-175">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-175">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="86a9e-176">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="86a9e-176">appSettings.json</span></span>

<span data-ttu-id="86a9e-177">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-177">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="86a9e-178">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-178">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="86a9e-179">Program.cs</span><span class="sxs-lookup"><span data-stu-id="86a9e-179">Program.cs</span></span>

<span data-ttu-id="86a9e-180">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-180">Contains the entry point for the program.</span></span> <span data-ttu-id="86a9e-181">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-181">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="86a9e-182">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="86a9e-182">Startup.cs</span></span>

<span data-ttu-id="86a9e-183">Uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-183">Contains code that configures app behavior.</span></span> <span data-ttu-id="86a9e-184">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-184">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86a9e-185">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="86a9e-185">Next steps</span></span>

<span data-ttu-id="86a9e-186">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="86a9e-186">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="86a9e-187">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="86a9e-187">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="86a9e-188">Bu, bir serinin ilk öğreticisidir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-188">This is the first tutorial of a series.</span></span> <span data-ttu-id="86a9e-189">[Seriler](xref:tutorials/razor-pages/index) , bir ASP.NET Core Razor pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-189">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="86a9e-190">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-190">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="86a9e-191">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="86a9e-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86a9e-192">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86a9e-192">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="86a9e-193">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-193">Run the app.</span></span>
> * <span data-ttu-id="86a9e-194">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-194">Examine the project files.</span></span>

<span data-ttu-id="86a9e-195">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="86a9e-195">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="86a9e-197">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="86a9e-197">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86a9e-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86a9e-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86a9e-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86a9e-200">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="86a9e-201">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="86a9e-201">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86a9e-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="86a9e-203">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="86a9e-203">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="86a9e-204">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-204">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="86a9e-206">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-206">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="86a9e-207">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-207">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/config.png)

* <span data-ttu-id="86a9e-209">Açılan **Web uygulamasındaki** **ASP.NET Core 2,2** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-209">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="86a9e-211">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="86a9e-211">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86a9e-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86a9e-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="86a9e-214">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="86a9e-214">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="86a9e-215">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-215">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="86a9e-216">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86a9e-216">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="86a9e-217">Komut RazorPagesMovie klasöründe yeni bir Razor Pages projesi oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="86a9e-217">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="86a9e-218">Komut, Visual Studio Code geçerli örneğindeki RazorPagesMovie klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="86a9e-218">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="86a9e-219">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, gerekli varlıkların derleme **ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="86a9e-219">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="86a9e-220">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-220">Select **Yes**.</span></span>

  <span data-ttu-id="86a9e-221">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-221">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86a9e-222">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="86a9e-223">Terminalden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="86a9e-223">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="86a9e-224">Yukarıdaki komutlar, bir Razor Pages projesi oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-224">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="86a9e-225">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="86a9e-225">Open the project</span></span>

<span data-ttu-id="86a9e-226">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-226">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="86a9e-227">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="86a9e-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86a9e-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="86a9e-229">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-229">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="86a9e-230">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="86a9e-231">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-231">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="86a9e-232">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-232">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="86a9e-233">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-233">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="86a9e-234">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-234">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="86a9e-235">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="86a9e-236">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="86a9e-238">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="86a9e-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86a9e-240">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86a9e-240">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="86a9e-241">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-241">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="86a9e-242">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="86a9e-242">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="86a9e-243">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-243">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="86a9e-244">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-244">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="86a9e-245">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-245">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="86a9e-246">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-246">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="86a9e-247">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-247">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="86a9e-249">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="86a9e-249">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86a9e-251">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86a9e-251">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="86a9e-252">Hata ayıklayıcı olmadan çalıştırmak için **cmd-opt-F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="86a9e-252">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="86a9e-253">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="86a9e-253">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="86a9e-254">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="86a9e-254">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="86a9e-255">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-255">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="86a9e-257">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="86a9e-257">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="86a9e-259">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="86a9e-259">Examine the project files</span></span>

<span data-ttu-id="86a9e-260">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-260">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="86a9e-261">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="86a9e-261">Pages folder</span></span>

<span data-ttu-id="86a9e-262">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-262">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="86a9e-263">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="86a9e-263">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="86a9e-264">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="86a9e-264">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="86a9e-265">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="86a9e-265">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="86a9e-266">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-266">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="86a9e-267">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="86a9e-267">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="86a9e-268">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="86a9e-268">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="86a9e-269">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-269">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="86a9e-270">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="86a9e-270">wwwroot folder</span></span>

<span data-ttu-id="86a9e-271">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-271">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="86a9e-272">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-272">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="86a9e-273">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="86a9e-273">appSettings.json</span></span>

<span data-ttu-id="86a9e-274">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-274">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="86a9e-275">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-275">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="86a9e-276">Program.cs</span><span class="sxs-lookup"><span data-stu-id="86a9e-276">Program.cs</span></span>

<span data-ttu-id="86a9e-277">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-277">Contains the entry point for the program.</span></span> <span data-ttu-id="86a9e-278">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-278">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="86a9e-279">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="86a9e-279">Startup.cs</span></span>

<span data-ttu-id="86a9e-280">Tanımlama bilgilerinin onayını gerektirip gerektirmediğini belirten uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="86a9e-280">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="86a9e-281">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="86a9e-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86a9e-282">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="86a9e-282">Additional resources</span></span>

* [<span data-ttu-id="86a9e-283">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="86a9e-283">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="86a9e-284">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="86a9e-284">Next steps</span></span>

<span data-ttu-id="86a9e-285">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="86a9e-285">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="86a9e-286">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="86a9e-286">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
