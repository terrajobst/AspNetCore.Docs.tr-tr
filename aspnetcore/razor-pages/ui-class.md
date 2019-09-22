---
title: ASP.NET Core Sınıf Kitaplığı'nda yeniden kullanılabilir Razor UI
author: Rick-Anderson
description: Yeniden kullanılabilir Razor kısmi görünümler, ASP.NET Core sınıf kitaplığında kullanarak kullanıcı Arabirimi oluşturma açıklanır.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/21/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 1b544e208be049f02d01e35daa6eb3bfba94265a
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176471"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="b2294-103">ASP.NET Core 'de Razor Sınıf Kitaplığı projesini kullanarak yeniden kullanılabilir kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2294-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="b2294-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b2294-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b2294-105">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="b2294-106">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="b2294-107">Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="b2294-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="b2294-108">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="b2294-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="b2294-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2294-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="b2294-110">Razor UI içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2294-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2294-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2294-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b2294-112">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="b2294-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b2294-113">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="b2294-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b2294-114">(Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b2294-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="b2294-115">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b2294-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="b2294-116">**ASP.NET Core 3,0** veya sonraki bir sürümü seçildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b2294-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="b2294-117">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="b2294-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="b2294-118">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="b2294-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="b2294-119">Visual Studio 'daki bir şablon seçeneği, sayfalar ve görünümler için şablon desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="b2294-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b2294-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2294-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b2294-121">Komut satırından çalıştırmak `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="b2294-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="b2294-122">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b2294-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="b2294-123">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="b2294-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="b2294-124">Sayfalar ve görünümler için`dotnet new razorclasslib -support-pages-and-views`destek sağlamak üzere seçeneğini()geçirin.`-support-pages-and-views`</span><span class="sxs-lookup"><span data-stu-id="b2294-124">Pass the `-support-pages-and-views` option (`dotnet new razorclasslib -support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="b2294-125">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="b2294-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="b2294-126">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b2294-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="b2294-127">Razor dosyaları için RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2294-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="b2294-128">ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="b2294-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="b2294-129">' De `~/Pages` içeriğinikullanımasunanbirRCLoluşturmakiçinRCLPagesdüzeninebakın.`~/Areas/Pages` [](#rcl-pages-layout)</span><span class="sxs-lookup"><span data-stu-id="b2294-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="b2294-130">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="b2294-130">Reference RCL content</span></span>

<span data-ttu-id="b2294-131">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="b2294-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="b2294-132">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b2294-132">NuGet package.</span></span> <span data-ttu-id="b2294-133">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="b2294-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="b2294-134">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="b2294-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="b2294-135">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="b2294-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="b2294-136">Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="b2294-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="b2294-137">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="b2294-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="b2294-138">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="b2294-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="b2294-139">Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="b2294-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="b2294-140">Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b2294-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="b2294-141">Yeni bir konum belirtmek için işaretleme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2294-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="b2294-142">Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b2294-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="b2294-143">RCL sayfa düzeni</span><span class="sxs-lookup"><span data-stu-id="b2294-143">RCL Pages layout</span></span>

<span data-ttu-id="b2294-144">RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b2294-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="b2294-145">*RazorUIClassLib/sayfaları*</span><span class="sxs-lookup"><span data-stu-id="b2294-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="b2294-146">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="b2294-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="b2294-147">Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b2294-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="b2294-148">`<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2294-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="b2294-149">Statik varlıklar içeren bir RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2294-149">Create an RCL with static assets</span></span>

<span data-ttu-id="b2294-150">RCL, RCL 'nin tüketen uygulaması tarafından başvurulabilen, yardımcı statik varlıklar gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="b2294-151">ASP.NET Core, tüketen bir uygulama tarafından kullanılabilen statik varlıkları içeren RCLs oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b2294-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="b2294-152">Yardımcı varlıkları RCL 'nin bir parçası olarak dahil etmek için, sınıf kitaplığında bir *Wwwroot* klasörü oluşturun ve gerekli dosyaları bu klasöre ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2294-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="b2294-153">RCL 'yi paketleyerek, *Wwwroot* klasöründeki tüm yardımcı varlıklar pakete otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="b2294-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="b2294-154">Statik varlıkları hariç tut</span><span class="sxs-lookup"><span data-stu-id="b2294-154">Exclude static assets</span></span>

<span data-ttu-id="b2294-155">Statik varlıkları dışlamak için, istenen dışlama yolunu `$(DefaultItemExcludes)` proje dosyasındaki özellik grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2294-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="b2294-156">Girişleri noktalı virgül (`;`) ile ayırın.</span><span class="sxs-lookup"><span data-stu-id="b2294-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="b2294-157">Aşağıdaki örnekte, *Wwwroot* klasöründeki *lib. css* stil sayfası statik bir varlık olarak değerlendirilmez ve yayımlanan RCL 'ye dahil değildir:</span><span class="sxs-lookup"><span data-stu-id="b2294-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="b2294-158">TypeScript tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="b2294-158">Typescript integration</span></span>

<span data-ttu-id="b2294-159">TypeScript dosyalarını RCL 'ye eklemek için:</span><span class="sxs-lookup"><span data-stu-id="b2294-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="b2294-160">TypeScript dosyalarını ( *. TS*) *Wwwroot* klasörünün dışına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2294-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="b2294-161">Örneğin, dosyaları bir *istemci* klasörüne yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2294-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="b2294-162">*Wwwroot* klasörü için TypeScript derleme çıkışını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="b2294-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="b2294-163">Proje dosyasındaki`PropertyGroup` öğesinin içindeki özelliğini ayarlayın: `TypescriptOutDir`</span><span class="sxs-lookup"><span data-stu-id="b2294-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="b2294-164">Proje dosyasında bir `PropertyGroup` öğesinin içine aşağıdaki hedefi ekleyerek TypeScript `ResolveCurrentProjectStaticWebAssets` hedefini hedefin bağımlılığı olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b2294-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="b2294-165">Başvurulan bir RCL 'den içerik tüketme</span><span class="sxs-lookup"><span data-stu-id="b2294-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="b2294-166">RCL 'nin *Wwwroot* klasörüne dahil edilen dosyalar, ön ek `_content/{LIBRARY NAME}/`altında tüketen uygulamaya sunulur.</span><span class="sxs-lookup"><span data-stu-id="b2294-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="b2294-167">Örneğin, *Razor. Class. lib* adlı bir kitaplık, konumundaki `_content/Razor.Class.Lib/`statik içeriğe bir yol ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="b2294-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="b2294-168">Tüketen uygulama `<script>` `<style>`, ,,vediğerHTMLetiketleriylekitaplıktarafındansunulanstatikvarlıklarabaşvurur.`<img>`</span><span class="sxs-lookup"><span data-stu-id="b2294-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="b2294-169">Kullanan uygulamada [statik dosya desteğinin](xref:fundamentals/static-files) etkinleştirilmesi `Startup.Configure`gerekir:</span><span class="sxs-lookup"><span data-stu-id="b2294-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="b2294-170">Yapı çıktısından (`dotnet run`) kullanan uygulamayı çalıştırırken, varsayılan olarak, statik Web varlıkları geliştirme ortamında etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b2294-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="b2294-171">Derleme çıktılarından çalışırken diğer ortamlardaki varlıkları desteklemek için, *program.cs*içindeki konak `UseStaticWebAssets` Oluşturucu 'da çağırın:</span><span class="sxs-lookup"><span data-stu-id="b2294-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="b2294-172">Yayımlanan `UseStaticWebAssets` çıktısından (`dotnet publish`) bir uygulama çalıştırılırken çağırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b2294-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="b2294-173">Çoklu projeli geliştirme akışı</span><span class="sxs-lookup"><span data-stu-id="b2294-173">Multi-project development flow</span></span>

<span data-ttu-id="b2294-174">Kullanan uygulama şu şekilde çalışır:</span><span class="sxs-lookup"><span data-stu-id="b2294-174">When the consuming app runs:</span></span>

* <span data-ttu-id="b2294-175">RCL içindeki varlıklar özgün klasörlerinde kalır.</span><span class="sxs-lookup"><span data-stu-id="b2294-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="b2294-176">Varlıklar, tüketim uygulamasına taşınmaz.</span><span class="sxs-lookup"><span data-stu-id="b2294-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="b2294-177">RCL 'nin *Wwwroot* klasörü içindeki tüm değişiklikler, RCL yeniden oluşturulduktan ve tüketen uygulamayı yeniden oluşturmadan önce tüketen uygulamaya yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="b2294-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="b2294-178">RCL yapılandırıldığında, statik Web varlık konumlarını açıklayan bir bildirim oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b2294-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="b2294-179">Tüketen uygulama, başvurulan proje ve paketlerden varlıkları kullanmak için çalışma zamanında bildirimi okur.</span><span class="sxs-lookup"><span data-stu-id="b2294-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="b2294-180">Bir RCL 'ye yeni bir varlık eklendiğinde, bir uygulamanın yeni varlığa erişebilmesi için bildirim güncellemek üzere RCL 'nin yeniden oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b2294-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="b2294-181">Yayımlama</span><span class="sxs-lookup"><span data-stu-id="b2294-181">Publish</span></span>

<span data-ttu-id="b2294-182">Uygulama yayımlandığında, tüm başvurulan projeler ve paketlerin yardımcı varlıkları altında `_content/{LIBRARY NAME}/`yayımlanan uygulamanın *Wwwroot* klasörüne kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="b2294-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b2294-183">Razor görünümleri, sayfalar, denetleyiciler, sayfa modelleri, [Razor bileşenleri](xref:blazor/class-libraries), [Görünüm bileşenleri](xref:mvc/views/view-components)ve veri modelleri Razor sınıf kitaplığı 'nda (RCL) yerleşik olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="b2294-184">RCL paketlenir ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="b2294-185">Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="b2294-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="b2294-186">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="b2294-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="b2294-187">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2294-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="b2294-188">Razor UI içeren bir sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2294-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2294-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2294-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b2294-190">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="b2294-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b2294-191">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="b2294-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b2294-192">(Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b2294-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="b2294-193">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b2294-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="b2294-194">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b2294-195">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="b2294-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="b2294-196">RCL aşağıdaki proje dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b2294-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b2294-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2294-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b2294-198">Komut satırından çalıştırmak `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="b2294-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="b2294-199">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b2294-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="b2294-200">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="b2294-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="b2294-201">Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b2294-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="b2294-202">Razor dosyaları için RCL ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2294-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="b2294-203">ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör.</span><span class="sxs-lookup"><span data-stu-id="b2294-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="b2294-204">' De `~/Pages` içeriğinikullanımasunanbirRCLoluşturmakiçinRCLPagesdüzeninebakın.`~/Areas/Pages` [](#rcl-pages-layout)</span><span class="sxs-lookup"><span data-stu-id="b2294-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="b2294-205">RCL içeriğine başvur</span><span class="sxs-lookup"><span data-stu-id="b2294-205">Reference RCL content</span></span>

<span data-ttu-id="b2294-206">RCL tarafından başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="b2294-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="b2294-207">NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b2294-207">NuGet package.</span></span> <span data-ttu-id="b2294-208">Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="b2294-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="b2294-209">*{ProjectName} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="b2294-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="b2294-210">Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="b2294-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="b2294-211">İzlenecek yol: Bir Razor Pages projesinden bir RCL projesi oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="b2294-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="b2294-212">İndirebileceğiniz [tam proje](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ve yerine oluşturma test edin.</span><span class="sxs-lookup"><span data-stu-id="b2294-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="b2294-213">Örnek indirme, ek kod ve test etmek proje kolaylaştırmak bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="b2294-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="b2294-214">Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) yorumlarınızı üzerinde adım adım yönergeler ve örnekleri indirme ile.</span><span class="sxs-lookup"><span data-stu-id="b2294-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="b2294-215">İndirme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="b2294-215">Test the download app</span></span>

<span data-ttu-id="b2294-216">Tamamlanmış uygulamayı karşıdan henüz ve bunun yerine izlenecek projesi oluşturacak, atlamak [sonraki bölümde](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="b2294-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2294-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2294-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2294-218">Açık *.sln* dosyasını Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="b2294-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="b2294-219">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b2294-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b2294-220">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2294-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b2294-221">Bir komut isteminden *CLI* dizin RCL oluşturun ve web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="b2294-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="b2294-222">Taşı *WebApp1* dizin ve uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2294-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="b2294-223">Bölümündeki yönergeleri [Test WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="b2294-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="b2294-224">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2294-224">Create an RCL</span></span>

<span data-ttu-id="b2294-225">Bu bölümde bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b2294-225">In this section, an RCL is created.</span></span> <span data-ttu-id="b2294-226">Razor dosyaları için RCL eklenir.</span><span class="sxs-lookup"><span data-stu-id="b2294-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2294-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2294-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2294-228">RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b2294-228">Create the RCL project:</span></span>

* <span data-ttu-id="b2294-229">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="b2294-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b2294-230">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="b2294-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b2294-231">Uygulamayı **RazorUIClassLib** > **Tamam**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="b2294-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="b2294-232">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b2294-233">**Razor sınıfı kitaplığı** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="b2294-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="b2294-234">Razor kısmi Görünüm adlı bir dosya ekleyin *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b2294-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b2294-235">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2294-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b2294-236">Komut satırından şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2294-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="b2294-237">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="b2294-237">The preceding commands:</span></span>

* <span data-ttu-id="b2294-238">`RazorUIClassLib` RCL 'yi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2294-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="b2294-239">Bir Razor il_eti sayfası oluşturur ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="b2294-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="b2294-240">`-np` Parametresi olmadan sayfa oluşturur bir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="b2294-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="b2294-241">Oluşturur bir [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) dosya ve için RCL ekler.</span><span class="sxs-lookup"><span data-stu-id="b2294-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="b2294-242">*_ViewStart.cshtml* dosyası (sonraki bölümde eklenir) Razor sayfaları proje düzenini kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b2294-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="b2294-243">Razor dosyaları ve klasörleri projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2294-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="b2294-244">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="b2294-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="b2294-245">Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="b2294-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="b2294-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="b2294-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="b2294-247">Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2294-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b2294-248">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b2294-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="b2294-249">Daha fazla bilgi için *_viewımports.cshtml*, bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="b2294-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="b2294-250">Derleyici hata doğrulamak için sınıf kitaplığı derleme:</span><span class="sxs-lookup"><span data-stu-id="b2294-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="b2294-251">Derleme çıktısını içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="b2294-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="b2294-252">*RazorUIClassLib.Views.dll* derlenmiş Razor içeriği.</span><span class="sxs-lookup"><span data-stu-id="b2294-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="b2294-253">Bir Razor sayfaları projeden Razor kullanıcı Arabirimi kitaplığını kullanma</span><span class="sxs-lookup"><span data-stu-id="b2294-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2294-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2294-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2294-255">Razor sayfaları web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b2294-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="b2294-256">**Çözüm Gezgini**, **Yeni proje** **Ekle** >> çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b2294-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="b2294-257">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="b2294-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b2294-258">Uygulamayı adlandırma **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b2294-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="b2294-259">Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.</span><span class="sxs-lookup"><span data-stu-id="b2294-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b2294-260">**Web uygulaması** > **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="b2294-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="b2294-261">Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **başlangıç projesi olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="b2294-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="b2294-262">**Çözüm Gezgini**, **WebApp1** ' ye sağ tıklayın ve **yapı bağımlılıkları** > **Proje bağımlılıkları**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="b2294-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="b2294-263">Denetleme **RazorUIClassLib** bağımlılık olarak **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b2294-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="b2294-264">**Çözüm Gezgini**, **WebApp1** ' a sağ tıklayın ve **başvuru** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b2294-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="b2294-265">**Başvuru Yöneticisi** iletişim kutusunda **RazorUIClassLib** > **Tamam**' ı işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="b2294-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="b2294-266">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b2294-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b2294-267">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2294-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b2294-268">Razor Pages uygulamasını ve RCL 'yi içeren bir Razor Pages Web uygulaması ve çözüm dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b2294-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="b2294-269">Web uygulaması derleyebilir ve:</span><span class="sxs-lookup"><span data-stu-id="b2294-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="b2294-270">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="b2294-270">Test WebApp1</span></span>

<span data-ttu-id="b2294-271">Razor UI `/MyFeature/Page1` sınıf kitaplığının kullanımda olduğunu doğrulamak için öğesine gidin.</span><span class="sxs-lookup"><span data-stu-id="b2294-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="b2294-272">Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="b2294-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="b2294-273">Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur ( *.cshtml* dosya) web uygulaması önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="b2294-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="b2294-274">Örneğin, *WebApp1/Areas/MyFeature/Pages/Sayfa1. cshtml* öğesini WebApp1 öğesine ekleyin ve WebApp1 içindeki Sayfa1, RCL 'deki Sayfa1 'e göre öncelikli olur.</span><span class="sxs-lookup"><span data-stu-id="b2294-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="b2294-275">Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.</span><span class="sxs-lookup"><span data-stu-id="b2294-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="b2294-276">Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b2294-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="b2294-277">Yeni bir konum belirtmek için işaretleme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2294-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="b2294-278">Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b2294-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="b2294-279">RCL sayfa düzeni</span><span class="sxs-lookup"><span data-stu-id="b2294-279">RCL Pages layout</span></span>

<span data-ttu-id="b2294-280">RCL içerik web uygulaması'nın bir parçasıdır ancak başvuru *sayfaları* klasörü, aşağıdaki dosya yapısı ile RCL projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b2294-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="b2294-281">*RazorUIClassLib/sayfaları*</span><span class="sxs-lookup"><span data-stu-id="b2294-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="b2294-282">*RazorUIClassLib/sayfalar/paylaşılan*</span><span class="sxs-lookup"><span data-stu-id="b2294-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="b2294-283">Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyaları içerir: *_Header.cshtml* ve *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b2294-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="b2294-284">`<partial>` Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2294-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
