---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/class-libraries
ms.openlocfilehash: 6c0b741de1e3b9ad2b226cc376f06ad8365542e8
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614908"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="476a1-103">Razor bileşenleri sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="476a1-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="476a1-104">Tarafından [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="476a1-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="476a1-105">Bileşenleri Razor sınıf kitaplıkları, projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="476a1-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="476a1-106">Bileşenlerin gelen dahil edilebilir:</span><span class="sxs-lookup"><span data-stu-id="476a1-106">Components can be included from:</span></span>

* <span data-ttu-id="476a1-107">Çözümdeki başka bir proje.</span><span class="sxs-lookup"><span data-stu-id="476a1-107">Another project in the solution.</span></span>
* <span data-ttu-id="476a1-108">Bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="476a1-108">A NuGet package.</span></span>
* <span data-ttu-id="476a1-109">Başvurulan bir .NET kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="476a1-109">A referenced .NET library.</span></span>

<span data-ttu-id="476a1-110">Normal .NET türleri yalnızca bileşenlerdir gibi Razor sınıf kitaplığı tarafından sağlanan normal .NET derlemelerini bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="476a1-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="476a1-111">Kullanım `razorclasslib` (Razor sınıf kitaplığı) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu:</span><span class="sxs-lookup"><span data-stu-id="476a1-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="476a1-112">Razor bileşen dosyaları ekleyin (*.razor*) Razor sınıf kitaplığı için.</span><span class="sxs-lookup"><span data-stu-id="476a1-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="476a1-113">Kitaplık var olan bir projeye eklemek için [dotnet sln](/dotnet/core/tools/dotnet-sln) komutu:</span><span class="sxs-lookup"><span data-stu-id="476a1-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="476a1-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="476a1-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="476a1-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="476a1-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="476a1-116">Razor sınıf kitaplıkları, ASP.NET Core Preview 3'te Blazor uygulamalarıyla uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="476a1-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="476a1-117">Blazor istemci tarafı ve Razor bileşenleri sunucu tarafı uygulamalar ile paylaşılan bir kitaplıktaki bileşenleri oluşturmak için tarafından oluşturulan bir Blazor sınıf kitaplığı kullanma `blazorlib` şablonu.</span><span class="sxs-lookup"><span data-stu-id="476a1-117">To create components in a library that can be shared with Blazor client-side and Razor Components server-side apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="476a1-118">Razor sınıf kitaplıkları, ASP.NET Core Preview 3'te statik varlıklar desteklemez.</span><span class="sxs-lookup"><span data-stu-id="476a1-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="476a1-119">Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="476a1-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="476a1-120">Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar (*.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="476a1-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="476a1-121">İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="476a1-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="476a1-122">Bir kitaplık bileşeni kullanma</span><span class="sxs-lookup"><span data-stu-id="476a1-122">Consume a library component</span></span>

<span data-ttu-id="476a1-123">Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) yönergesi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="476a1-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="476a1-124">Ada göre tek tek bileşenler eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="476a1-124">Individual components may be added by name.</span></span>

<span data-ttu-id="476a1-125">Yönergenin genel biçimi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="476a1-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="476a1-126">Örneğin, aşağıdaki yönergesi ekler `Component1` , `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="476a1-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="476a1-127">Ancak, bir derlemeden bir joker karakter kullanarak tüm bileşenleri eklemek için ortak olan (`*`):</span><span class="sxs-lookup"><span data-stu-id="476a1-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="476a1-128">`@addTagHelper` Yönergesini dahil edilebilir *_ViewImport.cshtml* bileşenleri uygulanan veya bir projenin tamamı için kullanılabilir bir tek sayfalı veya bir klasördeki sayfalar kümesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="476a1-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="476a1-129">İle `@addTagHelper` uygulama ile aynı derlemedeki oldukları gibi yerinde yönergesi, bileşen kitaplığının bileşenleri tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="476a1-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="476a1-130">Sevkiyat NuGet derleme ve paketi</span><span class="sxs-lookup"><span data-stu-id="476a1-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="476a1-131">Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı.</span><span class="sxs-lookup"><span data-stu-id="476a1-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="476a1-132">Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu:</span><span class="sxs-lookup"><span data-stu-id="476a1-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="476a1-133">NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komutu:</span><span class="sxs-lookup"><span data-stu-id="476a1-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="476a1-134">Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="476a1-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="476a1-135">Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="476a1-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
