---
title: 'Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama'
author: rick-anderson
description: Bu öğretici serisinde, ASP.NET Core Razor sayfaları kullanma işlemi gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturmak, veri erişimi için Entity Framework Core ve SQL Server kullanmak, arama işlevi eklemek, giriş doğrulaması eklemek ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 929bc72b16e302a5018038bc449704b7078dd33a
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425087"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="79deb-104">Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="79deb-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="79deb-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="79deb-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="79deb-106">Bu, bir serinin ilk öğreticidir.</span><span class="sxs-lookup"><span data-stu-id="79deb-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="79deb-107">[Serinin](xref:tutorials/razor-pages/index) bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="79deb-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="79deb-108">Serinin sonunda bir film veritabanı yöneten bir uygulaması oluşturmuş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="79deb-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="79deb-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="79deb-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79deb-110">Razor sayfaları web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79deb-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="79deb-111">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="79deb-111">Run the app.</span></span>
> * <span data-ttu-id="79deb-112">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="79deb-112">Examine the project files.</span></span>

<span data-ttu-id="79deb-113">Bu öğreticinin sonunda çalışan bir sonraki öğreticilerde oluşturacağınız Razor sayfaları web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="79deb-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

[<span data-ttu-id="79deb-114">Giriş ya da dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="79deb-114">Home or Index page</span></span>](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="79deb-115">Razor sayfaları web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="79deb-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79deb-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79deb-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79deb-117">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="79deb-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="79deb-118">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79deb-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="79deb-119">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="79deb-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="79deb-120">Projeyi adlandırın önemlidir *RazorPagesMovie* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="79deb-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="79deb-122">Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="79deb-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="79deb-124">Aşağıdaki başlangıç projesini oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="79deb-124">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79deb-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79deb-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79deb-127">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="79deb-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="79deb-128">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="79deb-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="79deb-129">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="79deb-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="79deb-130">`dotnet new` Komut yeni bir Razor sayfaları projesindeki oluşturur *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="79deb-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="79deb-131">`code` Komutu açılır *RazorPagesMovie* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="79deb-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="79deb-132">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'RazorPagesMovie' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="79deb-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="79deb-133">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="79deb-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79deb-134">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79deb-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79deb-135">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="79deb-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="79deb-136">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) Razor sayfaları projesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="79deb-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="79deb-137">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="79deb-137">Open the project</span></span>

<span data-ttu-id="79deb-138">Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="79deb-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-web-app"></a><span data-ttu-id="79deb-139">Web uygulamasını çalıştırma</span><span class="sxs-lookup"><span data-stu-id="79deb-139">Run the web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79deb-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79deb-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79deb-141">Hata Ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="79deb-141">Press Ctrl+F5 to run without the debugger.</span></span>

  <span data-ttu-id="79deb-142">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="79deb-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="79deb-143">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="79deb-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="79deb-144">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="79deb-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="79deb-145">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="79deb-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="79deb-146">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="79deb-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="79deb-147">Yukarıdaki görüntüde, 5001 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="79deb-147">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="79deb-148">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="79deb-148">When you run the app, you'll see a different port number.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79deb-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79deb-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79deb-150">Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="79deb-150">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="79deb-151">Visual Studio Code başlatıldığında başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="79deb-151">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="79deb-152">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="79deb-152">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="79deb-153">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="79deb-153">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="79deb-154">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="79deb-154">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79deb-155">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79deb-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79deb-156">Seçin **çalıştırın > hata ayıklama olmadan Başlat** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="79deb-156">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="79deb-157">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="79deb-157">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="79deb-158">Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="79deb-158">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="79deb-159">Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="79deb-159">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="79deb-161">İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="79deb-161">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="79deb-163">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="79deb-163">Examine the project files</span></span>

<span data-ttu-id="79deb-164">Sonraki öğreticilerde ile çalışacaksınız dosyaları ve klasörleri ana proje genel bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="79deb-164">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="79deb-165">Sayfaları klasörü</span><span class="sxs-lookup"><span data-stu-id="79deb-165">Pages folder</span></span>

<span data-ttu-id="79deb-166">Razor sayfaları ve Destek dosyalarını içerir.</span><span class="sxs-lookup"><span data-stu-id="79deb-166">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="79deb-167">Her bir Razor sayfası dosyalarının bir çiftini şöyledir:</span><span class="sxs-lookup"><span data-stu-id="79deb-167">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="79deb-168">A *.cshtml* ile HTML biçimlendirmesini içeren dosya C# Razor söz dizimini kullanarak kodu.</span><span class="sxs-lookup"><span data-stu-id="79deb-168">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="79deb-169">A *. cshtml.cs* içeren dosya C# sayfası olayları işleyen kodu.</span><span class="sxs-lookup"><span data-stu-id="79deb-169">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="79deb-170">Destekleyici dosyaları bir alt çizgi ile başlayan adları vardır.</span><span class="sxs-lookup"><span data-stu-id="79deb-170">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="79deb-171">Örneğin, *_Layout.cshtml* dosyası tüm sayfalar için ortak kullanıcı Arabirimi öğeleri yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="79deb-171">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="79deb-172">Bu dosya sayfanın üst gezinti menüsünde ve sayfanın alt kısmındaki telif hakkı bildirimi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="79deb-172">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="79deb-173">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="79deb-173">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="79deb-174">wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="79deb-174">wwwroot folder</span></span>

<span data-ttu-id="79deb-175">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="79deb-175">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="79deb-176">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="79deb-176">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="79deb-177">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="79deb-177">appSettings.json</span></span>

<span data-ttu-id="79deb-178">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="79deb-178">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="79deb-179">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="79deb-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="79deb-180">Program.cs</span><span class="sxs-lookup"><span data-stu-id="79deb-180">Program.cs</span></span>

<span data-ttu-id="79deb-181">Programın giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="79deb-181">Contains the entry point for the program.</span></span> <span data-ttu-id="79deb-182">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="79deb-182">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="79deb-183">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="79deb-183">Startup.cs</span></span>

<span data-ttu-id="79deb-184">Olup olmadığı için tanımlama bilgileri onayı gerektirir gibi uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="79deb-184">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="79deb-185">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="79deb-185">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79deb-186">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="79deb-186">Next steps</span></span>

<span data-ttu-id="79deb-187">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="79deb-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79deb-188">Razor sayfaları web uygulaması oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="79deb-188">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="79deb-189">Bir uygulamayı çalıştırdınız.</span><span class="sxs-lookup"><span data-stu-id="79deb-189">Ran the app.</span></span>
> * <span data-ttu-id="79deb-190">Proje dosyalarını incelenir.</span><span class="sxs-lookup"><span data-stu-id="79deb-190">Examined the project files.</span></span>

<span data-ttu-id="79deb-191">Serinin sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="79deb-191">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="79deb-192">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="79deb-192">Add a model</span></span>](xref:tutorials/razor-pages/model)
