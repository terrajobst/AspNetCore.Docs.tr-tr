---
title: ASP.NET Core Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: e99dd63200dc863552f099b5d715f78a9732165c
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538506"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="dc8c4-103">ASP.NET Core Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="dc8c4-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="dc8c4-104">Tarafından [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="dc8c4-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="dc8c4-105">Bileşenleri paylaşılabilir bir [Razor sınıf kitaplığı (RCL)](xref:razor-pages/ui-class) projeler arasında.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="dc8c4-106">A *Razor bileşenleri sınıf kitaplığı* gelen dahil edilebilir:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="dc8c4-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-107">Another project in the solution.</span></span>
* <span data-ttu-id="dc8c4-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-108">A NuGet package.</span></span>
* <span data-ttu-id="dc8c4-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-109">A referenced .NET library.</span></span>

<span data-ttu-id="dc8c4-110">Normal .NET türleri yalnızca bileşenlerdir gibi bir RCL tarafından sağlanan normal .NET derlemelerini bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="dc8c4-111">Bir RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="dc8c4-111">Create an RCL</span></span>

<span data-ttu-id="dc8c4-112">Sunulan yönergeleri <xref:blazor/get-started> makale Blazor için ortamınızı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc8c4-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc8c4-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="dc8c4-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-114">Create a new project.</span></span>
1. <span data-ttu-id="dc8c4-115">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="dc8c4-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-116">Select **Next**.</span></span>
1. <span data-ttu-id="dc8c4-117">Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="dc8c4-118">Örneklerde proje adı bu konudaki `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="dc8c4-119">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-119">Select **Create**.</span></span>
1. <span data-ttu-id="dc8c4-120">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="dc8c4-121">Seçin **Razor sınıf kitaplığı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="dc8c4-122">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-122">Select **Create**.</span></span>
1. <span data-ttu-id="dc8c4-123">RCL bir çözüme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="dc8c4-124">Çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-124">Right-click the solution.</span></span> <span data-ttu-id="dc8c4-125">Seçin **ekleme** > **mevcut proje**.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="dc8c4-126">RCL'ın proje dosyasına gidin.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="dc8c4-127">RCL'ın proje dosyası seçin ( *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="dc8c4-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="dc8c4-128">Bir başvuru RCL uygulamadan ekleyin:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="dc8c4-129">Uygulama projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-129">Right-click the app project.</span></span> <span data-ttu-id="dc8c4-130">Seçin **ekleme** > **başvuru**.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="dc8c4-131">RCL projeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-131">Select the RCL project.</span></span> <span data-ttu-id="dc8c4-132">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-132">Select **OK**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="dc8c4-133">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="dc8c4-133">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

1. <span data-ttu-id="dc8c4-134">Kullanım **Razor sınıf kitaplığı** şablonu (`razorclasslib`) ile [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu bir komut kabuğu'nda.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="dc8c4-135">Aşağıdaki örnekte, bir RCL adlandırılmış oluşturulan `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="dc8c4-136">Bulunduğu klasöre `MyComponentLib1` komut yürütülürken otomatik olarak oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="dc8c4-137">Kitaplık var olan bir projeye eklemek için [dotnet Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference) komutu bir komut kabuğu'nda.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="dc8c4-138">Aşağıdaki örnekte, RCL uygulamaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="dc8c4-139">Kitaplığa yoluna sahip uygulama proje klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="dc8c4-140">İstemci tarafı uygulamalar için desteklenmeyen RCLs</span><span class="sxs-lookup"><span data-stu-id="dc8c4-140">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="dc8c4-141">Geçerli ASP.NET Core 3.0 önizlemede, Razor sınıf kitaplıkları Blazor istemci tarafı uygulamalar ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-141">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="dc8c4-142">Tarafından oluşturulan bir Blazor bileşen kitaplık Blazor istemci tarafı uygulamalar için kullanmak `blazorlib` şablonu bir komut kabuğu'nda:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-142">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="dc8c4-143">Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-143">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="dc8c4-144">Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar ( *.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-144">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="dc8c4-145">İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-145">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="dc8c4-146">Bir kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="dc8c4-146">Consume a library component</span></span>

<span data-ttu-id="dc8c4-147">Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-147">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="dc8c4-148">Tam tür adı, ad alanı ile kullanın.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-148">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="dc8c4-149">Razor'ın kullanın [ \@kullanarak](xref:mvc/views/razor#using) yönergesi.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-149">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="dc8c4-150">Ada göre tek tek bileşenler eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-150">Individual components can be added by name.</span></span>

<span data-ttu-id="dc8c4-151">Aşağıdaki örneklerde, `MyComponentLib1` olan bir bileşen kitaplığını içeren bir `SalesReport` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-151">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="dc8c4-152">`SalesReport` Bileşen ad alanı ile tam tür adını kullanarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-152">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="dc8c4-153">Bileşen de kitaplık kapsama alınırsa başvurulabilir bir `@using` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-153">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="dc8c4-154">Dahil `@using MyComponentLib1` üst düzey yönerge *_Import.razor* kitaplığın bileşenleri tüm bir projeye kullanılabilir hale getirmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-154">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="dc8c4-155">Öğesine yönerge ekleyin bir *_Import.razor* ad alanı bir tek sayfalı veya bir klasördeki sayfalar kümesi uygulamak için herhangi bir düzeyde dosya.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-155">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="dc8c4-156">Sevkiyat NuGet derleme ve paketi</span><span class="sxs-lookup"><span data-stu-id="dc8c4-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="dc8c4-157">Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="dc8c4-158">Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu bir komut kabuğu'nda:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-158">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="dc8c4-159">NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komutu bir komut kabuğu'nda:</span><span class="sxs-lookup"><span data-stu-id="dc8c4-159">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="dc8c4-160">Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-160">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="dc8c4-161">Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-161">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="dc8c4-162">Bir Razor bileşenleri sınıf kitaplığı ile statik varlıkları oluşturma</span><span class="sxs-lookup"><span data-stu-id="dc8c4-162">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="dc8c4-163">Bir RCL statik varlıklar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-163">An RCL can include static assets.</span></span> <span data-ttu-id="dc8c4-164">Statik varlıkları, kitaplığı kullanan tüm uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-164">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="dc8c4-165">Daha fazla bilgi için bkz. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="dc8c4-165">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc8c4-166">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dc8c4-166">Additional resources</span></span>

* <xref:razor-pages/ui-class>
