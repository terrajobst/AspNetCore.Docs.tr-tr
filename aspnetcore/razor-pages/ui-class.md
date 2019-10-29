---
title: ASP.NET Core ile sınıf kitaplıklarında yeniden kullanılabilir Razor Kullanıcı arabirimi
author: Rick-Anderson
description: ASP.NET Core bir sınıf kitaplığında kısmi görünümler kullanarak yeniden kullanılabilir Razor Kullanıcı arabirimi oluşturmayı açıklar.
ms.author: riande
ms.date: 10/26/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: ff12eea5406c4f5392a466728741000e3dd16fc1
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034227"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="1ba7a-103">ASP.NET Core 'de Razor Sınıf Kitaplığı projesini kullanarak yeniden kullanılabilir kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ba7a-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="1ba7a-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="1ba7a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1ba7a-105">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="1ba7a-106">RCL paketlenebilir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="1ba7a-107">Uygulamalar RCL 'yi içerebilir ve içerdiği görünümleri ve sayfaları geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="1ba7a-108">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="1ba7a-109">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ba7a-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="1ba7a-110">Razor Kullanıcı arabirimi içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ba7a-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ba7a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ba7a-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1ba7a-112">Visual Studio 'dan **Yeni bir proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="1ba7a-113">**Razor sınıfı kitaplığı** > **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="1ba7a-114">Kitaplığı adlandırın (örneğin, "RazorClassLib"), > **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="1ba7a-115">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="1ba7a-116">Görünümleri desteketmeniz gerekiyorsa **destek sayfaları ve görünümleri '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="1ba7a-117">Varsayılan olarak yalnızca Razor Pages desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="1ba7a-118">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-118">Select **Create**.</span></span>

<span data-ttu-id="1ba7a-119">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="1ba7a-120">**Destek sayfaları ve görünümleri** seçeneği sayfaları ve görünümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ba7a-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ba7a-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ba7a-122">Komut satırından `dotnet new razorclasslib` ' ı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="1ba7a-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="1ba7a-124">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="1ba7a-125">Sayfalar ve görünümler için destek sağlamak üzere `--support-pages-and-views` seçeneğini (`dotnet new razorclasslib --support-pages-and-views`) geçirin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="1ba7a-126">Daha fazla bilgi için bkz. [DotNet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="1ba7a-127">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="1ba7a-128">RCL 'ye Razor dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="1ba7a-129">ASP.NET Core şablonları RCL içeriğinin *Areas* klasöründe olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="1ba7a-130">`~/Areas/Pages`yerine `~/Pages` içeriği kullanıma sunan bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="1ba7a-131">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="1ba7a-131">Reference RCL content</span></span>

<span data-ttu-id="1ba7a-132">RCL 'ye şu şekilde başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="1ba7a-133">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-133">NuGet package.</span></span> <span data-ttu-id="1ba7a-134">Bkz. [NuGet paketleri oluşturma](/nuget/create-packages/creating-a-package) ve [DotNet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [bir NuGet paketi oluşturma ve yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="1ba7a-135">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="1ba7a-136">Bkz. [DotNet-başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="1ba7a-137">Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="1ba7a-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="1ba7a-138">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="1ba7a-139">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="1ba7a-140">Örnek indirme içinde *WebApp1/Areas/MyFeature2* öğesini *WebApp1/Areas/myfeature* olarak yeniden adlandırın ve test önceliğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="1ba7a-141">*RazorUIClassLib/Areas/myfeature/Pages/Shared/_Message. cshtml* kısmi görünümünü *WebApp1/Areas/Myfeature/Pages/Shared/_Message. cshtml*'ye kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="1ba7a-142">Biçimlendirmeyi yeni konumu belirtecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="1ba7a-143">Uygulamanın kısmi sürümünün kullanılmakta olduğunu doğrulamak için uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="1ba7a-144">RCL sayfaları düzeni</span><span class="sxs-lookup"><span data-stu-id="1ba7a-144">RCL Pages layout</span></span>

<span data-ttu-id="1ba7a-145">RCL içeriğine, Web uygulamasının *Sayfalar* klasörünün bir parçası olmasına rağmen başvurmak için, aşağıdaki dosya yapısıyla RCL projesini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="1ba7a-146">*RazorUIClassLib/sayfalar*</span><span class="sxs-lookup"><span data-stu-id="1ba7a-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="1ba7a-147">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="1ba7a-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="1ba7a-148">*RazorUIClassLib/Pages/Shared* iki kısmi dosya Içerir: *_header. cshtml* ve *_footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="1ba7a-149">`<partial>` Etiketler *_Layout. cshtml* dosyasına eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="1ba7a-150">Statik varlıklar içeren bir RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ba7a-150">Create an RCL with static assets</span></span>

<span data-ttu-id="1ba7a-151">RCL, RCL 'nin tüketen uygulaması tarafından başvurulabilen, yardımcı statik varlıklar gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-151">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="1ba7a-152">ASP.NET Core, tüketen bir uygulama tarafından kullanılabilen statik varlıkları içeren RCLs oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="1ba7a-153">Yardımcı varlıkları RCL 'nin bir parçası olarak dahil etmek için, sınıf kitaplığında bir *Wwwroot* klasörü oluşturun ve gerekli dosyaları bu klasöre ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="1ba7a-154">RCL 'yi paketleyerek, *Wwwroot* klasöründeki tüm yardımcı varlıklar pakete otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="1ba7a-155">Statik varlıkları hariç tut</span><span class="sxs-lookup"><span data-stu-id="1ba7a-155">Exclude static assets</span></span>

<span data-ttu-id="1ba7a-156">Statik varlıkları dışlamak için, istenen dışlama yolunu proje dosyasındaki `$(DefaultItemExcludes)` özellik grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="1ba7a-157">Girişleri noktalı virgülle ayırın (`;`).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="1ba7a-158">Aşağıdaki örnekte, *Wwwroot* klasöründeki *lib. css* stil sayfası statik bir varlık olarak değerlendirilmez ve yayımlanan RCL 'ye dahil değildir:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="1ba7a-159">TypeScript tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="1ba7a-159">Typescript integration</span></span>

<span data-ttu-id="1ba7a-160">TypeScript dosyalarını RCL 'ye eklemek için:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="1ba7a-161">TypeScript dosyalarını ( *. TS*) *Wwwroot* klasörünün dışına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="1ba7a-162">Örneğin, dosyaları bir *istemci* klasörüne yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="1ba7a-163">*Wwwroot* klasörü için TypeScript derleme çıkışını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="1ba7a-164">Proje dosyasında bir `PropertyGroup` içinde `TypescriptOutDir` özelliğini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="1ba7a-165">Proje dosyasında bir `PropertyGroup` içine aşağıdaki hedefi ekleyerek TypeScript hedefini `ResolveCurrentProjectStaticWebAssets` hedefinin bir bağımlılığı olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="1ba7a-166">Başvurulan bir RCL 'den içerik tüketme</span><span class="sxs-lookup"><span data-stu-id="1ba7a-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="1ba7a-167">RCL 'nin *Wwwroot* klasörüne eklenen dosyalar, `_content/{LIBRARY NAME}/`önek altında tüketen uygulamaya sunulur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-167">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="1ba7a-168">Örneğin, *Razor. Class. lib* adlı bir kitaplık `_content/Razor.Class.Lib/`statik içerik yolu ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="1ba7a-169">Kullanan uygulama, kitaplık tarafından `<script>`, `<style>`, `<img>` ve diğer HTML etiketleriyle sunulan statik varlıklara başvurur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-169">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="1ba7a-170">Tüketim uygulaması `Startup.Configure` ' de [statik dosya desteğinin](xref:fundamentals/static-files) etkin olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-170">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="1ba7a-171">Yapı çıktısından tüketen uygulamayı çalıştırırken (`dotnet run`), statik Web varlıkları geliştirme ortamında varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-171">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="1ba7a-172">Derleme çıktılarından çalışırken diğer ortamlardaki varlıkları desteklemek için, *program.cs*' deki konak oluşturucusu 'nda `UseStaticWebAssets` ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-172">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="1ba7a-173">Yayımlanan çıktıdan bir uygulama çalıştırılırken çağrı `UseStaticWebAssets` gerekmez (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-173">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="1ba7a-174">Çoklu projeli geliştirme akışı</span><span class="sxs-lookup"><span data-stu-id="1ba7a-174">Multi-project development flow</span></span>

<span data-ttu-id="1ba7a-175">Kullanan uygulama şu şekilde çalışır:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-175">When the consuming app runs:</span></span>

* <span data-ttu-id="1ba7a-176">RCL içindeki varlıklar özgün klasörlerinde kalır.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-176">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="1ba7a-177">Varlıklar, tüketim uygulamasına taşınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-177">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="1ba7a-178">RCL 'nin *Wwwroot* klasörü içindeki tüm değişiklikler, RCL yeniden oluşturulduktan ve tüketen uygulamayı yeniden oluşturmadan önce tüketen uygulamaya yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-178">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="1ba7a-179">RCL yapılandırıldığında, statik Web varlık konumlarını açıklayan bir bildirim oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-179">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="1ba7a-180">Tüketen uygulama, başvurulan proje ve paketlerden varlıkları kullanmak için çalışma zamanında bildirimi okur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-180">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="1ba7a-181">Bir RCL 'ye yeni bir varlık eklendiğinde, bir uygulamanın yeni varlığa erişebilmesi için bildirim güncellemek üzere RCL 'nin yeniden oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-181">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="1ba7a-182">Yayınlamanız</span><span class="sxs-lookup"><span data-stu-id="1ba7a-182">Publish</span></span>

<span data-ttu-id="1ba7a-183">Uygulama yayımlandığında, tüm başvurulan projeler ve paketlerin yardımcı varlıkları `_content/{LIBRARY NAME}/`altında Yayınlanan uygulamanın *Wwwroot* klasörüne kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-183">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1ba7a-184">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-184">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="1ba7a-185">RCL paketlenebilir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-185">The RCL can be packaged and reused.</span></span> <span data-ttu-id="1ba7a-186">Uygulamalar RCL 'yi içerebilir ve içerdiği görünümleri ve sayfaları geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-186">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="1ba7a-187">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-187">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="1ba7a-188">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ba7a-188">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="1ba7a-189">Razor Kullanıcı arabirimi içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ba7a-189">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ba7a-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ba7a-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1ba7a-191">Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-191">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1ba7a-192">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-192">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1ba7a-193">Kitaplığı adlandırın (örneğin, "RazorClassLib") > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-193">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="1ba7a-194">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-194">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="1ba7a-195">**ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-195">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1ba7a-196">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-196">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="1ba7a-197">RCL aşağıdaki proje dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-197">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ba7a-198">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ba7a-198">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ba7a-199">Komut satırından `dotnet new razorclasslib` ' ı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-199">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="1ba7a-200">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-200">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="1ba7a-201">Daha fazla bilgi için bkz. [DotNet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-201">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="1ba7a-202">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-202">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="1ba7a-203">RCL 'ye Razor dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-203">Add Razor files to the RCL.</span></span>

<span data-ttu-id="1ba7a-204">ASP.NET Core şablonları RCL içeriğinin *Areas* klasöründe olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-204">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="1ba7a-205">`~/Areas/Pages`yerine `~/Pages` içeriği kullanıma sunan bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-205">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="1ba7a-206">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="1ba7a-206">Reference RCL content</span></span>

<span data-ttu-id="1ba7a-207">RCL 'ye şu şekilde başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-207">The RCL can be referenced by:</span></span>

* <span data-ttu-id="1ba7a-208">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-208">NuGet package.</span></span> <span data-ttu-id="1ba7a-209">Bkz. [NuGet paketleri oluşturma](/nuget/create-packages/creating-a-package) ve [DotNet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [bir NuGet paketi oluşturma ve yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-209">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="1ba7a-210">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-210">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="1ba7a-211">Bkz. [DotNet-başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-211">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="1ba7a-212">İzlenecek yol: bir Razor Pages projesinden bir RCL projesi oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="1ba7a-212">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="1ba7a-213">[Tüm projeyi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) indirebilir ve oluşturmak yerine test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-213">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="1ba7a-214">Örnek indirme, projenin test olmasını kolaylaştıran ek kod ve bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-214">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="1ba7a-215">Yükleme örnekleri ve adım adım yönergeler hakkındaki açıklamalarınızla [Bu GitHub sorunuyla](https://github.com/aspnet/AspNetCore.Docs/issues/6098) ilgili geri bildirimde bulunun.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-215">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="1ba7a-216">İndirme uygulamasını test etme</span><span class="sxs-lookup"><span data-stu-id="1ba7a-216">Test the download app</span></span>

<span data-ttu-id="1ba7a-217">Tamamlanmış uygulamayı indirmediyseniz ve gözden geçirme projesi oluşturmak istiyorsanız, [sonraki bölüme](#create-an-rcl)atlayın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-217">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ba7a-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ba7a-218">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ba7a-219">Visual Studio 'da *. sln* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-219">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="1ba7a-220">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-220">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ba7a-221">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ba7a-221">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ba7a-222">*CLI* dizinindeki bir komut isteminden RCL ve Web uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-222">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="1ba7a-223">*WebApp1* dizinine gidin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-223">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="1ba7a-224">[Test WebApp1](#test-webapp1) içindeki yönergeleri izleyin</span><span class="sxs-lookup"><span data-stu-id="1ba7a-224">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="1ba7a-225">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ba7a-225">Create an RCL</span></span>

<span data-ttu-id="1ba7a-226">Bu bölümde bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-226">In this section, an RCL is created.</span></span> <span data-ttu-id="1ba7a-227">Razor dosyaları RCL 'ye eklenir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-227">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ba7a-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ba7a-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ba7a-229">RCL projesini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-229">Create the RCL project:</span></span>

* <span data-ttu-id="1ba7a-230">Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-230">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1ba7a-231">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-231">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1ba7a-232">> **Tamam**, uygulamayı **RazorUIClassLib** olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-232">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="1ba7a-233">**ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-233">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1ba7a-234">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-234">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="1ba7a-235">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message. cshtml*adlı bir Razor kısmi görünüm dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-235">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ba7a-236">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ba7a-236">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ba7a-237">Komut satırından aşağıdakileri çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-237">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="1ba7a-238">Önceki komutlar:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-238">The preceding commands:</span></span>

* <span data-ttu-id="1ba7a-239">RCL `RazorUIClassLib` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-239">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="1ba7a-240">Bir Razor _Ileti sayfası oluşturur ve RCL 'ye ekler.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-240">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="1ba7a-241">`-np` parametresi, `PageModel`olmadan sayfayı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-241">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="1ba7a-242">Bir [_Viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) dosyası oluşturur ve RCL 'ye ekler.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-242">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="1ba7a-243">*_Viewstart. cshtml* dosyası, Razor Pages projenin (bir sonraki bölüme eklenen) düzeninin yerleşimini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-243">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="1ba7a-244">Projeye Razor dosyaları ve klasörleri ekleme</span><span class="sxs-lookup"><span data-stu-id="1ba7a-244">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="1ba7a-245">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message. cshtml* içindeki biçimlendirmeyi aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="1ba7a-246">*RazorUIClassLib/Areas/MyFeature/Pages/Sayfa1. cshtml* içindeki biçimlendirmeyi aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="1ba7a-247">kısmi görünümü kullanmak için `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="1ba7a-247">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="1ba7a-248">`@addTagHelper` yönergesini dahil etmek yerine, *_Viewwimports. cshtml* dosyası ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-248">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1ba7a-249">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-249">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="1ba7a-250">*_Viewwimports. cshtml*hakkında daha fazla bilgi için bkz. [paylaşılan yönergeleri içeri aktarma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="1ba7a-250">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="1ba7a-251">Derleyici hatası olmadığını doğrulamak için sınıf kitaplığı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-251">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="1ba7a-252">Derleme çıkışı *RazorUIClassLib. dll* ve *RazorUIClassLib. views. dll*içerir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-252">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="1ba7a-253">*RazorUIClassLib. views. dll* derlenen Razor içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-253">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="1ba7a-254">Razor Pages projesinden Razor Kullanıcı arabirimi kitaplığını kullanma</span><span class="sxs-lookup"><span data-stu-id="1ba7a-254">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ba7a-255">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ba7a-255">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ba7a-256">Razor Pages Web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-256">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="1ba7a-257">**Çözüm Gezgini**>**yeni proje** **eklemek** > çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-257">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="1ba7a-258">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-258">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1ba7a-259">Uygulamayı **WebApp1**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-259">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="1ba7a-260">**ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-260">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1ba7a-261">**Web uygulaması** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-261">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="1ba7a-262">**Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **Başlangıç projesi olarak ayarla**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-262">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="1ba7a-263">**Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **derleme bağımlılıkları** > **Proje bağımlılıkları**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-263">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="1ba7a-264">**WebApp1**'in bağımlılığı olarak **RazorUIClassLib** denetleyin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-264">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="1ba7a-265">**Çözüm Gezgini**, **WebApp1** ' a sağ tıklayıp > **başvurusu** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-265">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="1ba7a-266">**Başvuru Yöneticisi** Iletişim kutusunda **RazorUIClassLib** > **Tamam**' ı işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-266">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="1ba7a-267">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-267">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ba7a-268">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1ba7a-268">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ba7a-269">Razor Pages uygulamasını ve RCL 'yi içeren bir Razor Pages Web uygulaması ve çözüm dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-269">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="1ba7a-270">Web uygulamasını derleyin ve çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-270">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="1ba7a-271">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="1ba7a-271">Test WebApp1</span></span>

<span data-ttu-id="1ba7a-272">Razor UI sınıfı kitaplığının kullanımda olduğunu doğrulamak için `/MyFeature/Page1` ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-272">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="1ba7a-273">Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="1ba7a-273">Override views, partial views, and pages</span></span>

<span data-ttu-id="1ba7a-274">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-274">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="1ba7a-275">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-275">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="1ba7a-276">Örnek indirme içinde *WebApp1/Areas/MyFeature2* öğesini *WebApp1/Areas/myfeature* olarak yeniden adlandırın ve test önceliğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-276">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="1ba7a-277">*RazorUIClassLib/Areas/myfeature/Pages/Shared/_Message. cshtml* kısmi görünümünü *WebApp1/Areas/Myfeature/Pages/Shared/_Message. cshtml*'ye kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-277">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="1ba7a-278">Biçimlendirmeyi yeni konumu belirtecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-278">Update the markup to indicate the new location.</span></span> <span data-ttu-id="1ba7a-279">Uygulamanın kısmi sürümünün kullanılmakta olduğunu doğrulamak için uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-279">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="1ba7a-280">RCL sayfaları düzeni</span><span class="sxs-lookup"><span data-stu-id="1ba7a-280">RCL Pages layout</span></span>

<span data-ttu-id="1ba7a-281">RCL içeriğine, Web uygulamasının *Sayfalar* klasörünün bir parçası olmasına rağmen başvurmak için, aşağıdaki dosya yapısıyla RCL projesini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-281">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="1ba7a-282">*RazorUIClassLib/sayfalar*</span><span class="sxs-lookup"><span data-stu-id="1ba7a-282">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="1ba7a-283">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="1ba7a-283">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="1ba7a-284">*RazorUIClassLib/Pages/Shared* iki kısmi dosya Içerir: *_header. cshtml* ve *_footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1ba7a-284">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="1ba7a-285">`<partial>` Etiketler *_Layout. cshtml* dosyasına eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="1ba7a-285">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
