---
title: ASP.NET Core Sınıf Kitaplığı'nda yeniden kullanılabilir Razor UI
author: Rick-Anderson
description: Yeniden kullanılabilir Razor kısmi görünümler, ASP.NET Core sınıf kitaplığında kullanarak kullanıcı Arabirimi oluşturma açıklanır.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/22/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 92c04c1ac4c70c6245accf272753bc914aaab860
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081866"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="1ff38-103">ASP.NET Core 'de Razor Sınıf Kitaplığı projesini kullanarak yeniden kullanılabilir kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ff38-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="1ff38-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ff38-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1ff38-105">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="1ff38-106">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="1ff38-107">Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="1ff38-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="1ff38-108">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="1ff38-109">Bu özellik gerektirir [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="1ff38-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="1ff38-110">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ff38-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="1ff38-111">Razor UI içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ff38-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ff38-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ff38-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1ff38-113">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1ff38-114">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1ff38-115">(Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="1ff38-116">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1ff38-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="1ff38-117">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1ff38-118">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="1ff38-119">RCL aşağıdaki proje dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1ff38-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ff38-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ff38-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ff38-121">Komut satırından çalıştırmak `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="1ff38-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="1ff38-122">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ff38-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="1ff38-123">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1ff38-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="1ff38-124">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1ff38-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="1ff38-125">Razor dosyaları için RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="1ff38-126">ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1ff38-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="1ff38-127">' De `~/Pages` içeriğinikullanımasunanbirRCLoluşturmakiçinRCLPagesdüzeninebakın.`~/Areas/Pages` [](#afs)</span><span class="sxs-lookup"><span data-stu-id="1ff38-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="1ff38-128">RCL içeriğine başvuruluyor</span><span class="sxs-lookup"><span data-stu-id="1ff38-128">Referencing RCL content</span></span>

<span data-ttu-id="1ff38-129">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="1ff38-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="1ff38-130">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="1ff38-130">NuGet package.</span></span> <span data-ttu-id="1ff38-131">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="1ff38-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="1ff38-132">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="1ff38-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="1ff38-133">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="1ff38-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="1ff38-134">İzlenecek yol: Bir Razor Pages projesinden bir RCL projesi oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="1ff38-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="1ff38-135">İndirebileceğiniz [tam proje](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ve yerine oluşturma test edin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="1ff38-136">Örnek indirme, ek kod ve test etmek proje kolaylaştırmak bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="1ff38-137">Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) yorumlarınızı üzerinde adım adım yönergeler ve örnekleri indirme ile.</span><span class="sxs-lookup"><span data-stu-id="1ff38-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="1ff38-138">İndirme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="1ff38-138">Test the download app</span></span>

<span data-ttu-id="1ff38-139">Tamamlanmış uygulamayı karşıdan henüz ve bunun yerine izlenecek projesi oluşturacak, atlamak [sonraki bölümde](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="1ff38-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ff38-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ff38-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ff38-141">Açık *.sln* dosyasını Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="1ff38-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="1ff38-142">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ff38-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ff38-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ff38-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ff38-144">Bir komut isteminden *CLI* dizin RCL oluşturun ve web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="1ff38-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="1ff38-145">Taşı *WebApp1* dizin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1ff38-145">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="1ff38-146">Bölümündeki yönergeleri [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="1ff38-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="1ff38-147">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ff38-147">Create an RCL</span></span>

<span data-ttu-id="1ff38-148">Bu bölümde bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ff38-148">In this section, an RCL is created.</span></span> <span data-ttu-id="1ff38-149">Razor dosyaları için RCL eklenir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ff38-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ff38-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ff38-151">RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ff38-151">Create the RCL project:</span></span>

* <span data-ttu-id="1ff38-152">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1ff38-153">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1ff38-154">Uygulamayı adlandırma **RazorUIClassLib** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="1ff38-155">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1ff38-156">Seçin **Razor sınıf kitaplığı** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="1ff38-157">Razor kısmi Görünüm adlı bir dosya ekleyin *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1ff38-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ff38-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ff38-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ff38-159">Komut satırından şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1ff38-159">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="1ff38-160">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="1ff38-160">The preceding commands:</span></span>

* <span data-ttu-id="1ff38-161">`RazorUIClassLib` RCL 'yi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ff38-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="1ff38-162">Bir Razor il_eti sayfası oluşturur ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="1ff38-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="1ff38-163">`-np` Parametresi olmadan sayfa oluşturur bir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="1ff38-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="1ff38-164">Oluşturur bir [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) dosya ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="1ff38-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="1ff38-165">*_ViewStart.cshtml* dosyası (sonraki bölümde eklenir) Razor sayfaları proje düzenini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="1ff38-166">Razor dosyaları ve klasörleri projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="1ff38-167">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="1ff38-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="1ff38-168">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="1ff38-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="1ff38-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="1ff38-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="1ff38-170">Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="1ff38-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1ff38-171">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ff38-171">For example:</span></span>

```dotnetcli
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="1ff38-172">Daha fazla bilgi için *_viewımports.cshtml*, bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="1ff38-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="1ff38-173">Derleyici hata doğrulamak için sınıf kitaplığı derleme:</span><span class="sxs-lookup"><span data-stu-id="1ff38-173">Build the class library to verify there are no compiler errors:</span></span>

```dotnetcli
dotnet build RazorUIClassLib
```

<span data-ttu-id="1ff38-174">Derleme çıktısını içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="1ff38-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="1ff38-175">*RazorUIClassLib.Views.dll* derlenmiş Razor içeriği.</span><span class="sxs-lookup"><span data-stu-id="1ff38-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="1ff38-176">Bir Razor sayfaları projeden Razor kullanıcı Arabirimi kitaplığını kullanma</span><span class="sxs-lookup"><span data-stu-id="1ff38-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ff38-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ff38-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ff38-178">Razor sayfaları web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ff38-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="1ff38-179">Gelen **Çözüm Gezgini**, çözüme sağ tıklayın > **Ekle** >  **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="1ff38-180">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1ff38-181">Uygulamayı adlandırma **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="1ff38-182">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1ff38-183">Seçin **Web uygulaması** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="1ff38-184">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **başlangıç projesi olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="1ff38-185">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **yapı bağımlılıkları** > **proje bağımlılıkları**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="1ff38-186">Denetleme **RazorUIClassLib** bağımlılık olarak **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="1ff38-187">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **Ekle** > **başvuru**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="1ff38-188">İçinde **başvuru Yöneticisi** iletişim kutusunda, onay **RazorUIClassLib** > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1ff38-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="1ff38-189">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ff38-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ff38-190">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ff38-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ff38-191">Razor Pages uygulamasını ve RCL 'yi içeren bir Razor Pages Web uygulaması ve çözüm dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ff38-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="1ff38-192">Web uygulaması derleyebilir ve:</span><span class="sxs-lookup"><span data-stu-id="1ff38-192">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="1ff38-193">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="1ff38-193">Test WebApp1</span></span>

<span data-ttu-id="1ff38-194">Razor UI sınıf kitaplığının kullanımda olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="1ff38-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="1ff38-195">konumuna gözatın `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="1ff38-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="1ff38-196">Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="1ff38-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="1ff38-197">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="1ff38-198">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="1ff38-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="1ff38-199">Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="1ff38-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="1ff38-200">Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1ff38-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="1ff38-201">Yeni bir konum belirtmek için işaretleme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="1ff38-202">Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ff38-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="1ff38-203">RCL sayfa düzeni</span><span class="sxs-lookup"><span data-stu-id="1ff38-203">RCL Pages layout</span></span>

<span data-ttu-id="1ff38-204">RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ff38-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="1ff38-205">*RazorUIClassLib/sayfaları*</span><span class="sxs-lookup"><span data-stu-id="1ff38-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="1ff38-206">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="1ff38-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="1ff38-207">Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1ff38-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="1ff38-208">`<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1ff38-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="1ff38-209">Statik varlıklar içeren bir RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ff38-209">Create an RCL with static assets</span></span>

<span data-ttu-id="1ff38-210">RCL, RCL 'nin tüketen uygulaması tarafından başvurulabilen, yardımcı statik varlıklar gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="1ff38-211">ASP.NET Core, tüketen bir uygulama tarafından kullanılabilen statik varlıkları içeren RCLs oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="1ff38-212">Yardımcı varlıkları RCL 'nin bir parçası olarak dahil etmek için, sınıf kitaplığında bir *Wwwroot* klasörü oluşturun ve gerekli dosyaları bu klasöre ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="1ff38-213">RCL 'yi paketleyerek, *Wwwroot* klasöründeki tüm yardımcı varlıklar pakete otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-213">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="1ff38-214">Statik varlıkları hariç tut</span><span class="sxs-lookup"><span data-stu-id="1ff38-214">Exclude static assets</span></span>

<span data-ttu-id="1ff38-215">Statik varlıkları dışlamak için, istenen dışlama yolunu `$(DefaultItemExcludes)` proje dosyasındaki özellik grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-215">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="1ff38-216">Girişleri noktalı virgül (`;`) ile ayırın.</span><span class="sxs-lookup"><span data-stu-id="1ff38-216">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="1ff38-217">Aşağıdaki örnekte, *Wwwroot* klasöründeki *lib. css* stil sayfası statik bir varlık olarak değerlendirilmez ve yayımlanan RCL 'ye dahil değildir:</span><span class="sxs-lookup"><span data-stu-id="1ff38-217">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="1ff38-218">TypeScript tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="1ff38-218">Typescript integration</span></span>

<span data-ttu-id="1ff38-219">TypeScript dosyalarını RCL 'ye eklemek için:</span><span class="sxs-lookup"><span data-stu-id="1ff38-219">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="1ff38-220">TypeScript dosyalarını ( *. TS*) *Wwwroot* klasörünün dışına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-220">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="1ff38-221">Örneğin, dosyaları bir *istemci* klasörüne yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ff38-221">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="1ff38-222">*Wwwroot* klasörü için TypeScript derleme çıkışını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1ff38-222">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="1ff38-223">Proje dosyasındaki`PropertyGroup` öğesinin içindeki özelliğini ayarlayın: `TypescriptOutDir`</span><span class="sxs-lookup"><span data-stu-id="1ff38-223">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="1ff38-224">Proje dosyasında bir `PropertyGroup` öğesinin içine aşağıdaki hedefi ekleyerek TypeScript `ResolveCurrentProjectStaticWebAssets` hedefini hedefin bağımlılığı olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1ff38-224">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="1ff38-225">Başvurulan bir RCL 'den içerik tüketme</span><span class="sxs-lookup"><span data-stu-id="1ff38-225">Consume content from a referenced RCL</span></span>

<span data-ttu-id="1ff38-226">RCL 'nin *Wwwroot* klasörüne dahil edilen dosyalar, ön ek `_content/{LIBRARY NAME}/`altında tüketen uygulamaya sunulur.</span><span class="sxs-lookup"><span data-stu-id="1ff38-226">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="1ff38-227">Örneğin, *Razor. Class. lib* adlı bir kitaplık, konumundaki `_content/Razor.Class.Lib/`statik içeriğe bir yol ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="1ff38-227">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="1ff38-228">Tüketen uygulama `<script>` `<style>`, ,,vediğerHTMLetiketleriylekitaplıktarafındansunulanstatikvarlıklarabaşvurur.`<img>`</span><span class="sxs-lookup"><span data-stu-id="1ff38-228">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="1ff38-229">Kullanan uygulamada [statik dosya desteğinin](xref:fundamentals/static-files) etkinleştirilmesi `Startup.Configure`gerekir:</span><span class="sxs-lookup"><span data-stu-id="1ff38-229">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="1ff38-230">Yapı çıktısından (`dotnet run`) kullanan uygulamayı çalıştırırken, varsayılan olarak, statik Web varlıkları geliştirme ortamında etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-230">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="1ff38-231">Derleme çıktılarından çalışırken diğer ortamlardaki varlıkları desteklemek için, *program.cs*içindeki konak `UseStaticWebAssets` Oluşturucu 'da çağırın:</span><span class="sxs-lookup"><span data-stu-id="1ff38-231">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="1ff38-232">Yayımlanan `UseStaticWebAssets` çıktısından (`dotnet publish`) bir uygulama çalıştırılırken çağırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1ff38-232">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="1ff38-233">Çoklu projeli geliştirme akışı</span><span class="sxs-lookup"><span data-stu-id="1ff38-233">Multi-project development flow</span></span>

<span data-ttu-id="1ff38-234">Kullanan uygulama şu şekilde çalışır:</span><span class="sxs-lookup"><span data-stu-id="1ff38-234">When the consuming app runs:</span></span>

* <span data-ttu-id="1ff38-235">RCL içindeki varlıklar özgün klasörlerinde kalır.</span><span class="sxs-lookup"><span data-stu-id="1ff38-235">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="1ff38-236">Varlıklar, tüketim uygulamasına taşınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ff38-236">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="1ff38-237">RCL 'nin *Wwwroot* klasörü içindeki tüm değişiklikler, RCL yeniden oluşturulduktan ve tüketen uygulamayı yeniden oluşturmadan önce tüketen uygulamaya yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="1ff38-237">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="1ff38-238">RCL yapılandırıldığında, statik Web varlık konumlarını açıklayan bir bildirim oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ff38-238">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="1ff38-239">Tüketen uygulama, başvurulan proje ve paketlerden varlıkları kullanmak için çalışma zamanında bildirimi okur.</span><span class="sxs-lookup"><span data-stu-id="1ff38-239">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="1ff38-240">Bir RCL 'ye yeni bir varlık eklendiğinde, bir uygulamanın yeni varlığa erişebilmesi için bildirim güncellemek üzere RCL 'nin yeniden oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ff38-240">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="1ff38-241">Yayımlama</span><span class="sxs-lookup"><span data-stu-id="1ff38-241">Publish</span></span>

<span data-ttu-id="1ff38-242">Uygulama yayımlandığında, tüm başvurulan projeler ve paketlerin yardımcı varlıkları altında `_content/{LIBRARY NAME}/`yayımlanan uygulamanın *Wwwroot* klasörüne kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="1ff38-242">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
