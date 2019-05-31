---
title: 'Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama'
author: rick-anderson
description: Bu öğretici serisinde, ASP.NET Core Razor sayfaları kullanma işlemi gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturmak, veri erişimi için Entity Framework Core ve SQL Server kullanmak, arama işlevi eklemek, giriş doğrulaması eklemek ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 05/30/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e9f11f68aa138ab74a0ffbbd0e32067bc984606d
ms.sourcegitcommit: 9ae1fd11f39b0a72b2ae42f0b450345e6e306bc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66415668"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="1742b-104">Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1742b-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="1742b-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1742b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1742b-106">Bu, bir serinin ilk öğreticidir.</span><span class="sxs-lookup"><span data-stu-id="1742b-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="1742b-107">[Serinin](xref:tutorials/razor-pages/index) bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="1742b-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="1742b-108">Serinin sonunda bir film veritabanı yöneten bir uygulaması oluşturmuş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="1742b-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="1742b-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="1742b-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1742b-110">Razor sayfaları web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1742b-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="1742b-111">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1742b-111">Run the app.</span></span>
> * <span data-ttu-id="1742b-112">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="1742b-112">Examine the project files.</span></span>

<span data-ttu-id="1742b-113">Bu öğreticinin sonunda, çalışan bir sonraki öğreticilerde oluşturacağınız Razor sayfaları web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1742b-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="1742b-115">Razor sayfaları web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="1742b-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1742b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1742b-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1742b-117">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="1742b-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="1742b-118">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1742b-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="1742b-119">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="1742b-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="1742b-120">Projeyi adlandırın önemlidir *RazorPagesMovie* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="1742b-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="1742b-122">Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1742b-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="1742b-124">Aşağıdaki başlangıç projesini oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1742b-124">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1742b-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1742b-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1742b-127">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1742b-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="1742b-128">Dizine değiştirin (`cd`) proje içerir.</span><span class="sxs-lookup"><span data-stu-id="1742b-128">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="1742b-129">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1742b-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="1742b-130">`dotnet new` Komut yeni bir Razor sayfaları projesindeki oluşturur *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="1742b-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="1742b-131">`code` Komutu açılır *RazorPagesMovie* klasöründe, Visual Studio Code geçerli örneği.</span><span class="sxs-lookup"><span data-stu-id="1742b-131">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="1742b-132">Durum çubuğunun OmniSharp sonra soran bir iletişim kutusu yangın simgesi yeşile **gerekli varlıkları oluşturun ve hata ayıklama 'RazorPagesMovie' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="1742b-132">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="1742b-133">Seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="1742b-133">Select **Yes**.</span></span>

  <span data-ttu-id="1742b-134">A *.vscode* dizin içeren *launch.json* ve *tasks.json* dosyaları, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="1742b-134">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1742b-135">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1742b-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1742b-136">Bir terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1742b-136">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="1742b-137">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) Razor sayfaları projesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="1742b-137">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="1742b-138">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="1742b-138">Open the project</span></span>

<span data-ttu-id="1742b-139">Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="1742b-139">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="1742b-140">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1742b-140">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1742b-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1742b-141">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1742b-142">Hata Ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="1742b-142">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="1742b-143">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1742b-143">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="1742b-144">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1742b-144">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1742b-145">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="1742b-145">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="1742b-146">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="1742b-146">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="1742b-147">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1742b-147">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="1742b-148">Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="1742b-148">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1742b-149">Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="1742b-149">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1742b-151">İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="1742b-151">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1742b-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1742b-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="1742b-154">Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="1742b-154">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1742b-155">Visual Studio Code başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1742b-155">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="1742b-156">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1742b-156">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1742b-157">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="1742b-157">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="1742b-158">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="1742b-158">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="1742b-159">Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="1742b-159">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1742b-160">Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="1742b-160">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1742b-162">İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="1742b-162">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1742b-164">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1742b-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="1742b-165">Tuşuna **Cmd iyileştirilmiş F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="1742b-165">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1742b-166">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1742b-166">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="1742b-167">Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="1742b-167">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1742b-168">Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="1742b-168">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="1742b-170">İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="1742b-170">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="1742b-172">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="1742b-172">Examine the project files</span></span>

<span data-ttu-id="1742b-173">Sonraki öğreticilerde ile çalışacaksınız dosyaları ve klasörleri ana proje genel bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1742b-173">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="1742b-174">Sayfaları klasörü</span><span class="sxs-lookup"><span data-stu-id="1742b-174">Pages folder</span></span>

<span data-ttu-id="1742b-175">Razor sayfaları ve Destek dosyalarını içerir.</span><span class="sxs-lookup"><span data-stu-id="1742b-175">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="1742b-176">Her bir Razor sayfası dosyalarının bir çiftini şöyledir:</span><span class="sxs-lookup"><span data-stu-id="1742b-176">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="1742b-177">A *.cshtml* ile HTML biçimlendirmesini içeren dosya C# Razor söz dizimini kullanarak kodu.</span><span class="sxs-lookup"><span data-stu-id="1742b-177">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="1742b-178">A *. cshtml.cs* içeren dosya C# sayfası olayları işleyen kodu.</span><span class="sxs-lookup"><span data-stu-id="1742b-178">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="1742b-179">Destekleyici dosyaları bir alt çizgi ile başlayan adları vardır.</span><span class="sxs-lookup"><span data-stu-id="1742b-179">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="1742b-180">Örneğin, *_Layout.cshtml* dosyası tüm sayfalar için ortak kullanıcı Arabirimi öğeleri yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="1742b-180">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="1742b-181">Bu dosya sayfanın üst gezinti menüsünde ve sayfanın alt kısmındaki telif hakkı bildirimi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1742b-181">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="1742b-182">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="1742b-182">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="1742b-183">wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="1742b-183">wwwroot folder</span></span>

<span data-ttu-id="1742b-184">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="1742b-184">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="1742b-185">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="1742b-185">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="1742b-186">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="1742b-186">appSettings.json</span></span>

<span data-ttu-id="1742b-187">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="1742b-187">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="1742b-188">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1742b-188">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="1742b-189">Program.cs</span><span class="sxs-lookup"><span data-stu-id="1742b-189">Program.cs</span></span>

<span data-ttu-id="1742b-190">Programın giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="1742b-190">Contains the entry point for the program.</span></span> <span data-ttu-id="1742b-191">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="1742b-191">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="1742b-192">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="1742b-192">Startup.cs</span></span>

<span data-ttu-id="1742b-193">Olup olmadığı için tanımlama bilgileri onayı gerektirir gibi uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="1742b-193">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="1742b-194">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1742b-194">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1742b-195">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1742b-195">Additional resources</span></span>

* [<span data-ttu-id="1742b-196">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="1742b-196">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="1742b-197">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="1742b-197">Next steps</span></span>

<span data-ttu-id="1742b-198">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="1742b-198">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1742b-199">Razor sayfaları web uygulaması oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="1742b-199">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="1742b-200">Bir uygulamayı çalıştırdınız.</span><span class="sxs-lookup"><span data-stu-id="1742b-200">Ran the app.</span></span>
> * <span data-ttu-id="1742b-201">Proje dosyalarını incelenir.</span><span class="sxs-lookup"><span data-stu-id="1742b-201">Examined the project files.</span></span>

<span data-ttu-id="1742b-202">Serinin sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="1742b-202">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1742b-203">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="1742b-203">Add a model</span></span>](xref:tutorials/razor-pages/model)
