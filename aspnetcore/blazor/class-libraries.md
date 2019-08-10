---
title: ASP.NET Core Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenlerin, bir dış bileşen kitaplığından Blazor uygulamalarına nasıl dahil edileceğini öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: 402b7b072554f63f85e7cf5e55336104d235a071
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913907"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="a1fdc-103">ASP.NET Core Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="a1fdc-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="a1fdc-104">[Simon Timms](https://github.com/stimms) tarafından</span><span class="sxs-lookup"><span data-stu-id="a1fdc-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="a1fdc-105">Bileşenler, projeler genelinde [Razor sınıf kitaplığı 'nda (RCL)](xref:razor-pages/ui-class) paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="a1fdc-106">*Razor bileşenleri sınıf kitaplığı* , şuradan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="a1fdc-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-107">Another project in the solution.</span></span>
* <span data-ttu-id="a1fdc-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-108">A NuGet package.</span></span>
* <span data-ttu-id="a1fdc-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-109">A referenced .NET library.</span></span>

<span data-ttu-id="a1fdc-110">Bileşenler normal .NET türleri olduğu gibi, bir RCL tarafından sunulan bileşenler normal .NET derlemelerdir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="a1fdc-111">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="a1fdc-111">Create an RCL</span></span>

<span data-ttu-id="a1fdc-112">Ortamınızı Blazor için yapılandırmak üzere <xref:blazor/get-started> makalesindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a1fdc-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1fdc-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a1fdc-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-114">Create a new project.</span></span>
1. <span data-ttu-id="a1fdc-115">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a1fdc-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-116">Select **Next**.</span></span>
1. <span data-ttu-id="a1fdc-117">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="a1fdc-118">Bu konudaki örneklerde proje adı `MyComponentLib1`kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="a1fdc-119">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-119">Select **Create**.</span></span>
1. <span data-ttu-id="a1fdc-120">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="a1fdc-121">**Razor sınıfı kitaplık** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="a1fdc-122">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-122">Select **Create**.</span></span>
1. <span data-ttu-id="a1fdc-123">RCL 'yi bir çözüme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="a1fdc-124">Çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-124">Right-click the solution.</span></span> <span data-ttu-id="a1fdc-125">**Varolan proje** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="a1fdc-126">RCL 'nin proje dosyasına gidin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="a1fdc-127">RCL 'nin proje dosyasını ( *. csproj*) seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="a1fdc-128">Uygulamadan RCL 'ye bir başvuru ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="a1fdc-129">Uygulama projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-129">Right-click the app project.</span></span> <span data-ttu-id="a1fdc-130">**Başvuru** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="a1fdc-131">RCL projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-131">Select the RCL project.</span></span> <span data-ttu-id="a1fdc-132">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-132">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a1fdc-133">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a1fdc-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="a1fdc-134">Bir komut kabuğunda [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla **Razor sınıf kitaplığı** şablonunu (`razorclasslib`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="a1fdc-135">Aşağıdaki örnekte adlı `MyComponentLib1`bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="a1fdc-136">Komut yürütüldüğünde, tutan `MyComponentLib1` klasör otomatik olarak oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="a1fdc-137">Kitaplığı var olan bir projeye eklemek için, bir komut kabuğunda [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="a1fdc-138">Aşağıdaki örnekte, RCL uygulamaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="a1fdc-139">Uygulamanın proje klasöründen, kitaplığın yolunu kullanarak aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="a1fdc-140">İstemci tarafı uygulamalar için RCLs desteklenmez</span><span class="sxs-lookup"><span data-stu-id="a1fdc-140">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="a1fdc-141">Geçerli ASP.NET Core 3,0 önizlemesinde Razor sınıfı kitaplıkları Blazor istemci tarafı uygulamalarıyla uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-141">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="a1fdc-142">Blazor istemci tarafı uygulamaları için, bir komut kabuğu 'nda `blazorlib` şablon tarafından oluşturulan bir Blazor bileşen kitaplığı kullanın:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-142">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="a1fdc-143">`blazorlib` Şablonu kullanan bileşen kitaplıkları, görüntüler, JavaScript ve stil sayfaları gibi statik dosyalar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-143">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="a1fdc-144">Derleme zamanında statik dosyalar oluşturulmuş derleme dosyasına ( *. dll*) katıştırılır ve bu sayede, kaynakları nasıl dahil edileceğini merak etmenize gerek kalmadan bileşenlerin tüketimine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-144">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="a1fdc-145">`content` Dizine eklenen tüm dosyalar gömülü bir kaynak olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-145">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="a1fdc-146">Kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="a1fdc-146">Consume a library component</span></span>

<span data-ttu-id="a1fdc-147">Başka bir projedeki bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-147">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="a1fdc-148">Ad alanı ile tam tür adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-148">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="a1fdc-149">Razor 'in [ \@using](xref:mvc/views/razor#using) yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-149">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="a1fdc-150">Tek tek bileşenler, ada göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-150">Individual components can be added by name.</span></span>

<span data-ttu-id="a1fdc-151">Aşağıdaki örneklerde, `MyComponentLib1` bir `SalesReport` bileşeni içeren bir bileşen kitaplığı vardır.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-151">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="a1fdc-152">Bileşene `SalesReport` , ad alanı ile tam tür adı kullanılarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-152">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="a1fdc-153">Ayrıca, kitaplık bir `@using` yönergeyle kapsama alınırsa bileşene de başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-153">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="a1fdc-154">Kitaplığın bileşenlerini projenin tamamına kullanılabilir hale getirmek için en üst düzey *_ımport. Razor* dosyasına yönergesiniekleyin.`@using MyComponentLib1`</span><span class="sxs-lookup"><span data-stu-id="a1fdc-154">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="a1fdc-155">Ad alanını tek bir sayfaya veya bir klasör içindeki sayfa kümesine uygulamak için bir *_ımport. Razor* dosyasına yönerge ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-155">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="a1fdc-156">NuGet 'i derleyin, paketleyebilir ve iade edin</span><span class="sxs-lookup"><span data-stu-id="a1fdc-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="a1fdc-157">Bileşen kitaplıkları standart .NET kitaplıkları olduğundan, paketlemeden ve tüm kitaplıkları NuGet 'e dağıtmadan farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="a1fdc-158">Paketleme, bir komut kabuğu 'nda [DotNet Pack](/dotnet/core/tools/dotnet-pack) komutu kullanılarak gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-158">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="a1fdc-159">Bir komut kabuğu 'nda [DotNet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) komutunu kullanarak paketi NuGet 'e yükleyin:</span><span class="sxs-lookup"><span data-stu-id="a1fdc-159">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="a1fdc-160">`blazorlib` Şablonu kullanırken, statik kaynaklar NuGet paketine dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-160">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="a1fdc-161">Kitaplık tüketicileri otomatik olarak komut dosyaları ve stil sayfaları alır, bu nedenle müşterileri el ile yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-161">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="a1fdc-162">Statik varlıklar ile Razor bileşenleri sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a1fdc-162">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="a1fdc-163">RCL statik varlıkları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-163">An RCL can include static assets.</span></span> <span data-ttu-id="a1fdc-164">Statik varlıklar, kitaplığı kullanan tüm uygulamalar tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-164">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="a1fdc-165">Daha fazla bilgi için bkz. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="a1fdc-165">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a1fdc-166">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a1fdc-166">Additional resources</span></span>

* <xref:razor-pages/ui-class>
