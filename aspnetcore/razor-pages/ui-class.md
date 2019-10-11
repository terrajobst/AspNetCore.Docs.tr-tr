---
title: ASP.NET Core ile sınıf kitaplıklarında yeniden kullanılabilir Razor Kullanıcı arabirimi
author: Rick-Anderson
description: ASP.NET Core bir sınıf kitaplığında kısmi görünümler kullanarak yeniden kullanılabilir Razor Kullanıcı arabirimi oluşturmayı açıklar.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: dcd24f7dafd198f88cdf84d1ab67c84f45428a95
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179328"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="21c14-103">ASP.NET Core 'de Razor Sınıf Kitaplığı projesini kullanarak yeniden kullanılabilir kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="21c14-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="21c14-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21c14-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="21c14-105">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="21c14-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="21c14-106">RCL paketlenebilir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="21c14-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="21c14-107">Uygulamalar RCL 'yi içerebilir ve içerdiği görünümleri ve sayfaları geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="21c14-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="21c14-108">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="21c14-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="21c14-109">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="21c14-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="21c14-110">Razor Kullanıcı arabirimi içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="21c14-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="21c14-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21c14-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="21c14-112">Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="21c14-113">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="21c14-114">Kitaplığı adlandırın (örneğin, "RazorClassLib") > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="21c14-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="21c14-115">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="21c14-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="21c14-116">**ASP.NET Core 3,0** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="21c14-117">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="21c14-118">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="21c14-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="21c14-119">Visual Studio 'daki bir şablon seçeneği, sayfalar ve görünümler için şablon desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="21c14-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="21c14-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="21c14-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="21c14-121">Komut satırından `dotnet new razorclasslib` ' ı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="21c14-122">Örnek:</span><span class="sxs-lookup"><span data-stu-id="21c14-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="21c14-123">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="21c14-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="21c14-124">Sayfalar ve görünümler için destek sağlamak üzere `--support-pages-and-views` seçeneğini (`dotnet new razorclasslib --support-pages-and-views`) geçirin.</span><span class="sxs-lookup"><span data-stu-id="21c14-124">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="21c14-125">Daha fazla bilgi için bkz. [DotNet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="21c14-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="21c14-126">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="21c14-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="21c14-127">RCL 'ye Razor dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="21c14-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="21c14-128">ASP.NET Core şablonları RCL içeriğinin *Areas* klasöründe olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="21c14-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="21c14-129">@No__t-2 yerine `~/Pages` ' de içerik açığa çıkaran bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.</span><span class="sxs-lookup"><span data-stu-id="21c14-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="21c14-130">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="21c14-130">Reference RCL content</span></span>

<span data-ttu-id="21c14-131">RCL 'ye şu şekilde başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="21c14-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="21c14-132">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="21c14-132">NuGet package.</span></span> <span data-ttu-id="21c14-133">Bkz. [NuGet paketleri oluşturma](/nuget/create-packages/creating-a-package) ve [DotNet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [bir NuGet paketi oluşturma ve yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="21c14-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="21c14-134">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="21c14-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="21c14-135">Bkz. [DotNet-başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="21c14-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="21c14-136">Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="21c14-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="21c14-137">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="21c14-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="21c14-138">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="21c14-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="21c14-139">Örnek indirme içinde *WebApp1/Areas/MyFeature2* öğesini *WebApp1/Areas/myfeature* olarak yeniden adlandırın ve test önceliğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="21c14-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="21c14-140">*RazorUIClassLib/Areas/myfeature/Pages/Shared/_Message. cshtml* kısmi görünümünü *WebApp1/Areas/Myfeature/Pages/Shared/_Message. cshtml*'ye kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="21c14-141">Biçimlendirmeyi yeni konumu belirtecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="21c14-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="21c14-142">Uygulamanın kısmi sürümünün kullanılmakta olduğunu doğrulamak için uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="21c14-143">RCL sayfaları düzeni</span><span class="sxs-lookup"><span data-stu-id="21c14-143">RCL Pages layout</span></span>

<span data-ttu-id="21c14-144">RCL içeriğine, Web uygulamasının *Sayfalar* klasörünün bir parçası olmasına rağmen başvurmak için, aşağıdaki dosya yapısıyla RCL projesini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="21c14-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="21c14-145">*RazorUIClassLib/sayfalar*</span><span class="sxs-lookup"><span data-stu-id="21c14-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="21c14-146">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="21c14-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="21c14-147">*RazorUIClassLib/Pages/Shared* iki kısmi dosya Içerir: *_header. cshtml* ve *_footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="21c14-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="21c14-148">@No__t-0 etiketleri *_Layout. cshtml* dosyasına eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="21c14-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="21c14-149">Statik varlıklar içeren bir RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="21c14-149">Create an RCL with static assets</span></span>

<span data-ttu-id="21c14-150">RCL, RCL 'nin tüketen uygulaması tarafından başvurulabilen, yardımcı statik varlıklar gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="21c14-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="21c14-151">ASP.NET Core, tüketen bir uygulama tarafından kullanılabilen statik varlıkları içeren RCLs oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="21c14-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="21c14-152">Yardımcı varlıkları RCL 'nin bir parçası olarak dahil etmek için, sınıf kitaplığında bir *Wwwroot* klasörü oluşturun ve gerekli dosyaları bu klasöre ekleyin.</span><span class="sxs-lookup"><span data-stu-id="21c14-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="21c14-153">RCL 'yi paketleyerek, *Wwwroot* klasöründeki tüm yardımcı varlıklar pakete otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="21c14-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="21c14-154">Statik varlıkları hariç tut</span><span class="sxs-lookup"><span data-stu-id="21c14-154">Exclude static assets</span></span>

<span data-ttu-id="21c14-155">Statik varlıkları dışlamak için, istenen dışlama yolunu proje dosyasındaki `$(DefaultItemExcludes)` özellik grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="21c14-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="21c14-156">Girişleri noktalı virgülle ayırın (`;`).</span><span class="sxs-lookup"><span data-stu-id="21c14-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="21c14-157">Aşağıdaki örnekte, *Wwwroot* klasöründeki *lib. css* stil sayfası statik bir varlık olarak değerlendirilmez ve yayımlanan RCL 'ye dahil değildir:</span><span class="sxs-lookup"><span data-stu-id="21c14-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="21c14-158">TypeScript tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="21c14-158">Typescript integration</span></span>

<span data-ttu-id="21c14-159">TypeScript dosyalarını RCL 'ye eklemek için:</span><span class="sxs-lookup"><span data-stu-id="21c14-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="21c14-160">TypeScript dosyalarını ( *. TS*) *Wwwroot* klasörünün dışına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="21c14-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="21c14-161">Örneğin, dosyaları bir *istemci* klasörüne yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="21c14-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="21c14-162">*Wwwroot* klasörü için TypeScript derleme çıkışını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="21c14-163">Proje dosyasında bir `PropertyGroup` içinde `TypescriptOutDir` özelliğini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="21c14-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="21c14-164">Proje dosyasında bir `PropertyGroup` içine aşağıdaki hedefi ekleyerek TypeScript hedefini `ResolveCurrentProjectStaticWebAssets` hedefinin bir bağımlılığı olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="21c14-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
  <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
    CompileTypeScript;
    $(ResolveCurrentProjectStaticWebAssetsInputs)
  </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="21c14-165">Başvurulan bir RCL 'den içerik tüketme</span><span class="sxs-lookup"><span data-stu-id="21c14-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="21c14-166">RCL 'nin *Wwwroot* klasörüne eklenen dosyalar, `_content/{LIBRARY NAME}/` öneki altında tüketen uygulamaya sunulur.</span><span class="sxs-lookup"><span data-stu-id="21c14-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="21c14-167">Örneğin, *Razor. Class. lib* adlı bir kitaplık `_content/Razor.Class.Lib/` ' de statik içerik yolu ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="21c14-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="21c14-168">Kullanan uygulama, kitaplık tarafından `<script>`, `<style>`, `<img>` ve diğer HTML etiketleriyle sunulan statik varlıklara başvurur.</span><span class="sxs-lookup"><span data-stu-id="21c14-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="21c14-169">Tüketim uygulaması `Startup.Configure` ' de [statik dosya desteğinin](xref:fundamentals/static-files) etkin olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="21c14-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="21c14-170">Yapı çıktısından tüketen uygulamayı çalıştırırken (`dotnet run`), statik Web varlıkları geliştirme ortamında varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="21c14-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="21c14-171">Derleme çıktılarından çalışırken diğer ortamlardaki varlıkları desteklemek için, *program.cs*' deki konak oluşturucusu 'nda `UseStaticWebAssets` ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="21c14-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="21c14-172">(@No__t-1) yayımlanmış çıkışdan bir uygulama çalıştırılırken, @no__t çağrısı yapılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="21c14-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="21c14-173">Çoklu projeli geliştirme akışı</span><span class="sxs-lookup"><span data-stu-id="21c14-173">Multi-project development flow</span></span>

<span data-ttu-id="21c14-174">Kullanan uygulama şu şekilde çalışır:</span><span class="sxs-lookup"><span data-stu-id="21c14-174">When the consuming app runs:</span></span>

* <span data-ttu-id="21c14-175">RCL içindeki varlıklar özgün klasörlerinde kalır.</span><span class="sxs-lookup"><span data-stu-id="21c14-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="21c14-176">Varlıklar, tüketim uygulamasına taşınmaz.</span><span class="sxs-lookup"><span data-stu-id="21c14-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="21c14-177">RCL 'nin *Wwwroot* klasörü içindeki tüm değişiklikler, RCL yeniden oluşturulduktan ve tüketen uygulamayı yeniden oluşturmadan önce tüketen uygulamaya yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="21c14-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="21c14-178">RCL yapılandırıldığında, statik Web varlık konumlarını açıklayan bir bildirim oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="21c14-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="21c14-179">Tüketen uygulama, başvurulan proje ve paketlerden varlıkları kullanmak için çalışma zamanında bildirimi okur.</span><span class="sxs-lookup"><span data-stu-id="21c14-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="21c14-180">Bir RCL 'ye yeni bir varlık eklendiğinde, bir uygulamanın yeni varlığa erişebilmesi için bildirim güncellemek üzere RCL 'nin yeniden oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="21c14-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="21c14-181">Yayımlama</span><span class="sxs-lookup"><span data-stu-id="21c14-181">Publish</span></span>

<span data-ttu-id="21c14-182">Uygulama yayımlandığında, tüm başvurulan projeler ve paketlerin yardımcı varlıkları `_content/{LIBRARY NAME}/` altında yayımlanan uygulamanın *Wwwroot* klasörüne kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="21c14-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="21c14-183">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="21c14-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="21c14-184">RCL paketlenebilir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="21c14-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="21c14-185">Uygulamalar RCL 'yi içerebilir ve içerdiği görünümleri ve sayfaları geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="21c14-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="21c14-186">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="21c14-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="21c14-187">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="21c14-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="21c14-188">Razor Kullanıcı arabirimi içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="21c14-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="21c14-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21c14-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="21c14-190">Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="21c14-191">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="21c14-192">Kitaplığı adlandırın (örneğin, "RazorClassLib") > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="21c14-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="21c14-193">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="21c14-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="21c14-194">**ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="21c14-195">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="21c14-196">RCL aşağıdaki proje dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="21c14-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="21c14-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="21c14-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="21c14-198">Komut satırından `dotnet new razorclasslib` ' ı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="21c14-199">Örnek:</span><span class="sxs-lookup"><span data-stu-id="21c14-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="21c14-200">Daha fazla bilgi için bkz. [DotNet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="21c14-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="21c14-201">Oluşturulan görünüm kitaplığıyla bir dosya adı çarpışmasını önlemek için, kitaplık adının `.Views` ' da bitmediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="21c14-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="21c14-202">RCL 'ye Razor dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="21c14-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="21c14-203">ASP.NET Core şablonları RCL içeriğinin *Areas* klasöründe olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="21c14-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="21c14-204">@No__t-2 yerine `~/Pages` ' de içerik açığa çıkaran bir RCL oluşturmak için [RCL Pages düzenine](#rcl-pages-layout) bakın.</span><span class="sxs-lookup"><span data-stu-id="21c14-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="21c14-205">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="21c14-205">Reference RCL content</span></span>

<span data-ttu-id="21c14-206">RCL 'ye şu şekilde başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="21c14-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="21c14-207">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="21c14-207">NuGet package.</span></span> <span data-ttu-id="21c14-208">Bkz. [NuGet paketleri oluşturma](/nuget/create-packages/creating-a-package) ve [DotNet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [bir NuGet paketi oluşturma ve yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="21c14-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="21c14-209">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="21c14-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="21c14-210">Bkz. [DotNet-başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="21c14-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="21c14-211">İzlenecek yol: bir Razor Pages projesinden bir RCL projesi oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="21c14-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="21c14-212">[Tüm projeyi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) indirebilir ve oluşturmak yerine test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21c14-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="21c14-213">Örnek indirme, projenin test olmasını kolaylaştıran ek kod ve bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="21c14-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="21c14-214">Yükleme örnekleri ve adım adım yönergeler hakkındaki açıklamalarınızla [Bu GitHub sorunuyla](https://github.com/aspnet/AspNetCore.Docs/issues/6098) ilgili geri bildirimde bulunun.</span><span class="sxs-lookup"><span data-stu-id="21c14-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="21c14-215">İndirme uygulamasını test etme</span><span class="sxs-lookup"><span data-stu-id="21c14-215">Test the download app</span></span>

<span data-ttu-id="21c14-216">Tamamlanmış uygulamayı indirmediyseniz ve gözden geçirme projesi oluşturmak istiyorsanız, [sonraki bölüme](#create-an-rcl)atlayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="21c14-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21c14-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="21c14-218">Visual Studio 'da *. sln* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="21c14-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="21c14-219">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="21c14-220">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="21c14-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="21c14-221">*CLI* dizinindeki bir komut isteminden RCL ve Web uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="21c14-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="21c14-222">*WebApp1* dizinine gidin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="21c14-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="21c14-223">[Test WebApp1](#test-webapp1) içindeki yönergeleri izleyin</span><span class="sxs-lookup"><span data-stu-id="21c14-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="21c14-224">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="21c14-224">Create an RCL</span></span>

<span data-ttu-id="21c14-225">Bu bölümde bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="21c14-225">In this section, an RCL is created.</span></span> <span data-ttu-id="21c14-226">Razor dosyaları RCL 'ye eklenir.</span><span class="sxs-lookup"><span data-stu-id="21c14-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="21c14-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21c14-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="21c14-228">RCL projesini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="21c14-228">Create the RCL project:</span></span>

* <span data-ttu-id="21c14-229">Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="21c14-230">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="21c14-231">@No__t **-1 '** i uygulama **RazorUIClassLib** olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="21c14-232">**ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="21c14-233">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="21c14-234">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message. cshtml*adlı bir Razor kısmi görünüm dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="21c14-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="21c14-235">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="21c14-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="21c14-236">Komut satırından aşağıdakileri çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="21c14-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="21c14-237">Önceki komutlar:</span><span class="sxs-lookup"><span data-stu-id="21c14-237">The preceding commands:</span></span>

* <span data-ttu-id="21c14-238">@No__t-0 RCL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="21c14-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="21c14-239">Bir Razor _Ileti sayfası oluşturur ve RCL 'ye ekler.</span><span class="sxs-lookup"><span data-stu-id="21c14-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="21c14-240">@No__t-0 parametresi, `PageModel` olmadan sayfayı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="21c14-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="21c14-241">Bir [_Viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) dosyası oluşturur ve RCL 'ye ekler.</span><span class="sxs-lookup"><span data-stu-id="21c14-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="21c14-242">*_Viewstart. cshtml* dosyası, Razor Pages projenin (bir sonraki bölüme eklenen) düzeninin yerleşimini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="21c14-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="21c14-243">Projeye Razor dosyaları ve klasörleri ekleme</span><span class="sxs-lookup"><span data-stu-id="21c14-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="21c14-244">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message. cshtml* içindeki biçimlendirmeyi aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="21c14-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="21c14-245">*RazorUIClassLib/Areas/MyFeature/Pages/Sayfa1. cshtml* içindeki biçimlendirmeyi aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="21c14-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="21c14-246">kısmi görünümü kullanmak için `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="21c14-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="21c14-247">@No__t-0 yönergesini dahil etmek yerine, *_Viewwimports. cshtml* dosyası ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21c14-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="21c14-248">Örnek:</span><span class="sxs-lookup"><span data-stu-id="21c14-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="21c14-249">*_Viewwimports. cshtml*hakkında daha fazla bilgi için bkz. [paylaşılan yönergeleri içeri aktarma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="21c14-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="21c14-250">Derleyici hatası olmadığını doğrulamak için sınıf kitaplığı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="21c14-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="21c14-251">Derleme çıkışı *RazorUIClassLib. dll* ve *RazorUIClassLib. views. dll*içerir.</span><span class="sxs-lookup"><span data-stu-id="21c14-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="21c14-252">*RazorUIClassLib. views. dll* derlenen Razor içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="21c14-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="21c14-253">Razor Pages projesinden Razor Kullanıcı arabirimi kitaplığını kullanma</span><span class="sxs-lookup"><span data-stu-id="21c14-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="21c14-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21c14-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="21c14-255">Razor Pages Web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="21c14-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="21c14-256">**Çözüm Gezgini**>**yeni proje** **eklemek** > çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="21c14-257">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="21c14-258">Uygulamayı **WebApp1**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="21c14-259">**ASP.NET Core 2,1** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="21c14-260">**Web uygulaması** @no__t seçin-1 **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="21c14-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="21c14-261">**Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **Başlangıç projesi olarak ayarla**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="21c14-262">**Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **derleme bağımlılıkları** > **Proje bağımlılıkları**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="21c14-263">**WebApp1**'in bağımlılığı olarak **RazorUIClassLib** denetleyin.</span><span class="sxs-lookup"><span data-stu-id="21c14-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="21c14-264">**Çözüm Gezgini**, **WebApp1** ' a sağ tıklayıp > **başvurusu** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="21c14-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="21c14-265">**Başvuru Yöneticisi** Iletişim kutusunda **RazorUIClassLib** > **Tamam**' ı işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="21c14-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="21c14-266">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="21c14-267">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="21c14-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="21c14-268">Razor Pages uygulamasını ve RCL 'yi içeren bir Razor Pages Web uygulaması ve çözüm dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="21c14-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="21c14-269">Web uygulamasını derleyin ve çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="21c14-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="21c14-270">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="21c14-270">Test WebApp1</span></span>

<span data-ttu-id="21c14-271">Razor UI sınıfı kitaplığının kullanımda olduğunu doğrulamak için `/MyFeature/Page1` ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="21c14-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="21c14-272">Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="21c14-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="21c14-273">Hem Web uygulamasında hem de RCL 'de bir görünüm, kısmi görünüm veya Razor sayfası bulunduğunda, Web uygulamasındaki Razor biçimlendirmesi ( *. cshtml* dosyası) önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="21c14-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="21c14-274">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="21c14-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="21c14-275">Örnek indirme içinde *WebApp1/Areas/MyFeature2* öğesini *WebApp1/Areas/myfeature* olarak yeniden adlandırın ve test önceliğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="21c14-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="21c14-276">*RazorUIClassLib/Areas/myfeature/Pages/Shared/_Message. cshtml* kısmi görünümünü *WebApp1/Areas/Myfeature/Pages/Shared/_Message. cshtml*'ye kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="21c14-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="21c14-277">Biçimlendirmeyi yeni konumu belirtecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="21c14-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="21c14-278">Uygulamanın kısmi sürümünün kullanılmakta olduğunu doğrulamak için uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="21c14-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="21c14-279">RCL sayfaları düzeni</span><span class="sxs-lookup"><span data-stu-id="21c14-279">RCL Pages layout</span></span>

<span data-ttu-id="21c14-280">RCL içeriğine, Web uygulamasının *Sayfalar* klasörünün bir parçası olmasına rağmen başvurmak için, aşağıdaki dosya yapısıyla RCL projesini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="21c14-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="21c14-281">*RazorUIClassLib/sayfalar*</span><span class="sxs-lookup"><span data-stu-id="21c14-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="21c14-282">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="21c14-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="21c14-283">*RazorUIClassLib/Pages/Shared* iki kısmi dosya Içerir: *_header. cshtml* ve *_footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="21c14-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="21c14-284">@No__t-0 etiketleri *_Layout. cshtml* dosyasına eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="21c14-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
