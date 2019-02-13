---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Razor bileşenleri uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159233"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="d8ca7-103">Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="d8ca7-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="d8ca7-104">Tarafından [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="d8ca7-104">By [Simon Timms](https://github.com/stimms)</span></span>

> [!NOTE]
> <span data-ttu-id="d8ca7-105">.NET Core 3.0 Önizleme 2 SDK Razor bileşen sınıf kitaplıkları için bir proje şablonu içermez, ancak gelecekteki bir önizlemede şablon eklemek bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-105">The .NET Core 3.0 Preview 2 SDK doesn't include a project template for Razor Component Class Libraries, but we expect to add a template in a future preview.</span></span> <span data-ttu-id="d8ca7-106">Meantime bu konuda açıklanan Blazor bileşen sınıf kitaplığı şablonu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-106">In meantime, you can use the Blazor Component Class Library template explained in this topic.</span></span>

<span data-ttu-id="d8ca7-107">Bileşenleri bileşen kitaplıklarında, projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-107">Components can be shared in component libraries across projects.</span></span> <span data-ttu-id="d8ca7-108">Bileşenlerin gelen dahil edilebilir:</span><span class="sxs-lookup"><span data-stu-id="d8ca7-108">Components can be included from:</span></span>

* <span data-ttu-id="d8ca7-109">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-109">Another project in the solution.</span></span>
* <span data-ttu-id="d8ca7-110">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-110">A NuGet package.</span></span>
* <span data-ttu-id="d8ca7-111">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-111">A referenced .NET library.</span></span>

<span data-ttu-id="d8ca7-112">Normal .NET türleri yalnızca bileşenlerdir gibi normal .NET derlemeleri bileşen kitaplıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-112">Just as components are regular .NET types, component libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="d8ca7-113">Yeni bir bileşen kitaplığı oluşturmak için kullanın `blazorlib` şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-113">To create a new component library, use the `blazorlib` template with the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="d8ca7-114">Yüklü Şablonlar bir parçası olan şablondur olduğunda [Razor bileşenleri ayarlama](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="d8ca7-114">The template is part of the templates installed when [setting up Razor Components](xref:razor-components/get-started).</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="d8ca7-115">Kitaplık var olan bir projeye eklemek için [dotnet sln](/dotnet/core/tools/dotnet-sln) komutu:</span><span class="sxs-lookup"><span data-stu-id="d8ca7-115">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

```console
dotnet sln add .\MyComponentLib1
```

<span data-ttu-id="d8ca7-116">Bileşen kitaplıkları, resimler, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-116">Component libraries may contain static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="d8ca7-117">Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar (*.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-117">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="d8ca7-118">İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-118">Any files included in the `content` directory are marked as an embedded resource.</span></span> 

## <a name="consume-a-library-component"></a><span data-ttu-id="d8ca7-119">Bir kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="d8ca7-119">Consume a library component</span></span>

<span data-ttu-id="d8ca7-120">Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) yönergesi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-120">In order to consume components defined in a library in another project, the [@addTagHelper](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="d8ca7-121">Ada göre tek tek bileşenler eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-121">Individual components may be added by name.</span></span> <span data-ttu-id="d8ca7-122">Örneğin, aşağıdaki yönergesi ekler `Component1` , `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="d8ca7-122">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="d8ca7-123">Yönergenin genel biçimi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="d8ca7-123">The general format of the directive is:</span></span>

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

<span data-ttu-id="d8ca7-124">Ancak, genel olarak bir joker karakter kullanarak bir derlemeden bileşenlerinin tümünü içerir:</span><span class="sxs-lookup"><span data-stu-id="d8ca7-124">However, it's common to include all of the components from an assembly using a wildcard:</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="d8ca7-125">`@addTagHelper` Yönergesini dahil edilebilir *_ViewImport.cshtml* bileşenleri uygulanan veya bir projenin tamamı için kullanılabilir bir tek sayfalı veya bir klasördeki sayfalar kümesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-125">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="d8ca7-126">İle `@addTagHelper` uygulama ile aynı derlemedeki oldukları gibi yerinde yönergesi, bileşen kitaplığının bileşenleri tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-126">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span> 

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="d8ca7-127">Sevkiyat NuGet derleme ve paketi</span><span class="sxs-lookup"><span data-stu-id="d8ca7-127">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="d8ca7-128">Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-128">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="d8ca7-129">Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu:</span><span class="sxs-lookup"><span data-stu-id="d8ca7-129">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="d8ca7-130">NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komutu:</span><span class="sxs-lookup"><span data-stu-id="d8ca7-130">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="d8ca7-131">Dahil edilen tüm statik kaynakları, NuGet paketinin dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-131">Any included static resources are included in the NuGet package.</span></span> <span data-ttu-id="d8ca7-132">Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d8ca7-132">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
