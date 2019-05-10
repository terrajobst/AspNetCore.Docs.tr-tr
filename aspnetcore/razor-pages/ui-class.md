---
title: ASP.NET Core Sınıf Kitaplığı'nda yeniden kullanılabilir Razor UI
author: Rick-Anderson
description: Yeniden kullanılabilir Razor kısmi görünümler, ASP.NET Core sınıf kitaplığında kullanarak kullanıcı Arabirimi oluşturma açıklanır.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/07/2018
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 7ec36cc8f4832fb1e1a50831dfcb88f3cafb5ca9
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901889"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="f2dad-103">ASP.NET Core Razor sınıf kitaplığı projesi kullanarak yeniden kullanılabilir kullanıcı Arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="f2dad-103">Create reusable UI using the Razor Class Library project in ASP.NET Core</span></span>

<span data-ttu-id="f2dad-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f2dad-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f2dad-105">Razor görünümleri, sayfalar, denetleyicileri, sayfa modelleri [görüntülemek bileşenleri](xref:mvc/views/view-components), ve veri modelleri bir Razor sınıf kitaplığı (RCL) içine derlenebilir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="f2dad-106">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="f2dad-107">Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="f2dad-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="f2dad-108">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur (*.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="f2dad-109">Bu özellik gerektirir [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="f2dad-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="f2dad-110">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f2dad-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="f2dad-111">Razor UI içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f2dad-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2dad-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2dad-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2dad-113">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f2dad-114">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f2dad-115">(Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="f2dad-116">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="f2dad-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="f2dad-117">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f2dad-118">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="f2dad-119">Razor sınıf kitaplığı aşağıdaki proje dosyası vardır:</span><span class="sxs-lookup"><span data-stu-id="f2dad-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2dad-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f2dad-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f2dad-121">Komut satırından çalıştırmak `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="f2dad-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="f2dad-122">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f2dad-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="f2dad-123">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="f2dad-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="f2dad-124">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="f2dad-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="f2dad-125">Razor dosyaları için RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f2dad-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="f2dad-126">ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="f2dad-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="f2dad-127">Bkz: [RCL sayfa düzeni](#afs) kullanıma sunan bir RCL içeriği oluşturmak için `~/Pages` yerine `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="f2dad-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="f2dad-128">Razor sınıf kitaplığı içeriği başvurma</span><span class="sxs-lookup"><span data-stu-id="f2dad-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="f2dad-129">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="f2dad-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="f2dad-130">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="f2dad-130">NuGet package.</span></span> <span data-ttu-id="f2dad-131">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="f2dad-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="f2dad-132">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="f2dad-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="f2dad-133">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="f2dad-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="f2dad-134">İzlenecek yol: Razor sınıf kitaplığı projesi oluşturun ve bir Razor sayfaları projeden kullanın</span><span class="sxs-lookup"><span data-stu-id="f2dad-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="f2dad-135">İndirebileceğiniz [tam proje](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ve yerine oluşturma test edin.</span><span class="sxs-lookup"><span data-stu-id="f2dad-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="f2dad-136">Örnek indirme, ek kod ve test etmek proje kolaylaştırmak bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="f2dad-137">Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) yorumlarınızı üzerinde adım adım yönergeler ve örnekleri indirme ile.</span><span class="sxs-lookup"><span data-stu-id="f2dad-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="f2dad-138">İndirme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="f2dad-138">Test the download app</span></span>

<span data-ttu-id="f2dad-139">Tamamlanmış uygulamayı karşıdan henüz ve bunun yerine izlenecek projesi oluşturacak, atlamak [sonraki bölümde](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="f2dad-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2dad-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2dad-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2dad-141">Açık *.sln* dosyasını Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="f2dad-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="f2dad-142">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f2dad-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2dad-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f2dad-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f2dad-144">Bir komut isteminden *CLI* dizin RCL oluşturun ve web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="f2dad-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="f2dad-145">Taşı *WebApp1* dizin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f2dad-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="f2dad-146">Bölümündeki yönergeleri [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="f2dad-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="f2dad-147">Razor sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f2dad-147">Create a Razor Class Library</span></span>

<span data-ttu-id="f2dad-148">Bu bölümde, Razor sınıf kitaplığı (RCL) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f2dad-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="f2dad-149">Razor dosyaları için RCL eklenir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2dad-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2dad-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2dad-151">RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f2dad-151">Create the RCL project:</span></span>

* <span data-ttu-id="f2dad-152">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f2dad-153">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f2dad-154">Uygulamayı adlandırma **RazorUIClassLib** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="f2dad-155">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f2dad-156">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="f2dad-157">Razor kısmi Görünüm adlı bir dosya ekleyin *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f2dad-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2dad-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f2dad-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f2dad-159">Komut satırından şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f2dad-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="f2dad-160">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="f2dad-160">The preceding commands:</span></span>

* <span data-ttu-id="f2dad-161">Oluşturur `RazorUIClassLib` Razor sınıf kitaplığı (RCL).</span><span class="sxs-lookup"><span data-stu-id="f2dad-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="f2dad-162">Bir Razor il_eti sayfası oluşturur ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="f2dad-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="f2dad-163">`-np` Parametresi olmadan sayfa oluşturur bir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="f2dad-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="f2dad-164">Oluşturur bir [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) dosya ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="f2dad-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="f2dad-165">*_ViewStart.cshtml* dosyası (sonraki bölümde eklenir) Razor sayfaları proje düzenini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="f2dad-166">Razor dosyaları ve klasörleri projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f2dad-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="f2dad-167">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f2dad-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="f2dad-168">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f2dad-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="f2dad-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="f2dad-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="f2dad-170">Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="f2dad-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f2dad-171">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f2dad-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="f2dad-172">Daha fazla bilgi için *_viewımports.cshtml*, bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="f2dad-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="f2dad-173">Derleyici hata doğrulamak için sınıf kitaplığı derleme:</span><span class="sxs-lookup"><span data-stu-id="f2dad-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="f2dad-174">Derleme çıktısını içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="f2dad-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="f2dad-175">*RazorUIClassLib.Views.dll* derlenmiş Razor içeriği.</span><span class="sxs-lookup"><span data-stu-id="f2dad-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="f2dad-176">Bir Razor sayfaları projeden Razor kullanıcı Arabirimi kitaplığını kullanma</span><span class="sxs-lookup"><span data-stu-id="f2dad-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2dad-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2dad-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2dad-178">Razor sayfaları web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f2dad-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="f2dad-179">Gelen **Çözüm Gezgini**, çözüme sağ tıklayın > **Ekle** >  **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="f2dad-180">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f2dad-181">Uygulamayı adlandırma **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="f2dad-182">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f2dad-183">Seçin **Web uygulaması** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="f2dad-184">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **başlangıç projesi olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="f2dad-185">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **yapı bağımlılıkları** > **proje bağımlılıkları**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="f2dad-186">Denetleme **RazorUIClassLib** bağımlılık olarak **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="f2dad-187">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **Ekle** > **başvuru**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="f2dad-188">İçinde **başvuru Yöneticisi** iletişim kutusunda, onay **RazorUIClassLib** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f2dad-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="f2dad-189">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f2dad-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2dad-190">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f2dad-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f2dad-191">Razor sayfaları web uygulaması ve Razor sayfaları uygulama ve Razor sınıf kitaplığı içeren bir çözüm dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f2dad-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="f2dad-192">Web uygulaması derleyebilir ve:</span><span class="sxs-lookup"><span data-stu-id="f2dad-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="f2dad-193">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="f2dad-193">Test WebApp1</span></span>

<span data-ttu-id="f2dad-194">Razor UI sınıf kitaplığı kullanılan doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f2dad-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="f2dad-195">konumuna gözatın `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="f2dad-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="f2dad-196">Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="f2dad-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="f2dad-197">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de Razor sınıf kitaplığı, Razor işaretlemesi bulunur (*.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f2dad-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="f2dad-198">Örneğin, ekleme *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* WebApp1 için ve Sayfa1 WebApp1 içinde önceliklidir Sayfa1 Razor Sınıf Kitaplığı'nda.</span><span class="sxs-lookup"><span data-stu-id="f2dad-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the Razor Class Library.</span></span>

<span data-ttu-id="f2dad-199">Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="f2dad-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="f2dad-200">Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f2dad-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="f2dad-201">Yeni bir konum belirtmek için işaretleme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="f2dad-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="f2dad-202">Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f2dad-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="f2dad-203">RCL sayfa düzeni</span><span class="sxs-lookup"><span data-stu-id="f2dad-203">RCL Pages layout</span></span>

<span data-ttu-id="f2dad-204">RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f2dad-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="f2dad-205">*RazorUIClassLib/sayfaları*</span><span class="sxs-lookup"><span data-stu-id="f2dad-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="f2dad-206">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="f2dad-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="f2dad-207">Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f2dad-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="f2dad-208">`<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f2dad-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```
