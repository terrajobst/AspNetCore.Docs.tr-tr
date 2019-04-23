---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/class-libraries
ms.openlocfilehash: f7c9ce20bf23bc532e664764d6e48d9163db727f
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982984"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="36822-103">Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="36822-103">Razor components class libraries</span></span>

<span data-ttu-id="36822-104">Tarafından [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="36822-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="36822-105">Bileşenleri Razor sınıf kitaplıkları, projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="36822-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="36822-106">Bileşenlerin gelen dahil edilebilir:</span><span class="sxs-lookup"><span data-stu-id="36822-106">Components can be included from:</span></span>

* <span data-ttu-id="36822-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="36822-107">Another project in the solution.</span></span>
* <span data-ttu-id="36822-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="36822-108">A NuGet package.</span></span>
* <span data-ttu-id="36822-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="36822-109">A referenced .NET library.</span></span>

<span data-ttu-id="36822-110">Normal .NET türleri yalnızca bileşenlerdir gibi Razor sınıf kitaplığı tarafından sağlanan normal .NET derlemelerini bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="36822-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="36822-111">Kullanım `razorclasslib` (Razor sınıf kitaplığı) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu:</span><span class="sxs-lookup"><span data-stu-id="36822-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="36822-112">Razor bileşen dosyaları ekleyin (*.razor*) Razor sınıf kitaplığı için.</span><span class="sxs-lookup"><span data-stu-id="36822-112">Add Razor component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="36822-113">Kitaplık var olan bir projeye eklemek için [dotnet sln](/dotnet/core/tools/dotnet-sln) komutu:</span><span class="sxs-lookup"><span data-stu-id="36822-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="36822-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36822-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="36822-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="36822-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="36822-116">Razor sınıf kitaplıkları, ASP.NET Core Preview 4'te Blazor uygulamalarıyla uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="36822-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 4.</span></span>
>
> <span data-ttu-id="36822-117">Blazor istemci tarafı ve Razor bileşenleri sunucu tarafı uygulamalar ile paylaşılan bir kitaplıktaki bileşenleri oluşturmak için tarafından oluşturulan bir Blazor sınıf kitaplığı kullanma `blazorlib` şablonu.</span><span class="sxs-lookup"><span data-stu-id="36822-117">To create components in a library that can be shared with Blazor client-side and Razor components server-side apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="36822-118">Razor sınıf kitaplıkları, ASP.NET Core Preview 4'te statik varlıklar desteklemez.</span><span class="sxs-lookup"><span data-stu-id="36822-118">Razor class libraries don't support static assets in ASP.NET Core Preview 4.</span></span> <span data-ttu-id="36822-119">Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="36822-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="36822-120">Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar (*.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="36822-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="36822-121">İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="36822-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="36822-122">Bir kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="36822-122">Consume a library component</span></span>

<span data-ttu-id="36822-123">Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="36822-123">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="36822-124">Ad alanı ile tam tür adı.</span><span class="sxs-lookup"><span data-stu-id="36822-124">Full type name with the namespace.</span></span>
* <span data-ttu-id="36822-125">Razor'ın [ \@kullanarak](xref:mvc/views/razor#using) yönergesi.</span><span class="sxs-lookup"><span data-stu-id="36822-125">Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="36822-126">Ada göre tek tek bileşenler eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="36822-126">Individual components may be added by name.</span></span>

<span data-ttu-id="36822-127">Aşağıdaki örneklerde, `MyComponentLibrary` olduğu satış raporu içeren bir bileşen kitaplığı (`SalesReport`) bileşeni.</span><span class="sxs-lookup"><span data-stu-id="36822-127">In the following examples, `MyComponentLibrary` is a component library containing the Sales Report (`SalesReport`) component.</span></span>

<span data-ttu-id="36822-128">Satış Raporu bileşen ad alanı ile tam tür adını kullanarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="36822-128">The Sales Report component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLibrary.SalesReport />
```

<span data-ttu-id="36822-129">Bileşen de kitaplık kapsama alınırsa başvurulabilir bir `@using` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="36822-129">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLibrary

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="36822-130">`@using` Yönergesini dahil edilebilir *_Import.razor* bileşenleri uygulanan veya bir projenin tamamı için kullanılabilir bir tek sayfalı veya bir klasördeki sayfalar kümesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="36822-130">The `@using` directive can be included in *_Import.razor* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="36822-131">Sevkiyat NuGet derleme ve paketi</span><span class="sxs-lookup"><span data-stu-id="36822-131">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="36822-132">Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı.</span><span class="sxs-lookup"><span data-stu-id="36822-132">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="36822-133">Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu:</span><span class="sxs-lookup"><span data-stu-id="36822-133">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="36822-134">NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komutu:</span><span class="sxs-lookup"><span data-stu-id="36822-134">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="36822-135">Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="36822-135">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="36822-136">Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="36822-136">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
