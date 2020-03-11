---
title: ASP.NET Core Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenlerin bir dış bileşen kitaplığından Blazor uygulamalara nasıl dahil edileceğini öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/class-libraries
ms.openlocfilehash: 32088b43f91174596f6b9251d36782e806f966b9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660491"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="cb6cd-103">ASP.NET Core Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="cb6cd-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="cb6cd-104">[Simon Timms](https://github.com/stimms) tarafından</span><span class="sxs-lookup"><span data-stu-id="cb6cd-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="cb6cd-105">Bileşenler, projeler genelinde [Razor sınıf kitaplığı 'nda (RCL)](xref:razor-pages/ui-class) paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="cb6cd-106">*Razor bileşenleri sınıf kitaplığı* , şuradan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="cb6cd-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-107">Another project in the solution.</span></span>
* <span data-ttu-id="cb6cd-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-108">A NuGet package.</span></span>
* <span data-ttu-id="cb6cd-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-109">A referenced .NET library.</span></span>

<span data-ttu-id="cb6cd-110">Bileşenler normal .NET türleri olduğu gibi, bir RCL tarafından sunulan bileşenler normal .NET derlemelerdir.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="cb6cd-111">RCL oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb6cd-111">Create an RCL</span></span>

<span data-ttu-id="cb6cd-112">Ortamınızı Blazor için yapılandırmak üzere <xref:blazor/get-started> makalesindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="cb6cd-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb6cd-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="cb6cd-114">Yeni bir proje oluşturma.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-114">Create a new project.</span></span>
1. <span data-ttu-id="cb6cd-115">**Razor sınıfı kitaplığı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="cb6cd-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-116">Select **Next**.</span></span>
1. <span data-ttu-id="cb6cd-117">**Yeni bir Razor sınıf kitaplığı oluştur** Iletişim kutusunda **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="cb6cd-118">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="cb6cd-119">Bu konudaki örneklerde `MyComponentLib1`proje adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="cb6cd-120">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-120">Select **Create**.</span></span>
1. <span data-ttu-id="cb6cd-121">RCL 'yi bir çözüme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="cb6cd-122">Çözüme sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-122">Right-click the solution.</span></span> <span data-ttu-id="cb6cd-123"> > **var olan projeyi** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="cb6cd-124">RCL 'nin proje dosyasına gidin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="cb6cd-125">RCL 'nin proje dosyasını ( *. csproj*) seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="cb6cd-126">Uygulamadan RCL 'ye bir başvuru ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="cb6cd-127">Uygulama projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-127">Right-click the app project.</span></span> <span data-ttu-id="cb6cd-128"> > **başvuru** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="cb6cd-129">RCL projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-129">Select the RCL project.</span></span> <span data-ttu-id="cb6cd-130">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-130">Select **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="cb6cd-131">Şablondan RCL oluşturulurken **destek sayfaları ve görünümler** onay kutusu Işaretliyse, Razor bileşeni yazma özelliğini etkinleştirmek için aşağıdaki içeriklerle oluşturulan projenin köküne bir *_Imports. Razor* dosyası da ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-131">If the **Support pages and views** check box is selected when generating the RCL from the template, then also add an *_Imports.razor* file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Components.Web
> ```
>
> <span data-ttu-id="cb6cd-132">Dosyayı oluşturulan projenin köküne el ile ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-132">Manually add the file the root of the generated project.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="cb6cd-133">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cb6cd-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="cb6cd-134">Bir komut kabuğunda [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla **Razor sınıf kitaplığı** şablonunu (`razorclasslib`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="cb6cd-135">Aşağıdaki örnekte, `MyComponentLib1`adlı bir RCL oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="cb6cd-136">`MyComponentLib1` tutan klasör komut yürütüldüğünde otomatik olarak oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

   > [!NOTE]
   > <span data-ttu-id="cb6cd-137">Şablondan RCL oluşturulurken `-s|--support-pages-and-views` anahtarı kullanılıyorsa, Razor bileşeni yazma özelliğini etkinleştirmek için aşağıdaki içeriklerle oluşturulan projenin köküne bir *_Imports. Razor* dosyası da ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-137">If the `-s|--support-pages-and-views` switch is used when generating the RCL from the template, then also add an *_Imports.razor* file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
   >
   > ```razor
   > @using Microsoft.AspNetCore.Components.Web
   > ```
   >
   > <span data-ttu-id="cb6cd-138">Dosyayı oluşturulan projenin köküne el ile ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-138">Manually add the file the root of the generated project.</span></span>

1. <span data-ttu-id="cb6cd-139">Kitaplığı var olan bir projeye eklemek için, bir komut kabuğunda [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-139">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="cb6cd-140">Aşağıdaki örnekte, RCL uygulamaya eklenir.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-140">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="cb6cd-141">Uygulamanın proje klasöründen, kitaplığın yolunu kullanarak aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-141">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="cb6cd-142">Kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="cb6cd-142">Consume a library component</span></span>

<span data-ttu-id="cb6cd-143">Başka bir projedeki bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-143">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="cb6cd-144">Ad alanı ile tam tür adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-144">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="cb6cd-145">Razor kullanarak Razor [\@](xref:mvc/views/razor#using) kullanın.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-145">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="cb6cd-146">Tek tek bileşenler, ada göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-146">Individual components can be added by name.</span></span>

<span data-ttu-id="cb6cd-147">Aşağıdaki örneklerde `MyComponentLib1`, bir `SalesReport` bileşeni içeren bir bileşen kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-147">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="cb6cd-148">`SalesReport` bileşene, ad alanı ile tam tür adı kullanılarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-148">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="cb6cd-149">Ayrıca, kitaplık bir `@using` yönergesi ile kapsama alınırsa bileşene de başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-149">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="cb6cd-150">Kitaplığın bileşenlerini bir projenin tamamına kullanılabilir hale getirmek için en üst düzey *_Import. Razor* dosyasına `@using MyComponentLib1` yönergesini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-150">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="cb6cd-151">Ad alanını tek bir sayfaya veya bir klasör içindeki sayfa kümesine uygulamak için herhangi bir düzeydeki bir *_Import. Razor* dosyasına yönergesini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-151">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="cb6cd-152">NuGet 'i derleyin, paketleyebilir ve iade edin</span><span class="sxs-lookup"><span data-stu-id="cb6cd-152">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="cb6cd-153">Bileşen kitaplıkları standart .NET kitaplıkları olduğundan, paketlemeden ve tüm kitaplıkları NuGet 'e dağıtmadan farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-153">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="cb6cd-154">Paketleme, bir komut kabuğu 'nda [DotNet Pack](/dotnet/core/tools/dotnet-pack) komutu kullanılarak gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="cb6cd-154">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="cb6cd-155">Bir komut kabuğunda [DotNet NuGet Push](/dotnet/core/tools/dotnet-nuget-push) komutunu kullanarak paketi NuGet 'e yükleyin.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-155">Upload the package to NuGet using the [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push) command in a command shell.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="cb6cd-156">Statik varlıklar ile Razor bileşenleri sınıf kitaplığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb6cd-156">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="cb6cd-157">RCL statik varlıkları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-157">An RCL can include static assets.</span></span> <span data-ttu-id="cb6cd-158">Statik varlıklar, kitaplığı kullanan tüm uygulamalar tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-158">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="cb6cd-159">Daha fazla bilgi için bkz. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="cb6cd-159">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cb6cd-160">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cb6cd-160">Additional resources</span></span>

* <xref:razor-pages/ui-class>
