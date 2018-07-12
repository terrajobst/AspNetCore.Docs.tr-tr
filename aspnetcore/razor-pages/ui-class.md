---
title: ASP.NET Core Sınıf Kitaplığı'nda yeniden kullanılabilir Razor UI
author: Rick-Anderson
description: Yeniden kullanılabilir Razor UI bir sınıf kitaplığında oluşturulması açıklanmaktadır.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 8190302a15670b0a7474445f7b11d4cba46981db
ms.sourcegitcommit: 19cbda409bdbbe42553dc385ea72d2a8e246509c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992853"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="f1ef0-103">Yeniden kullanılabilir kullanıcı Arabirimi ASP.NET Core Razor sınıf kitaplığı projesini kullanarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="f1ef0-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1ef0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1ef0-105">Razor görünümleri, sayfalar, denetleyicileri, sayfa modelleri [görüntülemek bileşenleri](xref:mvc/views/view-components), ve veri modelleri bir Razor sınıf kitaplığı (RCL) içine derlenebilir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="f1ef0-106">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="f1ef0-107">Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="f1ef0-108">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur (*.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="f1ef0-109">Bu özellik gerektirir [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="f1ef0-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="f1ef0-110">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f1ef0-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="f1ef0-111">Razor UI içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f1ef0-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f1ef0-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1ef0-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f1ef0-113">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f1ef0-114">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f1ef0-115">(Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="f1ef0-116">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="f1ef0-117">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f1ef0-118">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-118">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f1ef0-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f1ef0-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f1ef0-120">Komut satırından çalıştırmak `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-120">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="f1ef0-121">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-121">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="f1ef0-122">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="f1ef0-122">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="f1ef0-123">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-123">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="f1ef0-124">Razor dosyaları için RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-124">Add Razor files to the RCL.</span></span>

<span data-ttu-id="f1ef0-125">İçinde içerik Git RCL öneririz *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-125">We recommend RCL content go in the *Areas* folder.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="f1ef0-126">Razor sınıf kitaplığı içeriği başvurma</span><span class="sxs-lookup"><span data-stu-id="f1ef0-126">Referencing Razor Class Library content</span></span>

<span data-ttu-id="f1ef0-127">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-127">The RCL can be referenced by:</span></span>

* <span data-ttu-id="f1ef0-128">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-128">NuGet package.</span></span> <span data-ttu-id="f1ef0-129">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="f1ef0-129">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="f1ef0-130">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-130">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="f1ef0-131">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="f1ef0-131">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="f1ef0-132">İzlenecek yol: bir Razor sınıf kitaplığı projesi oluşturun ve bir Razor sayfaları projeden kullanın</span><span class="sxs-lookup"><span data-stu-id="f1ef0-132">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="f1ef0-133">İndirebileceğiniz [tam proje](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ve yerine oluşturma test edin.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-133">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="f1ef0-134">Örnek indirme, ek kod ve test etmek proje kolaylaştırmak bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-134">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="f1ef0-135">Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6098) yorumlarınızı üzerinde adım adım yönergeler ve örnekleri indirme ile.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-135">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="f1ef0-136">İndirme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="f1ef0-136">Test the download app</span></span>

<span data-ttu-id="f1ef0-137">Tamamlanmış uygulamayı karşıdan henüz ve bunun yerine izlenecek projesi oluşturacak, atlamak [sonraki bölümde](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="f1ef0-137">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f1ef0-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1ef0-138">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f1ef0-139">Açık *.sln* dosyasını Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-139">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="f1ef0-140">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-140">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f1ef0-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f1ef0-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f1ef0-142">Bir komut isteminden *CLI* dizin RCL oluşturun ve web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-142">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="f1ef0-143">Taşı *WebApp1* dizin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-143">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="f1ef0-144">Bölümündeki yönergeleri [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="f1ef0-144">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="f1ef0-145">Razor sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f1ef0-145">Create a Razor Class Library</span></span>

<span data-ttu-id="f1ef0-146">Bu bölümde, Razor sınıf kitaplığı (RCL) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-146">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="f1ef0-147">Razor dosyaları için RCL eklenir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-147">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f1ef0-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1ef0-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f1ef0-149">RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-149">Create the RCL project:</span></span>

* <span data-ttu-id="f1ef0-150">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f1ef0-151">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f1ef0-152">Uygulamayı adlandırma **RazorUIClassLib** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-152">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="f1ef0-153">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-153">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f1ef0-154">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-154">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="f1ef0-155">Razor kısmi Görünüm adlı bir dosya ekleyin *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-155">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f1ef0-156">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f1ef0-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f1ef0-157">Komut satırından şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-157">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="f1ef0-158">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-158">The preceding commands:</span></span>

* <span data-ttu-id="f1ef0-159">Oluşturur `RazorUIClassLib` Razor sınıf kitaplığı (RCL).</span><span class="sxs-lookup"><span data-stu-id="f1ef0-159">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="f1ef0-160">Bir Razor il_eti sayfası oluşturur ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-160">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="f1ef0-161">`-np` Parametresi olmadan sayfa oluşturur bir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-161">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="f1ef0-162">Oluşturur bir [viewstart](xref:mvc/views/layout#running-code-before-each-view) dosya ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-162">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="f1ef0-163">Viewstart dosya (sonraki bölümde eklenir) Razor sayfaları proje düzenini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-163">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

------

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="f1ef0-164">Razor dosyaları ve klasörleri projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-164">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="f1ef0-165">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-165">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="f1ef0-166">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-166">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="f1ef0-167">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="f1ef0-167">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="f1ef0-168">Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-168">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f1ef0-169">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-169">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="f1ef0-170">Viewimports hakkında daha fazla bilgi için bkz. [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="f1ef0-170">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="f1ef0-171">Derleyici hata doğrulamak için sınıf kitaplığı derleme:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-171">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="f1ef0-172">Derleme çıktısını içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-172">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="f1ef0-173">*RazorUIClassLib.Views.dll* derlenmiş Razor içeriği.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-173">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="f1ef0-174">Bir Razor sayfaları projeden Razor kullanıcı Arabirimi kitaplığını kullanma</span><span class="sxs-lookup"><span data-stu-id="f1ef0-174">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f1ef0-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1ef0-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f1ef0-176">Razor sayfaları web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-176">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="f1ef0-177">Gelen **Çözüm Gezgini**, çözüme sağ tıklayın > **Ekle** >  **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-177">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="f1ef0-178">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-178">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f1ef0-179">Uygulamayı adlandırma **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-179">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="f1ef0-180">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-180">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f1ef0-181">Seçin **Web uygulaması** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-181">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="f1ef0-182">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **başlangıç projesi olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-182">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="f1ef0-183">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **yapı bağımlılıkları** > **proje bağımlılıkları**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-183">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="f1ef0-184">Denetleme **RazorUIClassLib** bağımlılık olarak **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-184">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="f1ef0-185">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **Ekle** > **başvuru**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-185">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="f1ef0-186">İçinde **başvuru Yöneticisi** iletişim kutusunda, onay **RazorUIClassLib** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-186">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="f1ef0-187">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-187">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f1ef0-188">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f1ef0-188">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f1ef0-189">Razor sayfaları web uygulaması ve Razor sayfaları uygulama ve Razor sınıf kitaplığı içeren bir çözüm dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-189">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

<span data-ttu-id="f1ef0-190">Web uygulaması derleyebilir ve:</span><span class="sxs-lookup"><span data-stu-id="f1ef0-190">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="f1ef0-191">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="f1ef0-191">Test WebApp1</span></span>

<span data-ttu-id="f1ef0-192">Razor UI sınıf kitaplığı kullanılan doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-192">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="f1ef0-193">Gözat `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-193">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="f1ef0-194">Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="f1ef0-194">Override views, partial views, and pages</span></span>

<span data-ttu-id="f1ef0-195">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de Razor sınıf kitaplığı, Razor işaretlemesi bulunur (*.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-195">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="f1ef0-196">Örneğin, ekleme *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* WebApp1 için ve Sayfa1 WebApp1 içinde önceliklidir Page1in üzerinde Razor sınıf kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-196">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="f1ef0-197">Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-197">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="f1ef0-198">Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-198">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="f1ef0-199">Yeni bir konum belirtmek için işaretleme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-199">Update the markup to indicate the new location.</span></span> <span data-ttu-id="f1ef0-200">Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f1ef0-200">Build and run the app to verify the app's version of the partial is being used.</span></span>
