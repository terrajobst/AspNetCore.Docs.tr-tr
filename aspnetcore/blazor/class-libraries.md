---
title: ASP.NET Core Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/24/2019
uid: blazor/class-libraries
ms.openlocfilehash: 8676e0fd660b7d281c80d06d24d5593c2df6348b
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394622"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="bab98-103">ASP.NET Core Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="bab98-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="bab98-104">Tarafından [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="bab98-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="bab98-105">Bileşenleri paylaşılabilir bir [Razor sınıf kitaplığı (RCL)](xref:razor-pages/ui-class) projeler arasında.</span><span class="sxs-lookup"><span data-stu-id="bab98-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="bab98-106">A *Razor bileşenleri sınıf kitaplığı* gelen dahil edilebilir:</span><span class="sxs-lookup"><span data-stu-id="bab98-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="bab98-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="bab98-107">Another project in the solution.</span></span>
* <span data-ttu-id="bab98-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="bab98-108">A NuGet package.</span></span>
* <span data-ttu-id="bab98-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="bab98-109">A referenced .NET library.</span></span>

<span data-ttu-id="bab98-110">Normal .NET türleri yalnızca bileşenlerdir gibi bir RCL tarafından sağlanan normal .NET derlemelerini bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="bab98-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="bab98-111">Bir RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="bab98-111">Create an RCL</span></span>

<span data-ttu-id="bab98-112">Sunulan yönergeleri <xref:blazor/get-started> makale Blazor için ortamınızı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="bab98-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bab98-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bab98-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="bab98-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bab98-114">Create a new project.</span></span>
1. <span data-ttu-id="bab98-115">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="bab98-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="bab98-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bab98-116">Select **Next**.</span></span>
1. <span data-ttu-id="bab98-117">Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="bab98-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="bab98-118">Örneklerde proje adı bu konudaki `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="bab98-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="bab98-119">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="bab98-119">Select **Create**.</span></span>
1. <span data-ttu-id="bab98-120">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.</span><span class="sxs-lookup"><span data-stu-id="bab98-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="bab98-121">Seçin **Razor sınıf kitaplığı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="bab98-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="bab98-122">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="bab98-122">Select **Create**.</span></span>
1. <span data-ttu-id="bab98-123">RCL bir çözüme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bab98-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="bab98-124">Çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bab98-124">Right-click the solution.</span></span> <span data-ttu-id="bab98-125">Seçin **ekleme** > **mevcut proje**.</span><span class="sxs-lookup"><span data-stu-id="bab98-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="bab98-126">RCL'ın proje dosyasına gidin.</span><span class="sxs-lookup"><span data-stu-id="bab98-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="bab98-127">RCL'ın proje dosyası seçin ( *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="bab98-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="bab98-128">Bir başvuru RCL uygulamadan ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bab98-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="bab98-129">Uygulama projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bab98-129">Right-click the app project.</span></span> <span data-ttu-id="bab98-130">Seçin **ekleme** > **başvuru**.</span><span class="sxs-lookup"><span data-stu-id="bab98-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="bab98-131">RCL projeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="bab98-131">Select the RCL project.</span></span> <span data-ttu-id="bab98-132">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="bab98-132">Select **OK**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="bab98-133">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bab98-133">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

1. <span data-ttu-id="bab98-134">Razor sınıf kitaplığı şablonu (`razorclasslib`) ile [yeni dotnet](/dotnet/core/tools/dotnet-new) komut kabuğundan komutu.</span><span class="sxs-lookup"><span data-stu-id="bab98-134">Use the Razor Class Library template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="bab98-135">Aşağıdaki örnekte, bir RCL adlandırılmış oluşturulan `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="bab98-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="bab98-136">Bulunduğu klasöre `MyComponentLib1` komut yürütülürken otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bab98-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed.</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="bab98-137">Kitaplık var olan bir projeye eklemek için [dotnet Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference) komut kabuğu komutunu.</span><span class="sxs-lookup"><span data-stu-id="bab98-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command from a command shell.</span></span> <span data-ttu-id="bab98-138">Aşağıdaki örnekte, RCL uygulamaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="bab98-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="bab98-139">Kitaplığa yoluna sahip uygulama proje klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="bab98-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

<span data-ttu-id="bab98-140">Razor bileşen dosyaları ekleyin ( *.razor*) RCL için.</span><span class="sxs-lookup"><span data-stu-id="bab98-140">Add Razor component files (*.razor*) to the RCL.</span></span>

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="bab98-141">İstemci tarafı uygulamalar için desteklenmeyen RCLs</span><span class="sxs-lookup"><span data-stu-id="bab98-141">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="bab98-142">ASP.NET Core 3.0 Önizleme'de, Razor sınıf kitaplıkları Blazor istemci tarafı uygulamalar ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="bab98-142">In ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span>

<span data-ttu-id="bab98-143">Tarafından oluşturulan bir Blazor bileşen kitaplık Blazor istemci tarafı uygulamalar için kullanmak `blazorlib` komut kabuğundan şablonu:</span><span class="sxs-lookup"><span data-stu-id="bab98-143">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template from a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="bab98-144">Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="bab98-144">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="bab98-145">Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar ( *.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="bab98-145">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="bab98-146">İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="bab98-146">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="bab98-147">Bir kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="bab98-147">Consume a library component</span></span>

<span data-ttu-id="bab98-148">Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="bab98-148">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="bab98-149">Tam tür adı, ad alanı ile kullanın.</span><span class="sxs-lookup"><span data-stu-id="bab98-149">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="bab98-150">Razor'ın kullanın [ \@kullanarak](xref:mvc/views/razor#using) yönergesi.</span><span class="sxs-lookup"><span data-stu-id="bab98-150">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="bab98-151">Ada göre tek tek bileşenler eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="bab98-151">Individual components can be added by name.</span></span>

<span data-ttu-id="bab98-152">Aşağıdaki örneklerde, `MyComponentLib1` olduğu satış raporunu içeren bir bileşen kitaplığı (`SalesReport`) bileşeni.</span><span class="sxs-lookup"><span data-stu-id="bab98-152">In the following examples, `MyComponentLib1` is a component library containing a Sales Report (`SalesReport`) component.</span></span>

<span data-ttu-id="bab98-153">Satış Raporu bileşen ad alanı ile tam tür adını kullanarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="bab98-153">The Sales Report component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="bab98-154">Bileşen de kitaplık kapsama alınırsa başvurulabilir bir `@using` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="bab98-154">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="bab98-155">Dahil `@using MyComponentLib1` üst düzey yönerge *_Import.razor* kitaplığın bileşenleri tüm bir projeye kullanılabilir hale getirmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="bab98-155">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="bab98-156">Öğesine yönerge ekleyin bir *_Import.razor* ad alanı bir tek sayfalı veya bir klasördeki sayfalar kümesi uygulamak için herhangi bir düzeyde dosya.</span><span class="sxs-lookup"><span data-stu-id="bab98-156">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="bab98-157">Sevkiyat NuGet derleme ve paketi</span><span class="sxs-lookup"><span data-stu-id="bab98-157">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="bab98-158">Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı.</span><span class="sxs-lookup"><span data-stu-id="bab98-158">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="bab98-159">Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komut kabuğu komutunu:</span><span class="sxs-lookup"><span data-stu-id="bab98-159">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command from a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="bab98-160">NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komut kabuğu komutunu:</span><span class="sxs-lookup"><span data-stu-id="bab98-160">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command from a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="bab98-161">Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="bab98-161">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="bab98-162">Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="bab98-162">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="bab98-163">Bir Razor bileşenleri sınıf kitaplığı ile statik varlıkları oluşturma</span><span class="sxs-lookup"><span data-stu-id="bab98-163">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="bab98-164">Bir RCL statik varlıklar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="bab98-164">An RCL can include static assets.</span></span> <span data-ttu-id="bab98-165">Statik varlıkları, kitaplığı kullanan tüm uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bab98-165">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="bab98-166">Daha fazla bilgi için bkz. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="bab98-166">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bab98-167">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bab98-167">Additional resources</span></span>

* <xref:razor-pages/ui-class>
