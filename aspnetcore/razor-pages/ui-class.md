---
title: ASP.NET Core Sınıf Kitaplığı'nda yeniden kullanılabilir Razor UI
author: Rick-Anderson
description: Yeniden kullanılabilir Razor kısmi görünümler, ASP.NET Core sınıf kitaplığında kullanarak kullanıcı Arabirimi oluşturma açıklanır.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 420cc54701394673e2b442b1fdf999e421820fd5
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809126"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="1a69c-103">ASP.NET Core 'de Razor Sınıf Kitaplığı projesini kullanarak yeniden kullanılabilir kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a69c-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="1a69c-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a69c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1a69c-105">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="1a69c-106">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="1a69c-107">Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="1a69c-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="1a69c-108">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="1a69c-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1a69c-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="1a69c-110">Razor UI içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a69c-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a69c-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a69c-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1a69c-112">Visual Studio 'dan **Yeni bir proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="1a69c-113">**Razor sınıfı kitaplığı** > **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="1a69c-114">Kitaplığı adlandırın (örneğin, "RazorClassLib"), > **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="1a69c-115">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1a69c-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="1a69c-116">Görünümleri desteketmeniz gerekiyorsa **destek sayfaları ve görünümleri '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="1a69c-117">Varsayılan olarak yalnızca Razor Pages desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="1a69c-118">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-118">Select **Create**.</span></span>

<span data-ttu-id="1a69c-119">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="1a69c-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="1a69c-120">**Destek sayfaları ve görünümleri** seçeneği sayfaları ve görünümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="1a69c-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a69c-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a69c-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a69c-122">Komut satırından çalıştırmak `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="1a69c-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="1a69c-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1a69c-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="1a69c-124">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="1a69c-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="1a69c-125">Sayfalar ve görünümler için destek sağlamak üzere `--support-pages-and-views` seçeneğini (`dotnet new razorclasslib --support-pages-and-views`) geçirin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="1a69c-126">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1a69c-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="1a69c-127">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1a69c-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="1a69c-128">Razor dosyaları için RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="1a69c-129">ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1a69c-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="1a69c-130">`~/Areas/Pages`yerine `~/Pages` içeriği kullanıma sunan bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="1a69c-131">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="1a69c-131">Reference RCL content</span></span>

<span data-ttu-id="1a69c-132">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="1a69c-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="1a69c-133">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="1a69c-133">NuGet package.</span></span> <span data-ttu-id="1a69c-134">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="1a69c-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="1a69c-135">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="1a69c-136">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="1a69c-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="1a69c-137">Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="1a69c-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="1a69c-138">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="1a69c-139">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="1a69c-140">Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="1a69c-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="1a69c-141">Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="1a69c-142">Yeni bir konum belirtmek için işaretleme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="1a69c-143">Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="1a69c-144">RCL sayfa düzeni</span><span class="sxs-lookup"><span data-stu-id="1a69c-144">RCL Pages layout</span></span>

<span data-ttu-id="1a69c-145">RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1a69c-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="1a69c-146">*RazorUIClassLib/sayfaları*</span><span class="sxs-lookup"><span data-stu-id="1a69c-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="1a69c-147">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="1a69c-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="1a69c-148">Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="1a69c-149">`<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1a69c-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="1a69c-150">Statik varlıklar içeren bir RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a69c-150">Create an RCL with static assets</span></span>

<span data-ttu-id="1a69c-151">RCL, RCL 'nin RCL veya tüketen uygulaması tarafından başvurulabilen, yardımcı statik varlıklar gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="1a69c-152">ASP.NET Core, tüketen bir uygulama tarafından kullanılabilen statik varlıkları içeren RCLs oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="1a69c-153">Yardımcı varlıkları RCL 'nin bir parçası olarak dahil etmek için, sınıf kitaplığında bir *Wwwroot* klasörü oluşturun ve gerekli dosyaları bu klasöre ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="1a69c-154">RCL 'yi paketleyerek, *Wwwroot* klasöründeki tüm yardımcı varlıklar pakete otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="1a69c-155">Statik varlıkları hariç tut</span><span class="sxs-lookup"><span data-stu-id="1a69c-155">Exclude static assets</span></span>

<span data-ttu-id="1a69c-156">Statik varlıkları dışlamak için, istenen dışlama yolunu proje dosyasındaki `$(DefaultItemExcludes)` özellik grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="1a69c-157">Girişleri noktalı virgülle ayırın (`;`).</span><span class="sxs-lookup"><span data-stu-id="1a69c-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="1a69c-158">Aşağıdaki örnekte, *Wwwroot* klasöründeki *lib. css* stil sayfası statik bir varlık olarak değerlendirilmez ve yayımlanan RCL 'ye dahil değildir:</span><span class="sxs-lookup"><span data-stu-id="1a69c-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="1a69c-159">TypeScript tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="1a69c-159">Typescript integration</span></span>

<span data-ttu-id="1a69c-160">TypeScript dosyalarını RCL 'ye eklemek için:</span><span class="sxs-lookup"><span data-stu-id="1a69c-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="1a69c-161">TypeScript dosyalarını ( *. TS*) *Wwwroot* klasörünün dışına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="1a69c-162">Örneğin, dosyaları bir *istemci* klasörüne yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="1a69c-163">*Wwwroot* klasörü için TypeScript derleme çıkışını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="1a69c-164">Proje dosyasındaki bir `PropertyGroup` içindeki `TypescriptOutDir` özelliğini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="1a69c-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="1a69c-165">Proje dosyasındaki bir `PropertyGroup` içine aşağıdaki hedefi ekleyerek TypeScript hedefini `ResolveCurrentProjectStaticWebAssets` hedefinin bir bağımlılığı olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1a69c-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="1a69c-166">Başvurulan bir RCL 'den içerik tüketme</span><span class="sxs-lookup"><span data-stu-id="1a69c-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="1a69c-167">RCL 'nin *Wwwroot* klasörüne eklenen dosyalar, ön ek `_content/{LIBRARY NAME}/`RCL veya tüketen uygulamaya sunulur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="1a69c-168">Örneğin, *Razor. Class. lib* adlı bir kitaplık `_content/Razor.Class.Lib/`statik içerik yolu ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="1a69c-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="1a69c-169">Bir NuGet paketi üretilirken ve derleme adı paket KIMLIĞIYLE aynı değilse, `{LIBRARY NAME}`için paket KIMLIĞINI kullanın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="1a69c-170">Tüketen uygulama, kitaplık tarafından `<script>`, `<style>`, `<img>`ve diğer HTML etiketleriyle sunulan statik varlıklara başvurur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="1a69c-171">Tüketim uygulaması `Startup.Configure`içinde [statik dosya desteğinin](xref:fundamentals/static-files) etkinleştirilmiş olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="1a69c-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="1a69c-172">Yapı çıktısından (`dotnet run`) tüketen uygulamayı çalıştırırken, varsayılan olarak, statik Web varlıkları geliştirme ortamında etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="1a69c-173">Yapı çıktısından çalışırken diğer ortamlardaki varlıkları desteklemek için, *program.cs*' deki konak oluşturucusu 'nda `UseStaticWebAssets` çağırın:</span><span class="sxs-lookup"><span data-stu-id="1a69c-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="1a69c-174">Yayımlanan çıktıdan bir uygulama çalıştırılırken çağrı `UseStaticWebAssets` gerekmez (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="1a69c-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="1a69c-175">Çoklu projeli geliştirme akışı</span><span class="sxs-lookup"><span data-stu-id="1a69c-175">Multi-project development flow</span></span>

<span data-ttu-id="1a69c-176">Kullanan uygulama şu şekilde çalışır:</span><span class="sxs-lookup"><span data-stu-id="1a69c-176">When the consuming app runs:</span></span>

* <span data-ttu-id="1a69c-177">RCL içindeki varlıklar özgün klasörlerinde kalır.</span><span class="sxs-lookup"><span data-stu-id="1a69c-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="1a69c-178">Varlıklar, tüketim uygulamasına taşınmaz.</span><span class="sxs-lookup"><span data-stu-id="1a69c-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="1a69c-179">RCL 'nin *Wwwroot* klasörü içindeki tüm değişiklikler, RCL yeniden oluşturulduktan ve tüketen uygulamayı yeniden oluşturmadan önce tüketen uygulamaya yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="1a69c-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="1a69c-180">RCL yapılandırıldığında, statik Web varlık konumlarını açıklayan bir bildirim oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="1a69c-181">Tüketen uygulama, başvurulan proje ve paketlerden varlıkları kullanmak için çalışma zamanında bildirimi okur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="1a69c-182">Bir RCL 'ye yeni bir varlık eklendiğinde, bir uygulamanın yeni varlığa erişebilmesi için bildirim güncellemek üzere RCL 'nin yeniden oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="1a69c-183">Yayımla</span><span class="sxs-lookup"><span data-stu-id="1a69c-183">Publish</span></span>

<span data-ttu-id="1a69c-184">Uygulama yayımlandığında, tüm başvurulan projeler ve paketlerin yardımcı varlıkları `_content/{LIBRARY NAME}/`altında Yayınlanan uygulamanın *Wwwroot* klasörüne kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="1a69c-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1a69c-185">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="1a69c-186">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="1a69c-187">Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="1a69c-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="1a69c-188">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="1a69c-189">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1a69c-189">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="1a69c-190">Razor UI içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a69c-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a69c-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a69c-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1a69c-192">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1a69c-193">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1a69c-194">(Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="1a69c-195">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1a69c-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="1a69c-196">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1a69c-197">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="1a69c-198">RCL aşağıdaki proje dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1a69c-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a69c-199">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a69c-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a69c-200">Komut satırından çalıştırmak `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="1a69c-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="1a69c-201">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1a69c-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="1a69c-202">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1a69c-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="1a69c-203">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1a69c-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="1a69c-204">Razor dosyaları için RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="1a69c-205">ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1a69c-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="1a69c-206">`~/Areas/Pages`yerine `~/Pages` içeriği kullanıma sunan bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="1a69c-207">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="1a69c-207">Reference RCL content</span></span>

<span data-ttu-id="1a69c-208">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="1a69c-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="1a69c-209">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="1a69c-209">NuGet package.</span></span> <span data-ttu-id="1a69c-210">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="1a69c-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="1a69c-211">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="1a69c-212">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="1a69c-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="1a69c-213">İzlenecek yol: bir Razor Pages projesinden bir RCL projesi oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="1a69c-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="1a69c-214">İndirebileceğiniz [tam proje](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ve yerine oluşturma test edin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-214">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="1a69c-215">Örnek indirme, ek kod ve test etmek proje kolaylaştırmak bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="1a69c-216">Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) yorumlarınızı üzerinde adım adım yönergeler ve örnekleri indirme ile.</span><span class="sxs-lookup"><span data-stu-id="1a69c-216">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="1a69c-217">İndirme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="1a69c-217">Test the download app</span></span>

<span data-ttu-id="1a69c-218">Tamamlanmış uygulamayı karşıdan henüz ve bunun yerine izlenecek projesi oluşturacak, atlamak [sonraki bölümde](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="1a69c-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a69c-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a69c-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a69c-220">Açık *.sln* dosyasını Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="1a69c-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="1a69c-221">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-221">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a69c-222">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a69c-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a69c-223">Bir komut isteminden *CLI* dizin RCL oluşturun ve web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="1a69c-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="1a69c-224">Taşı *WebApp1* dizin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1a69c-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="1a69c-225">Bölümündeki yönergeleri [Test WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="1a69c-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="1a69c-226">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a69c-226">Create an RCL</span></span>

<span data-ttu-id="1a69c-227">Bu bölümde bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-227">In this section, an RCL is created.</span></span> <span data-ttu-id="1a69c-228">Razor dosyaları için RCL eklenir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a69c-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a69c-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a69c-230">RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1a69c-230">Create the RCL project:</span></span>

* <span data-ttu-id="1a69c-231">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1a69c-232">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1a69c-233">> **Tamam**, uygulamayı **RazorUIClassLib** olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="1a69c-234">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1a69c-235">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="1a69c-236">Razor kısmi Görünüm adlı bir dosya ekleyin *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a69c-237">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a69c-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a69c-238">Komut satırından şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1a69c-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="1a69c-239">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="1a69c-239">The preceding commands:</span></span>

* <span data-ttu-id="1a69c-240">RCL `RazorUIClassLib` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="1a69c-241">Bir Razor il_eti sayfası oluşturur ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="1a69c-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="1a69c-242">`-np` Parametresi olmadan sayfa oluşturur bir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="1a69c-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="1a69c-243">Oluşturur bir [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) dosya ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="1a69c-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="1a69c-244">*_ViewStart.cshtml* dosyası (sonraki bölümde eklenir) Razor sayfaları proje düzenini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="1a69c-245">Razor dosyaları ve klasörleri projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="1a69c-246">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="1a69c-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="1a69c-247">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="1a69c-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="1a69c-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="1a69c-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="1a69c-249">Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="1a69c-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1a69c-250">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1a69c-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="1a69c-251">Daha fazla bilgi için *_viewımports.cshtml*, bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="1a69c-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="1a69c-252">Derleyici hata doğrulamak için sınıf kitaplığı derleme:</span><span class="sxs-lookup"><span data-stu-id="1a69c-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="1a69c-253">Derleme çıktısını içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="1a69c-254">*RazorUIClassLib.Views.dll* derlenmiş Razor içeriği.</span><span class="sxs-lookup"><span data-stu-id="1a69c-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="1a69c-255">Bir Razor sayfaları projeden Razor kullanıcı Arabirimi kitaplığını kullanma</span><span class="sxs-lookup"><span data-stu-id="1a69c-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a69c-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a69c-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a69c-257">Razor sayfaları web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1a69c-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="1a69c-258">**Çözüm Gezgini**> çözüme sağ tıklayıp **Yeni >proje** **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="1a69c-259">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1a69c-260">Uygulamayı adlandırma **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="1a69c-261">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1a69c-262">**Web uygulaması** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="1a69c-263">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **başlangıç projesi olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="1a69c-264">**Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **Proje bağımlılıkları**> **derleme bağımlılıkları** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="1a69c-265">Denetleme **RazorUIClassLib** bağımlılık olarak **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="1a69c-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="1a69c-266">**Çözüm Gezgini**, **WebApp1** ' a sağ tıklayıp > **başvuru** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="1a69c-267">**Başvuru Yöneticisi** Iletişim kutusunda **RazorUIClassLib** > **Tamam**' ı işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="1a69c-268">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-268">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a69c-269">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a69c-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a69c-270">Razor Pages uygulamasını ve RCL 'yi içeren bir Razor Pages Web uygulaması ve çözüm dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1a69c-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="1a69c-271">Web uygulaması derleyebilir ve:</span><span class="sxs-lookup"><span data-stu-id="1a69c-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="1a69c-272">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="1a69c-272">Test WebApp1</span></span>

<span data-ttu-id="1a69c-273">Razor UI sınıfı kitaplığının kullanımda olduğunu doğrulamak için `/MyFeature/Page1` gidin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="1a69c-274">Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="1a69c-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="1a69c-275">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1a69c-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="1a69c-276">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="1a69c-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="1a69c-277">Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="1a69c-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="1a69c-278">Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="1a69c-279">Yeni bir konum belirtmek için işaretleme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1a69c-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="1a69c-280">Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1a69c-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="1a69c-281">RCL sayfa düzeni</span><span class="sxs-lookup"><span data-stu-id="1a69c-281">RCL Pages layout</span></span>

<span data-ttu-id="1a69c-282">RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1a69c-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="1a69c-283">*RazorUIClassLib/sayfaları*</span><span class="sxs-lookup"><span data-stu-id="1a69c-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="1a69c-284">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="1a69c-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="1a69c-285">Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a69c-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="1a69c-286">`<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1a69c-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
