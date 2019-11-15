---
title: 'Öğretici: ASP.NET Core Razor Pages ile çalışmaya başlama'
author: rick-anderson
description: Bu öğretici dizisinde Razor Pages ASP.NET Core nasıl kullanılacağı gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturma, veri erişimi için Entity Framework Core ve SQL Server kullanma, arama işlevselliği ekleme, giriş doğrulaması ekleme ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: a8381dee05f267077a29999f3d8bbe6327c2b863
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74116153"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="be280-104">Öğretici: ASP.NET Core Razor Pages ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="be280-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="be280-105">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="be280-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="be280-106">Bu, bir serinin ASP.NET Core Razor Pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="be280-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="be280-107">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="be280-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="be280-108">Bu öğreticide şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="be280-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="be280-109">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="be280-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="be280-110">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="be280-110">Run the app.</span></span>
> * <span data-ttu-id="be280-111">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="be280-111">Examine the project files.</span></span>

<span data-ttu-id="be280-112">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="be280-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="be280-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="be280-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be280-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be280-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be280-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be280-117">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="be280-118">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="be280-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be280-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be280-120">Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="be280-121">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="be280-122">Yeni ASP.NET Core Web uygulaması ![](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="be280-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="be280-123">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="be280-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="be280-124">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="be280-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="be280-125">Yeni ASP.NET Core Web uygulaması ![](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="be280-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="be280-126">Açılan **Web uygulamasındaki** **ASP.NET Core 3,0** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="be280-128">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="be280-128">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be280-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be280-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="be280-131">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="be280-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="be280-132">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="be280-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="be280-133">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="be280-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="be280-134">`dotnet new` komutu, *RazorPagesMovie* klasöründe yeni bir Razor Pages projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="be280-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="be280-135">`code` komutu, geçerli Visual Studio Code örneğindeki *RazorPagesMovie* klasörünü açar.</span><span class="sxs-lookup"><span data-stu-id="be280-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="be280-136">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, **gerekli varlıkların derleme ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="be280-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="be280-137">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-137">Select **Yes**.</span></span>

  <span data-ttu-id="be280-138">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="be280-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be280-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="be280-140">**Dosya** > **yeni çözüm**seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-140">Select **File** > **New Solution**.</span></span>

![macOS yeni çözüm](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="be280-142">**Web uygulaması** > bir **sonraki**> **.NET Core** > **App** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="be280-144">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** Iletişim kutusunda **hedef Framework 'ü** **.NET Core 3,0**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="be280-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![macOS .NET Core 3,0 seçimi](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="be280-146">Projeyi **RazorPagesMovie**olarak adlandırın ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="be280-148">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="be280-148">Open the project</span></span>

<span data-ttu-id="be280-149">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="be280-150">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="be280-150">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="be280-151">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="be280-151">Examine the project files</span></span>

<span data-ttu-id="be280-152">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="be280-152">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="be280-153">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="be280-153">Pages folder</span></span>

<span data-ttu-id="be280-154">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-154">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="be280-155">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="be280-155">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="be280-156">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="be280-156">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="be280-157">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="be280-157">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="be280-158">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="be280-158">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="be280-159">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="be280-159">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="be280-160">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="be280-160">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="be280-161">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="be280-161">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="be280-162">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="be280-162">wwwroot folder</span></span>

<span data-ttu-id="be280-163">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-163">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="be280-164">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="be280-164">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="be280-165">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="be280-165">appSettings.json</span></span>

<span data-ttu-id="be280-166">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-166">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="be280-167">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="be280-167">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="be280-168">Program.cs</span><span class="sxs-lookup"><span data-stu-id="be280-168">Program.cs</span></span>

<span data-ttu-id="be280-169">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-169">Contains the entry point for the program.</span></span> <span data-ttu-id="be280-170">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="be280-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="be280-171">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="be280-171">Startup.cs</span></span>

<span data-ttu-id="be280-172">Uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-172">Contains code that configures app behavior.</span></span> <span data-ttu-id="be280-173">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="be280-173">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be280-174">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="be280-174">Next steps</span></span>

<span data-ttu-id="be280-175">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="be280-175">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="be280-176">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="be280-176">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="be280-177">Bu, bir serinin ilk öğreticisidir.</span><span class="sxs-lookup"><span data-stu-id="be280-177">This is the first tutorial of a series.</span></span> <span data-ttu-id="be280-178">[Seriler](xref:tutorials/razor-pages/index) , bir ASP.NET Core Razor pages Web uygulaması oluşturma hakkında temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="be280-178">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="be280-179">Serinin sonunda, bir film veritabanını yöneten bir uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="be280-179">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="be280-180">Bu öğreticide şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="be280-180">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="be280-181">Razor Pages bir Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="be280-181">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="be280-182">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="be280-182">Run the app.</span></span>
> * <span data-ttu-id="be280-183">Proje dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="be280-183">Examine the project files.</span></span>

<span data-ttu-id="be280-184">Bu öğreticinin sonunda, daha sonraki öğreticilerde oluşturacağınız çalışan bir Razor Pages Web uygulamasına sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="be280-184">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="be280-186">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="be280-186">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be280-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-187">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be280-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be280-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be280-189">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="be280-190">Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="be280-190">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be280-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be280-192">Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="be280-193">Yeni bir ASP.NET Core Web uygulaması oluşturun ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-193">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="be280-195">Projeyi **RazorPagesMovie**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="be280-195">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="be280-196">Kodu kopyaladığınızda ve yapıştırdığınızda ad alanlarının eşleşmesi için Project *RazorPagesMovie* olarak adı vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="be280-196">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/config.png)

* <span data-ttu-id="be280-198">Açılan **Web uygulamasındaki** **ASP.NET Core 2,2** ' i seçin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-198">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="be280-200">Aşağıdaki Başlatıcı proje oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="be280-200">The following starter project is created:</span></span>

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be280-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be280-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="be280-203">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="be280-203">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="be280-204">Projeyi içerecek dizine (`cd`) geçin.</span><span class="sxs-lookup"><span data-stu-id="be280-204">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="be280-205">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="be280-205">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="be280-206">`dotnet new` komutu, *RazorPagesMovie* klasöründe yeni bir Razor Pages projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="be280-206">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="be280-207">`code` komutu, geçerli Visual Studio Code örneğindeki *RazorPagesMovie* klasörünü açar.</span><span class="sxs-lookup"><span data-stu-id="be280-207">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="be280-208">Durum çubuğunun omnisharp Yangın simgesi yeşil ' i etkinleştirdikten sonra, **gerekli varlıkların derleme ve hata ayıklama için ' RazorPagesMovie ' içinde eksik olduğunu soran bir iletişim kutusu yok. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="be280-208">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="be280-209">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-209">Select **Yes**.</span></span>

  <span data-ttu-id="be280-210">*Launch. JSON* ve *Tasks. JSON* dosyalarını içeren bir *. vscode* dizini, projenin kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="be280-210">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be280-211">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="be280-212">Terminalden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="be280-212">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="be280-213">Yukarıdaki komutlar, bir Razor Pages projesi oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="be280-213">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="be280-214">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="be280-214">Open the project</span></span>

<span data-ttu-id="be280-215">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *RazorPagesMovie. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-215">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="be280-216">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="be280-216">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be280-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be280-218">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="be280-218">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="be280-219">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="be280-219">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="be280-220">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="be280-220">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="be280-221">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="be280-221">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="be280-222">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="be280-222">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="be280-223">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="be280-223">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="be280-224">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-224">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="be280-225">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-225">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="be280-227">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="be280-227">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be280-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be280-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="be280-230">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="be280-230">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="be280-231">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `http://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="be280-231">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="be280-232">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="be280-232">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="be280-233">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="be280-233">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="be280-234">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="be280-234">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="be280-235">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="be280-236">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="be280-238">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="be280-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be280-240">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be280-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="be280-241">Hata ayıklayıcı olmadan çalıştırmak için **cmd-opt-F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="be280-241">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="be280-242">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `http://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="be280-242">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="be280-243">Uygulamanın giriş sayfasında, izlemeye izin vermek için **kabul et** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="be280-243">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="be280-244">Bu uygulama kişisel bilgileri izlemez, ancak proje şablonu, Avrupa Birliği 'nin [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)ile uyumlu olması için ihtiyaç duymanız durumunda izin özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-244">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="be280-246">Aşağıdaki görüntüde, izlemeye onay verdikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="be280-246">The following image shows the app after you give consent to tracking:</span></span>

  ![Giriş veya dizin sayfası](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="be280-248">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="be280-248">Examine the project files</span></span>

<span data-ttu-id="be280-249">Aşağıda, daha sonraki öğreticilerde birlikte çalışacağımız ana proje klasörlerine ve dosyalarına genel bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="be280-249">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="be280-250">Sayfalar klasörü</span><span class="sxs-lookup"><span data-stu-id="be280-250">Pages folder</span></span>

<span data-ttu-id="be280-251">Razor sayfaları ve destekleyici dosyalar içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-251">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="be280-252">Her Razor sayfası bir dosya çiftidir:</span><span class="sxs-lookup"><span data-stu-id="be280-252">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="be280-253">Razor söz dizimi kullanarak C# kodla HTML işaretlemesi içeren bir *. cshtml* dosyası.</span><span class="sxs-lookup"><span data-stu-id="be280-253">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="be280-254">Sayfa olaylarını işleyen kodu içeren C# bir *. cshtml.cs* dosyası.</span><span class="sxs-lookup"><span data-stu-id="be280-254">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="be280-255">Destekleyici dosyalar bir alt çizgiyle başlayan adlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="be280-255">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="be280-256">Örneğin, *_Layout. cshtml* dosyası tüm sayfalarda ortak kullanıcı arabirimi öğelerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="be280-256">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="be280-257">Bu dosya sayfanın en üstündeki gezinti menüsünü ve sayfanın alt kısmındaki telif hakkı bildirimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="be280-257">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="be280-258">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="be280-258">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="be280-259">Wwwroot klasörü</span><span class="sxs-lookup"><span data-stu-id="be280-259">wwwroot folder</span></span>

<span data-ttu-id="be280-260">HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-260">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="be280-261">Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="be280-261">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="be280-262">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="be280-262">appSettings.json</span></span>

<span data-ttu-id="be280-263">Bağlantı dizeleri gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-263">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="be280-264">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="be280-264">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="be280-265">Program.cs</span><span class="sxs-lookup"><span data-stu-id="be280-265">Program.cs</span></span>

<span data-ttu-id="be280-266">Programın giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-266">Contains the entry point for the program.</span></span> <span data-ttu-id="be280-267">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="be280-267">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="be280-268">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="be280-268">Startup.cs</span></span>

<span data-ttu-id="be280-269">Tanımlama bilgilerinin onayını gerektirip gerektirmediğini belirten uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="be280-269">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="be280-270">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="be280-270">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be280-271">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="be280-271">Additional resources</span></span>

* [<span data-ttu-id="be280-272">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="be280-272">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="be280-273">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="be280-273">Next steps</span></span>

<span data-ttu-id="be280-274">Serideki bir sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="be280-274">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="be280-275">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="be280-275">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
