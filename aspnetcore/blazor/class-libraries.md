---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/09/2019
uid: blazor/class-libraries
ms.openlocfilehash: 4b0b9150a507eef302a95055ae1485f0f9c2d8cc
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034711"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="7f9bb-103">Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="7f9bb-103">Razor components class libraries</span></span>

<span data-ttu-id="7f9bb-104">Tarafından [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="7f9bb-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="7f9bb-105">Bileşenleri Razor sınıf kitaplıkları, projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="7f9bb-106">Bileşenlerin gelen dahil edilebilir:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-106">Components can be included from:</span></span>

* <span data-ttu-id="7f9bb-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-107">Another project in the solution.</span></span>
* <span data-ttu-id="7f9bb-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-108">A NuGet package.</span></span>
* <span data-ttu-id="7f9bb-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-109">A referenced .NET library.</span></span>

<span data-ttu-id="7f9bb-110">Normal .NET türleri yalnızca bileşenlerdir gibi Razor sınıf kitaplığı tarafından sağlanan normal .NET derlemelerini bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="7f9bb-111">Razor sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="7f9bb-111">Create a Razor class library</span></span>

<span data-ttu-id="7f9bb-112">Sunulan yönergeleri <xref:blazor/get-started> makale Blazor için ortamınızı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f9bb-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f9bb-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7f9bb-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-114">Create a new project.</span></span>
1. <span data-ttu-id="7f9bb-115">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="7f9bb-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-116">Select **Next**.</span></span>
1. <span data-ttu-id="7f9bb-117">Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="7f9bb-118">Örneklerde proje adı bu konudaki `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="7f9bb-119">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-119">Select **Create**.</span></span>
1. <span data-ttu-id="7f9bb-120">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="7f9bb-121">Seçin **Razor sınıf kitaplığı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="7f9bb-122">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-122">Select **Create**.</span></span>
1. <span data-ttu-id="7f9bb-123">Razor sınıf kitaplığı, bir çözüme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-123">Add the Razor class library to a solution:</span></span>
   1. <span data-ttu-id="7f9bb-124">Çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-124">Right-click the solution.</span></span> <span data-ttu-id="7f9bb-125">Seçin **ekleme** > **mevcut proje**.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="7f9bb-126">Razor Sınıf Kitaplığı'nızın proje dosyasına gidin.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-126">Navigate to the Razor class library's project file.</span></span>
   1. <span data-ttu-id="7f9bb-127">Razor Sınıf Kitaplığı'nızın proje dosyası seçin ( *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7f9bb-127">Select the Razor class library's project file (*.csproj*).</span></span>
1. <span data-ttu-id="7f9bb-128">Razor sınıf kitaplığı başvurusu uygulamadan ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-128">Add a reference the Razor class library from the app:</span></span>
   1. <span data-ttu-id="7f9bb-129">Uygulama projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-129">Right-click the app project.</span></span> <span data-ttu-id="7f9bb-130">Seçin **ekleme** > **başvuru**.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="7f9bb-131">Razor sınıf kitaplığı Projesi'ni seçin.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-131">Select the Razor class library project.</span></span> <span data-ttu-id="7f9bb-132">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-132">Select **OK**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="7f9bb-133">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7f9bb-133">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

1. <span data-ttu-id="7f9bb-134">Razor sınıf kitaplığı kullanma (`razorclasslib`) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komut kabuğundan komutu.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-134">Use the Razor class library (`razorclasslib`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="7f9bb-135">Aşağıdaki örnekte, bir Razor sınıf kitaplığı adlandırılmış oluşturulan `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-135">In the following example, a Razor class library is created named `MyComponentLib1`.</span></span> <span data-ttu-id="7f9bb-136">Bulunduğu klasöre `MyComponentLib1` komut yürütülürken otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed.</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="7f9bb-137">Kitaplık var olan bir projeye eklemek için [dotnet Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference) komut kabuğu komutunu.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command from a command shell.</span></span> <span data-ttu-id="7f9bb-138">Aşağıdaki örnekte, Razor sınıf kitaplığı, uygulamaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-138">In the following example, the Razor class library is added to the app.</span></span> <span data-ttu-id="7f9bb-139">Kitaplığa yoluna sahip uygulama proje klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

<span data-ttu-id="7f9bb-140">Razor bileşen dosyaları ekleyin ( *.razor*) Razor sınıf kitaplığı için.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-140">Add Razor component files (*.razor*) to the Razor class library.</span></span>

## <a name="razor-class-libraries-not-supported-for-client-side-apps"></a><span data-ttu-id="7f9bb-141">Razor sınıf kitaplıkları için istemci tarafı uygulamalar desteklenmiyor</span><span class="sxs-lookup"><span data-stu-id="7f9bb-141">Razor class libraries not supported for client-side apps</span></span>

<span data-ttu-id="7f9bb-142">ASP.NET Core 3.0 Önizleme'de, Razor sınıf kitaplıkları Blazor istemci tarafı uygulamalar ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-142">In ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span>

<span data-ttu-id="7f9bb-143">Tarafından oluşturulan bir Blazor bileşen kitaplık Blazor istemci tarafı uygulamalar için kullanmak `blazorlib` komut kabuğundan şablonu:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-143">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template from a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="7f9bb-144">Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-144">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="7f9bb-145">Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar ( *.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-145">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="7f9bb-146">İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-146">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="static-assets-not-supported-for-server-side-apps"></a><span data-ttu-id="7f9bb-147">Sunucu tarafı uygulamalar için desteklenmeyen statik varlıklar</span><span class="sxs-lookup"><span data-stu-id="7f9bb-147">Static assets not supported for server-side apps</span></span>

<span data-ttu-id="7f9bb-148">ASP.NET Core 3.0 Önizleme'de, statik ya da bir Razor sınıf kitaplığı varlıklarından Blazor sunucu tarafı uygulamalar kullanamıyor (`razorclasslib`) veya Blazor kitaplığı (`blazorlib`).</span><span class="sxs-lookup"><span data-stu-id="7f9bb-148">In ASP.NET Core 3.0 Preview, Blazor server-side apps can't consume static assets from either a Razor class library (`razorclasslib`) or a Blazor library (`blazorlib`).</span></span>

<span data-ttu-id="7f9bb-149">Geçici bir çözüm, deneyebileceğiniz [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/).</span><span class="sxs-lookup"><span data-stu-id="7f9bb-149">As a temporary workaround, you can try [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/).</span></span>

> [!NOTE]
> <span data-ttu-id="7f9bb-150">[BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/) tutulan veya Microsoft tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-150">[BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/) isn't maintained or supported by Microsoft.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="7f9bb-151">Bir kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="7f9bb-151">Consume a library component</span></span>

<span data-ttu-id="7f9bb-152">Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-152">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="7f9bb-153">Tam tür adı, ad alanı ile kullanın.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-153">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="7f9bb-154">Razor'ın kullanın [ \@kullanarak](xref:mvc/views/razor#using) yönergesi.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-154">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="7f9bb-155">Ada göre tek tek bileşenler eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-155">Individual components can be added by name.</span></span>

<span data-ttu-id="7f9bb-156">Aşağıdaki örneklerde, `MyComponentLib1` olduğu satış raporunu içeren bir bileşen kitaplığı (`SalesReport`) bileşeni.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-156">In the following examples, `MyComponentLib1` is a component library containing a Sales Report (`SalesReport`) component.</span></span>

<span data-ttu-id="7f9bb-157">Satış Raporu bileşen ad alanı ile tam tür adını kullanarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-157">The Sales Report component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="7f9bb-158">Bileşen de kitaplık kapsama alınırsa başvurulabilir bir `@using` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-158">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="7f9bb-159">Dahil `@using MyComponentLib1` üst düzey yönerge *_Import.razor* kitaplığın bileşenleri tüm bir projeye kullanılabilir hale getirmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-159">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="7f9bb-160">Öğesine yönerge ekleyin bir *_Import.razor* ad alanı bir tek sayfalı veya bir klasördeki sayfalar kümesi uygulamak için herhangi bir düzeyde dosya.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-160">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="7f9bb-161">Sevkiyat NuGet derleme ve paketi</span><span class="sxs-lookup"><span data-stu-id="7f9bb-161">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="7f9bb-162">Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-162">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="7f9bb-163">Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komut kabuğu komutunu:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-163">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command from a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="7f9bb-164">NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komut kabuğu komutunu:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-164">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command from a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="7f9bb-165">Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-165">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="7f9bb-166">Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-166">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span> <span data-ttu-id="7f9bb-167">Unutmayın [statik varlıklar, sunucu tarafı uygulamalar için desteklenmeyen](#static-assets-not-supported-for-server-side-apps), Blazor kitaplığı da dahil olmak üzere (`blazorlib`) bir sunucu tarafı uygulama tarafından başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-167">Note that [static assets aren't supported for server-side apps](#static-assets-not-supported-for-server-side-apps), including when a Blazor library (`blazorlib`) is referenced by a server-side app.</span></span>

## <a name="create-a-razor-class-library-with-static-assets"></a><span data-ttu-id="7f9bb-168">Razor sınıf kitaplığı ile statik varlıkları oluşturma</span><span class="sxs-lookup"><span data-stu-id="7f9bb-168">Create a Razor class library with static assets</span></span>

<span data-ttu-id="7f9bb-169">Razor sınıf kitaplıkları (RCL) sık RCL kullanan uygulama tarafından başvurulan statik varlıklar Yardımcısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-169">Razor class libraries (RCL) frequently require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="7f9bb-170">ASP.NET Core, kullanan bir uygulama için kullanılabilir olan statik varlıkları içeren RCLs oluşturulmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-170">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="7f9bb-171">Yardımcı varlıklar bir Razor sınıf kitaplığının bir parçası dahil etmek için oluşturma bir *wwwroot* sınıf kitaplığı klasöründe ve tüm gerekli dosyaları bu klasörde içerir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-171">To include companion assets as part of a Razor class library, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="7f9bb-172">Razor sınıf kitaplığı paketleme, tüm varlıkları Yahoo! companion *wwwroot* klasörü paketine otomatik olarak dahil edilen ve bu paketi uygulamaları için kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-172">When packing a Razor class library, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-razor-class-library"></a><span data-ttu-id="7f9bb-173">Başvurulan bir Razor sınıf kitaplığı ait içerikleri kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="7f9bb-173">Consume content from a referenced Razor class library</span></span>

<span data-ttu-id="7f9bb-174">Eklenen dosyalar *wwwroot* klasör Razor sınıf kitaplığı kullanan uygulamaya önek altında sunulur `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-174">The files included in the *wwwroot* folder of the Razor class library are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="7f9bb-175">Aracılığıyla bu varlıkları kullanan uygulama başvuran `<script>`, `<style>`, `<img>`ve diğer HTML etiketleri.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-175">The consuming app references these assets via `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="7f9bb-176">Birden çok proje geliştirme akış</span><span class="sxs-lookup"><span data-stu-id="7f9bb-176">Multi-project development flow</span></span>

<span data-ttu-id="7f9bb-177">Uygulama çalıştırıldığında:</span><span class="sxs-lookup"><span data-stu-id="7f9bb-177">When the app runs:</span></span>

* <span data-ttu-id="7f9bb-178">Varlıklar, özgün konumlarında haberdar olun.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-178">The assets stay in their original folders.</span></span>
* <span data-ttu-id="7f9bb-179">Sınıf kitaplığı içinde herhangi bir değişiklik *wwwroot* klasörü, uygulamada derlenmeden yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-179">Any change within the class library *wwwroot* folder is reflected in the app without rebuilding.</span></span>

<span data-ttu-id="7f9bb-180">Derleme zamanında bildirim ile tüm statik web varlık konumlarda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-180">At build time, a manifest is produced with all the static web asset locations.</span></span> <span data-ttu-id="7f9bb-181">Bildirimi, çalışma zamanında okuyun ve başvurulan projeler ve paketleri varlıklarından kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-181">The manifest is read at runtime and allows the app to consume the assets from referenced projects and packages.</span></span>

### <a name="publish"></a><span data-ttu-id="7f9bb-182">Yayımlama</span><span class="sxs-lookup"><span data-stu-id="7f9bb-182">Publish</span></span>

<span data-ttu-id="7f9bb-183">Uygulama yayınlandıktan sonra başvuruda bulunulan tüm projelerin ve paketleri Yardımcısı varlıklarından içine kopyalanır *wwwroot* klasörü altında yayımlanan uygulamanın `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="7f9bb-183">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>
