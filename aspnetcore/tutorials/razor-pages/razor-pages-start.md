---
title: 'Öğretici: ASP.NET Core Razor Pages kullanmaya başlama'
author: rick-anderson
description: Bu öğretici dizisinde Razor Pages ASP.NET Core nasıl kullanılacağı gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturma, veri erişimi için Entity Framework Core ve SQL Server kullanma, arama işlevselliği ekleme, giriş doğrulaması ekleme ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1605197188d97f27a884739a72400da2d5818b1a
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371968"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="37cb1-104">Öğretici: ASP.NET Core Razor Pages kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="37cb1-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="37cb1-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="37cb1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="37cb1-106">Bu, bir serinin ASP.NET Core Razor Pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="37cb1-107">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="37cb1-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="37cb1-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37cb1-109">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="37cb1-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="37cb1-110">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-110">Run the app.</span></span>
> * <span data-ttu-id="37cb1-111">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-111">Examine the project files.</span></span>

<span data-ttu-id="37cb1-112">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="37cb1-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="37cb1-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="37cb1-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37cb1-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37cb1-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37cb1-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="37cb1-117">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="37cb1-118">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="37cb1-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37cb1-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="37cb1-120">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="37cb1-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="37cb1-121">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="37cb1-122">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="37cb1-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="37cb1-123">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="37cb1-124">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="37cb1-125">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="37cb1-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="37cb1-126">Açılan **Web uygulamasındaki** **ASP.NET Core 3,0** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="37cb1-128">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="37cb1-128">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37cb1-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37cb1-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="37cb1-131">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="37cb1-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="37cb1-132">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="37cb1-133">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="37cb1-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="37cb1-134">Komut RazorPagesMovie klasöründe yeni bir Razor Pages projesi oluşturur.  `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="37cb1-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="37cb1-135">Komut, Visual Studio Code geçerli örneğindeki RazorPagesMovie klasörünü açar.  `code`</span><span class="sxs-lookup"><span data-stu-id="37cb1-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="37cb1-136">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, gerekli varlıkların derleme **ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="37cb1-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="37cb1-137">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-137">Select **Yes**.</span></span>

  <span data-ttu-id="37cb1-138">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="37cb1-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="37cb1-140">Terminalden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="37cb1-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="37cb1-141">Yukarıdaki komutlar, bir Razor Pages projesi oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="37cb1-142">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="37cb1-142">Open the project</span></span>

<span data-ttu-id="37cb1-143">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="37cb1-144">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="37cb1-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37cb1-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="37cb1-146">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="37cb1-147">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="37cb1-148">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="37cb1-149">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="37cb1-150">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="37cb1-151">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37cb1-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37cb1-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="37cb1-153">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="37cb1-154">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="37cb1-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="37cb1-155">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="37cb1-156">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="37cb1-157">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="37cb1-158">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="37cb1-159">Hata ayıklayıcı olmadan çalıştırmak için **cmd-opt-F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-159">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="37cb1-160">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="37cb1-160">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="37cb1-161">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="37cb1-161">Examine the project files</span></span>

<span data-ttu-id="37cb1-162">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="37cb1-163">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="37cb1-163">Pages folder</span></span>

<span data-ttu-id="37cb1-164">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="37cb1-165">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="37cb1-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="37cb1-166">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="37cb1-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="37cb1-167">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="37cb1-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="37cb1-168">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="37cb1-169">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="37cb1-170">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="37cb1-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="37cb1-171">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-171">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="37cb1-172">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="37cb1-172">wwwroot folder</span></span>

<span data-ttu-id="37cb1-173">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="37cb1-174">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="37cb1-175">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="37cb1-175">appSettings.json</span></span>

<span data-ttu-id="37cb1-176">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="37cb1-177">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="37cb1-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="37cb1-178">Program.cs</span></span>

<span data-ttu-id="37cb1-179">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-179">Contains the entry point for the program.</span></span> <span data-ttu-id="37cb1-180">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-180">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="37cb1-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="37cb1-181">Startup.cs</span></span>

<span data-ttu-id="37cb1-182">Uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-182">Contains code that configures app behavior.</span></span> <span data-ttu-id="37cb1-183">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37cb1-184">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="37cb1-184">Next steps</span></span>

<span data-ttu-id="37cb1-185">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="37cb1-185">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="37cb1-186">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="37cb1-186">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="37cb1-187">Bu, bir serinin ilk öğreticisidir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-187">This is the first tutorial of a series.</span></span> <span data-ttu-id="37cb1-188">[Seriler](xref:tutorials/razor-pages/index) , bir ASP.NET Core Razor pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-188">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="37cb1-189">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-189">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="37cb1-190">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="37cb1-190">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37cb1-191">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="37cb1-191">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="37cb1-192">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-192">Run the app.</span></span>
> * <span data-ttu-id="37cb1-193">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-193">Examine the project files.</span></span>

<span data-ttu-id="37cb1-194">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="37cb1-194">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="37cb1-196">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="37cb1-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37cb1-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37cb1-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37cb1-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="37cb1-199">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="37cb1-200">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="37cb1-200">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37cb1-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="37cb1-202">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="37cb1-202">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="37cb1-203">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-203">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="37cb1-205">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-205">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="37cb1-206">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-206">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/config.png)

* <span data-ttu-id="37cb1-208">Açılan **Web uygulamasındaki** **ASP.NET Core 2,2** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-208">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="37cb1-210">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="37cb1-210">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37cb1-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37cb1-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="37cb1-213">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="37cb1-213">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="37cb1-214">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-214">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="37cb1-215">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="37cb1-215">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="37cb1-216">Komut RazorPagesMovie klasöründe yeni bir Razor Pages projesi oluşturur.  `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="37cb1-216">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="37cb1-217">Komut, Visual Studio Code geçerli örneğindeki RazorPagesMovie klasörünü açar.  `code`</span><span class="sxs-lookup"><span data-stu-id="37cb1-217">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="37cb1-218">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, gerekli varlıkların derleme **ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="37cb1-218">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="37cb1-219">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-219">Select **Yes**.</span></span>

  <span data-ttu-id="37cb1-220">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-220">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="37cb1-221">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-221">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="37cb1-222">Terminalden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="37cb1-222">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="37cb1-223">Yukarıdaki komutlar, bir Razor Pages projesi oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-223">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="37cb1-224">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="37cb1-224">Open the project</span></span>

<span data-ttu-id="37cb1-225">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-225">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="37cb1-226">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="37cb1-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37cb1-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-227">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="37cb1-228">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-228">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="37cb1-229">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="37cb1-230">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-230">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="37cb1-231">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-231">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="37cb1-232">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-232">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="37cb1-233">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="37cb1-234">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-234">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="37cb1-235">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-235">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="37cb1-237">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="37cb1-237">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37cb1-239">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37cb1-239">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="37cb1-240">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-240">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="37cb1-241">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="37cb1-241">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="37cb1-242">Adres çubuğu gibi `example.com`bir `localhost:port#` şey gösterir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-242">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="37cb1-243">Bunun nedeni `localhost` , yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-243">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="37cb1-244">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-244">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="37cb1-245">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="37cb1-246">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="37cb1-248">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="37cb1-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="37cb1-250">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37cb1-250">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="37cb1-251">Hata ayıklayıcı olmadan çalıştırmak için **cmd-opt-F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="37cb1-251">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="37cb1-252">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve şuraya gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="37cb1-252">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="37cb1-253">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="37cb1-253">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="37cb1-254">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-254">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="37cb1-256">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="37cb1-256">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="37cb1-258">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="37cb1-258">Examine the project files</span></span>

<span data-ttu-id="37cb1-259">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-259">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="37cb1-260">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="37cb1-260">Pages folder</span></span>

<span data-ttu-id="37cb1-261">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-261">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="37cb1-262">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="37cb1-262">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="37cb1-263">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="37cb1-263">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="37cb1-264">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="37cb1-264">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="37cb1-265">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-265">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="37cb1-266">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="37cb1-266">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="37cb1-267">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="37cb1-267">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="37cb1-268">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-268">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="37cb1-269">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="37cb1-269">wwwroot folder</span></span>

<span data-ttu-id="37cb1-270">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-270">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="37cb1-271">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-271">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="37cb1-272">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="37cb1-272">appSettings.json</span></span>

<span data-ttu-id="37cb1-273">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-273">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="37cb1-274">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-274">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="37cb1-275">Program.cs</span><span class="sxs-lookup"><span data-stu-id="37cb1-275">Program.cs</span></span>

<span data-ttu-id="37cb1-276">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-276">Contains the entry point for the program.</span></span> <span data-ttu-id="37cb1-277">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-277">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="37cb1-278">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="37cb1-278">Startup.cs</span></span>

<span data-ttu-id="37cb1-279">Tanımlama bilgilerinin onayını gerektirip gerektirmediğini belirten uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="37cb1-279">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="37cb1-280">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="37cb1-280">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37cb1-281">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="37cb1-281">Additional resources</span></span>

* [<span data-ttu-id="37cb1-282">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="37cb1-282">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="37cb1-283">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="37cb1-283">Next steps</span></span>

<span data-ttu-id="37cb1-284">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="37cb1-284">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="37cb1-285">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="37cb1-285">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end