---
title: ASP.NET Core Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenlerin, bir dış bileşen kitaplığından Blazor uygulamalarına nasıl dahil edileceğini öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/class-libraries
ms.openlocfilehash: d9ef276357e95d97b7d89427c5e237aceea7a0d3
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207107"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="d2fcc-103">ASP.NET Core Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="d2fcc-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="d2fcc-104">[Simon Timms](https://github.com/stimms) tarafından</span><span class="sxs-lookup"><span data-stu-id="d2fcc-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="d2fcc-105">Bileşenler, projeler genelinde [Razor sınıf kitaplığı 'nda (RCL)](xref:razor-pages/ui-class) paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="d2fcc-106">*Razor bileşenleri sınıf kitaplığı* , şuradan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="d2fcc-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-107">Another project in the solution.</span></span>
* <span data-ttu-id="d2fcc-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-108">A NuGet package.</span></span>
* <span data-ttu-id="d2fcc-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-109">A referenced .NET library.</span></span>

<span data-ttu-id="d2fcc-110">Bileşenler normal .NET türleri olduğu gibi, bir RCL tarafından sunulan bileşenler normal .NET derlemelerdir.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="d2fcc-111">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="d2fcc-111">Create an RCL</span></span>

<span data-ttu-id="d2fcc-112">Ortamınızı Blazor için yapılandırmak üzere <xref:blazor/get-started> makalesindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2fcc-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2fcc-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d2fcc-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-114">Create a new project.</span></span>
1. <span data-ttu-id="d2fcc-115">**Razor sınıfı kitaplığı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="d2fcc-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-116">Select **Next**.</span></span>
1. <span data-ttu-id="d2fcc-117">**Yeni bir Razor sınıf kitaplığı oluştur** Iletişim kutusunda **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="d2fcc-118">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d2fcc-119">Bu konudaki örneklerde proje adı `MyComponentLib1`kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="d2fcc-120">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-120">Select **Create**.</span></span>
1. <span data-ttu-id="d2fcc-121">RCL 'yi bir çözüme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="d2fcc-122">Çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-122">Right-click the solution.</span></span> <span data-ttu-id="d2fcc-123">**Varolan proje** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="d2fcc-124">RCL 'nin proje dosyasına gidin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="d2fcc-125">RCL 'nin proje dosyasını ( *. csproj*) seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="d2fcc-126">Uygulamadan RCL 'ye bir başvuru ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="d2fcc-127">Uygulama projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-127">Right-click the app project.</span></span> <span data-ttu-id="d2fcc-128">**Başvuru** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="d2fcc-129">RCL projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-129">Select the RCL project.</span></span> <span data-ttu-id="d2fcc-130">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d2fcc-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d2fcc-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="d2fcc-132">Bir komut kabuğunda [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla **Razor sınıf kitaplığı** şablonunu (`razorclasslib`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="d2fcc-133">Aşağıdaki örnekte adlı `MyComponentLib1`bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="d2fcc-134">Komut yürütüldüğünde, tutan `MyComponentLib1` klasör otomatik olarak oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="d2fcc-135">Kitaplığı var olan bir projeye eklemek için, bir komut kabuğunda [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="d2fcc-136">Aşağıdaki örnekte, RCL uygulamaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="d2fcc-137">Uygulamanın proje klasöründen, kitaplığın yolunu kullanarak aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="d2fcc-138">Kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="d2fcc-138">Consume a library component</span></span>

<span data-ttu-id="d2fcc-139">Başka bir projedeki bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-139">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="d2fcc-140">Ad alanı ile tam tür adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-140">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="d2fcc-141">Razor 'in [ \@using](xref:mvc/views/razor#using) yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-141">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="d2fcc-142">Tek tek bileşenler, ada göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-142">Individual components can be added by name.</span></span>

<span data-ttu-id="d2fcc-143">Aşağıdaki örneklerde, `MyComponentLib1` bir `SalesReport` bileşeni içeren bir bileşen kitaplığı vardır.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-143">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="d2fcc-144">Bileşene `SalesReport` , ad alanı ile tam tür adı kullanılarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-144">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="d2fcc-145">Ayrıca, kitaplık bir `@using` yönergeyle kapsama alınırsa bileşene de başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-145">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="d2fcc-146">Kitaplığın bileşenlerini projenin tamamına kullanılabilir hale getirmek için en üst düzey *_ımport. Razor* dosyasına yönergesiniekleyin.`@using MyComponentLib1`</span><span class="sxs-lookup"><span data-stu-id="d2fcc-146">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="d2fcc-147">Ad alanını tek bir sayfaya veya bir klasör içindeki sayfa kümesine uygulamak için bir *_ımport. Razor* dosyasına yönerge ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-147">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="d2fcc-148">NuGet 'i derleyin, paketleyebilir ve iade edin</span><span class="sxs-lookup"><span data-stu-id="d2fcc-148">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="d2fcc-149">Bileşen kitaplıkları standart .NET kitaplıkları olduğundan, paketlemeden ve tüm kitaplıkları NuGet 'e dağıtmadan farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-149">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="d2fcc-150">Paketleme, bir komut kabuğu 'nda [DotNet Pack](/dotnet/core/tools/dotnet-pack) komutu kullanılarak gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-150">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="d2fcc-151">Bir komut kabuğu 'nda [DotNet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) komutunu kullanarak paketi NuGet 'e yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d2fcc-151">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```dotnetcli
dotnet nuget publish
```

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="d2fcc-152">Statik varlıklar ile Razor bileşenleri sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="d2fcc-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="d2fcc-153">RCL statik varlıkları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-153">An RCL can include static assets.</span></span> <span data-ttu-id="d2fcc-154">Statik varlıklar, kitaplığı kullanan tüm uygulamalar tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="d2fcc-155">Daha fazla bilgi için bkz. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="d2fcc-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d2fcc-156">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d2fcc-156">Additional resources</span></span>

* <xref:razor-pages/ui-class>
