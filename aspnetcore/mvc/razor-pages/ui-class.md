---
title: Yeniden kullanılabilir Razor kullanıcı Arabiriminde sınıf kitaplıkları ile ASP.NET çekirdek
author: Rick-Anderson
description: Bir sınıf kitaplığı'nda yeniden kullanılabilir Razor kullanıcı Arabirimi oluşturma açıklanmaktadır.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 4370fdaf7a5c066cec7b341a6012a100f8aed3ea
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734555"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="a9205-103">Yeniden kullanılabilir kullanıcı Arabirimi kullanarak ASP.NET Core Razor sınıf kitaplığı projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9205-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="a9205-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9205-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9205-105">Razor görünümleri, sayfalar, denetleyicileri, sayfa modelleri [görüntülemek bileşenleri](xref:mvc/views/view-components), ve veri modelleri bir Razor sınıf kitaplığı (RCL) içine oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="a9205-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="a9205-106">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a9205-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="a9205-107">Uygulamalar görünümler ve içerdiği sayfaları geçersiz kılmak ve RCL içerir.</span><span class="sxs-lookup"><span data-stu-id="a9205-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="a9205-108">Ne zaman görünümü, kısmi görünümü veya Razor sayfasını web uygulaması ve Razor biçimlendirme RCL bulunduğunda (*.cshtml* dosyası) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a9205-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="a9205-109">Bu özellik gerektirir [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="a9205-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="a9205-110">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9205-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="a9205-111">Razor UI içeren bir sınıf kitaplığı oluşturun</span><span class="sxs-lookup"><span data-stu-id="a9205-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9205-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9205-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9205-113">Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="a9205-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a9205-114">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="a9205-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a9205-115">(Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9205-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="a9205-116">Oluşturulan görünüm kitaplığı ile dosya adı çakışmaları önlemek için kitaplık adını uç olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="a9205-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="a9205-117">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="a9205-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a9205-118">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9205-118">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a9205-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a9205-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a9205-120">Komut satırı ' çalıştırma `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="a9205-120">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="a9205-121">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a9205-121">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="a9205-122">Daha fazla bilgi için bkz: [dotnet yeni](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a9205-122">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="a9205-123">Oluşturulan görünüm kitaplığı ile dosya adı çakışmaları önlemek için kitaplık adını uç olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="a9205-123">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="a9205-124">Razor dosyaları RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9205-124">Add Razor files to the RCL.</span></span>

<span data-ttu-id="a9205-125">İçinde içerik Git RCL öneririz *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="a9205-125">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="a9205-126">Razor sınıf kitaplığı içerik başvurma</span><span class="sxs-lookup"><span data-stu-id="a9205-126">Referencing Razor Class Library content</span></span>

<span data-ttu-id="a9205-127">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="a9205-127">The RCL can be referenced by:</span></span>

* <span data-ttu-id="a9205-128">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="a9205-128">NuGet package.</span></span> <span data-ttu-id="a9205-129">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [oluşturma ve bir NuGet Paketi Yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="a9205-129">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="a9205-130">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="a9205-130">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="a9205-131">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="a9205-131">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="a9205-132">RCL kısmi dosya erişimi</span><span class="sxs-lookup"><span data-stu-id="a9205-132">Partial files access in the RCL</span></span>

<span data-ttu-id="a9205-133">RCL dışında içerik için ASP.NET çekirdeği çalışma zamanı RCL kısmi dosyalarında aramaz.</span><span class="sxs-lookup"><span data-stu-id="a9205-133">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="a9205-134">Örneğin, örnek indirme içinde *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görüntüleyebilecek **değil** başvuruda bulunulamıyor *WebApp1\Pages\About.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="a9205-134">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="a9205-135">Ancak, RCL sayfaları ( *RazorUIClassLib /* **için** erişim *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a9205-135">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="a9205-136">İzlenecek yol: bir Razor sınıf kitaplığı proje oluşturma ve Razor sayfalarının projeden kullanma</span><span class="sxs-lookup"><span data-stu-id="a9205-136">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="a9205-137">İndirebilirsiniz [tam projesini](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ve yerine bunu oluşturmayı sınayın.</span><span class="sxs-lookup"><span data-stu-id="a9205-137">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="a9205-138">Örnek indirme ek kod ve projeyi test kolaylaştırmak bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="a9205-138">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="a9205-139">Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6098) yorumlarınızı adım adım yönergeler karşı indirme örnekleri üzerinde ile.</span><span class="sxs-lookup"><span data-stu-id="a9205-139">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="a9205-140">İndirme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="a9205-140">Test the download app</span></span>

<span data-ttu-id="a9205-141">Tamamlanan uygulama indirilen henüz ve gözden geçirme proje yerine oluşturacak, geçin [sonraki bölümde](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="a9205-141">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9205-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9205-142">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a9205-143">Açık *.sln* dosyasını Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="a9205-143">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="a9205-144">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a9205-144">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a9205-145">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a9205-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a9205-146">Bir komut isteminden *CLI* dizin RCL oluşturmak ve web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="a9205-146">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="a9205-147">Taşıma *WebApp1* dizin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9205-147">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="a9205-148">' Ndaki yönergeleri izleyin [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="a9205-148">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="a9205-149">Bir Razor sınıf kitaplığı oluşturun</span><span class="sxs-lookup"><span data-stu-id="a9205-149">Create a Razor Class Library</span></span>

<span data-ttu-id="a9205-150">Bu bölümde, bir Razor sınıf kitaplığı (RCL) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a9205-150">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="a9205-151">Razor dosyaları RCL eklenir.</span><span class="sxs-lookup"><span data-stu-id="a9205-151">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9205-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9205-152">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a9205-153">RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a9205-153">Create the RCL project:</span></span>

* <span data-ttu-id="a9205-154">Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="a9205-154">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a9205-155">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="a9205-155">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a9205-156">Uygulama adı **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="a9205-156">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="a9205-157">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="a9205-157">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a9205-158">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9205-158">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="a9205-159">Razor sayfalarının web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a9205-159">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="a9205-160">Gelen **Çözüm Gezgini**, çözüme sağ tıklayın > **Ekle** >  **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="a9205-160">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="a9205-161">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="a9205-161">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a9205-162">Uygulama adı **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="a9205-162">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="a9205-163">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="a9205-163">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a9205-164">Seçin **Web uygulaması** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9205-164">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="a9205-165">Razor dosyaları ve klasörleri projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9205-165">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="a9205-166">Adlı bir Razor kısmi görünüm dosya eklemek *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a9205-166">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="a9205-167">Biçimlendirme Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a9205-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="a9205-168">Kopya *_ViewStart.cshtml* WebApp1 proje dosyasından *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a9205-168">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="a9205-169">[Viewstart](xref:mvc/views/layout#running-code-before-each-view) dosya Razor sayfalarının proje düzeni kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a9205-169">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a9205-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a9205-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a9205-171">Komut satırından aşağıdakileri çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9205-171">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="a9205-172">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="a9205-172">The preceding commands:</span></span>

* <span data-ttu-id="a9205-173">Oluşturur `RazorUIClassLib` Razor sınıf kitaplığı (RCL).</span><span class="sxs-lookup"><span data-stu-id="a9205-173">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="a9205-174">Bir Razor _Message sayfası oluşturur ve RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="a9205-174">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="a9205-175">`-np` Parametresi oluşturur sayfa olmadan bir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="a9205-175">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="a9205-176">Oluşturur bir [viewstart](xref:mvc/views/layout#running-code-before-each-view) dosya ve RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="a9205-176">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="a9205-177">Viewstart dosyası (sonraki bölümde eklenir) Razor sayfalarının projesi düzenini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a9205-177">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="a9205-178">Razor sayfalarının güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="a9205-178">Update the Razor Pages:</span></span>

* <span data-ttu-id="a9205-179">Biçimlendirme Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a9205-179">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="a9205-180">Biçimlendirme Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a9205-180">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="a9205-181">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="a9205-181">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="a9205-182">Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="a9205-182">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a9205-183">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a9205-183">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="a9205-184">Viewimports hakkında daha fazla bilgi için bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="a9205-184">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="a9205-185">Derleyici hatası olmadığını doğrulamak için sınıf kitaplığı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a9205-185">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="a9205-186">Derleme çıktı içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="a9205-186">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="a9205-187">*RazorUIClassLib.Views.dll* derlenmiş Razor içeriği içerir.</span><span class="sxs-lookup"><span data-stu-id="a9205-187">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="a9205-188">Bir Razor sayfalarının projeden Razor kullanıcı Arabirimi kitaplığı kullanma</span><span class="sxs-lookup"><span data-stu-id="a9205-188">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9205-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9205-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9205-190">Gelen **Çözüm Gezgini**, sağ tıklayın **WebApp1** seçip **başlangıç projesi olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="a9205-190">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="a9205-191">Gelen **Çözüm Gezgini**, sağ tıklayın **WebApp1** seçip **derleme bağımlılıkları** > **proje bağımlılıkları**.</span><span class="sxs-lookup"><span data-stu-id="a9205-191">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="a9205-192">Denetleme **RazorUIClassLib** bir bağımlılık olarak **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="a9205-192">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="a9205-193">Gelen **Çözüm Gezgini**, sağ tıklayın **WebApp1** seçip **Ekle** > **başvuru**.</span><span class="sxs-lookup"><span data-stu-id="a9205-193">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="a9205-194">İçinde **başvuru Yöneticisi** iletişim kutusunda, onay **RazorUIClassLib** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9205-194">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="a9205-195">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a9205-195">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a9205-196">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a9205-196">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a9205-197">Razor sayfalarının web app ve Razor sayfalarının uygulama ve Razor sınıf kitaplığı içeren bir çözüm dosyasını oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a9205-197">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="a9205-198">Derleme ve web uygulaması çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9205-198">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="a9205-199">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="a9205-199">Test WebApp1</span></span>

<span data-ttu-id="a9205-200">Razor UI sınıf kitaplığı kullanılan doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a9205-200">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="a9205-201">Gözat `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="a9205-201">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="a9205-202">Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="a9205-202">Override views, partial views, and pages</span></span>

<span data-ttu-id="a9205-203">Ne zaman görünümü, kısmi görünümü veya Razor sayfasını bulundu hem web uygulamasını hem de Razor sınıf kitaplığı, Razor biçimlendirme (*.cshtml* dosyası) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a9205-203">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="a9205-204">Örneğin, ekleyin *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* WebApp1 için ve WebApp1 Page1 sürer öncelik Page1in Razor sınıf kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="a9205-204">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="a9205-205">Örnek indirme yeniden adlandırma *alanları/WebApp1/MyFeature2* için *alanları/WebApp1/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="a9205-205">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="a9205-206">Kopya *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a9205-206">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="a9205-207">Yeni konumu belirtmek için biçimlendirme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a9205-207">Update the markup to indicate the new location.</span></span> <span data-ttu-id="a9205-208">Derleme ve kısmi uygulamanın sürümü kullanılan doğrulamak üzere uygulamasını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a9205-208">Build and run the app to verify the app's version of the partial is being used.</span></span>
