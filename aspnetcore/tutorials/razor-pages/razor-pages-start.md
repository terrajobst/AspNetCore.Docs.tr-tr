---
title: 'Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama'
author: rick-anderson
description: Bu öğretici serisinde, ASP.NET Core Razor sayfaları kullanma işlemi gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturmak, veri erişimi için Entity Framework Core ve SQL Server kullanmak, arama işlevi eklemek, giriş doğrulaması eklemek ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d264ca4a605d8291e273a8f054c92e7eefa5548
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903296"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="e0807-104">Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e0807-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e0807-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e0807-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e0807-106">Bu, bir serinin ilk öğreticidir.</span><span class="sxs-lookup"><span data-stu-id="e0807-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="e0807-107">[Serinin](xref:tutorials/razor-pages/index) bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="e0807-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="e0807-108">Serinin sonunda bir film veritabanı yöneten bir uygulaması oluşturmuş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="e0807-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="e0807-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="e0807-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e0807-110">Razor sayfaları web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e0807-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="e0807-111">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e0807-111">Run the app.</span></span>
> * <span data-ttu-id="e0807-112">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="e0807-112">Examine the project files.</span></span>

<span data-ttu-id="e0807-113">Bu öğreticinin sonunda çalışan bir sonraki öğreticilerde oluşturacağınız Razor sayfaları web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0807-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="e0807-115">Razor sayfaları web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e0807-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e0807-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0807-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e0807-117">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="e0807-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="e0807-118">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e0807-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e0807-119">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e0807-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e0807-120">Projeyi adlandırın önemlidir *RazorPagesMovie* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="e0807-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="e0807-122">Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="e0807-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="e0807-124">Aşağıdaki başlangıç projesini oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="e0807-124">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e0807-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e0807-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e0807-127">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e0807-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="e0807-128">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="e0807-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="e0807-129">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e0807-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="e0807-130">`dotnet new` Komut yeni bir Razor sayfaları projesindeki oluşturur *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="e0807-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="e0807-131">`code` Komutu açılır *RazorPagesMovie* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="e0807-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="e0807-132">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'RazorPagesMovie' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="e0807-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="e0807-133">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="e0807-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e0807-134">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0807-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e0807-135">Bir terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e0807-135">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="e0807-136">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) Razor sayfaları projesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e0807-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="e0807-137">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="e0807-137">Open the project</span></span>

<span data-ttu-id="e0807-138">Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="e0807-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="e0807-139">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e0807-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e0807-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0807-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e0807-141">Hata Ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e0807-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="e0807-142">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="e0807-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e0807-143">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e0807-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e0807-144">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="e0807-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="e0807-145">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="e0807-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e0807-146">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0807-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="e0807-147">Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="e0807-147">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="e0807-148">Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e0807-148">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="e0807-150">İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="e0807-150">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e0807-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e0807-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="e0807-153">Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="e0807-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="e0807-154">Visual Studio Code başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e0807-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="e0807-155">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e0807-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e0807-156">Çünkü `localhost` standart yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="e0807-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="e0807-157">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="e0807-157">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="e0807-158">Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="e0807-158">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="e0807-159">Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e0807-159">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="e0807-161">İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="e0807-161">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e0807-163">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0807-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="e0807-164">Tuşuna **Cmd iyileştirilmiş F5** hata ayıklayıcı olmadan çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="e0807-164">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="e0807-165">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e0807-165">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="e0807-166">Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="e0807-166">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="e0807-167">Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e0807-167">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="e0807-169">İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="e0807-169">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="e0807-171">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="e0807-171">Examine the project files</span></span>

<span data-ttu-id="e0807-172">Sonraki öğreticilerde ile çalışacaksınız dosyaları ve klasörleri ana proje genel bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e0807-172">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="e0807-173">Sayfaları klasörü</span><span class="sxs-lookup"><span data-stu-id="e0807-173">Pages folder</span></span>

<span data-ttu-id="e0807-174">Razor sayfaları ve Destek dosyalarını içerir.</span><span class="sxs-lookup"><span data-stu-id="e0807-174">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="e0807-175">Her bir Razor sayfası dosyalarının bir çiftini şöyledir:</span><span class="sxs-lookup"><span data-stu-id="e0807-175">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="e0807-176">A *.cshtml* ile HTML biçimlendirmesini içeren dosya C# Razor söz dizimini kullanarak kodu.</span><span class="sxs-lookup"><span data-stu-id="e0807-176">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="e0807-177">A *. cshtml.cs* içeren dosya C# sayfası olayları işleyen kodu.</span><span class="sxs-lookup"><span data-stu-id="e0807-177">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="e0807-178">Destekleyici dosyaları bir alt çizgi ile başlayan adları vardır.</span><span class="sxs-lookup"><span data-stu-id="e0807-178">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="e0807-179">Örneğin, *_Layout.cshtml* dosyası tüm sayfalar için ortak kullanıcı Arabirimi öğeleri yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e0807-179">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="e0807-180">Bu dosya sayfanın üst gezinti menüsünde ve sayfanın alt kısmındaki telif hakkı bildirimi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e0807-180">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="e0807-181">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="e0807-181">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="e0807-182">wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="e0807-182">wwwroot folder</span></span>

<span data-ttu-id="e0807-183">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="e0807-183">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="e0807-184">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="e0807-184">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="e0807-185">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="e0807-185">appSettings.json</span></span>

<span data-ttu-id="e0807-186">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="e0807-186">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="e0807-187">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e0807-187">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="e0807-188">Program.cs</span><span class="sxs-lookup"><span data-stu-id="e0807-188">Program.cs</span></span>

<span data-ttu-id="e0807-189">Programın giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="e0807-189">Contains the entry point for the program.</span></span> <span data-ttu-id="e0807-190">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="e0807-190">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="e0807-191">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="e0807-191">Startup.cs</span></span>

<span data-ttu-id="e0807-192">Olup olmadığı için tanımlama bilgileri onayı gerektirir gibi uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="e0807-192">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="e0807-193">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e0807-193">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0807-194">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e0807-194">Additional resources</span></span>

* [<span data-ttu-id="e0807-195">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="e0807-195">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="e0807-196">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="e0807-196">Next steps</span></span>

<span data-ttu-id="e0807-197">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="e0807-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e0807-198">Razor sayfaları web uygulaması oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="e0807-198">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="e0807-199">Bir uygulamayı çalıştırdınız.</span><span class="sxs-lookup"><span data-stu-id="e0807-199">Ran the app.</span></span>
> * <span data-ttu-id="e0807-200">Proje dosyalarını incelenir.</span><span class="sxs-lookup"><span data-stu-id="e0807-200">Examined the project files.</span></span>

<span data-ttu-id="e0807-201">Serinin sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="e0807-201">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e0807-202">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="e0807-202">Add a model</span></span>](xref:tutorials/razor-pages/model)
