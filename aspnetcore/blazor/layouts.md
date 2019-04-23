---
title: Blazor düzenleri
author: guardrex
description: Blazor uygulamalar için yeniden kullanılabilir Düzen bileşenlerinin nasıl oluşturulacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: blazor/layouts
ms.openlocfilehash: 4a412b86d72cf4489dc2244a23c8b5c334f31e62
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982649"
---
# <a name="blazor-layouts"></a><span data-ttu-id="409d8-103">Blazor düzenleri</span><span class="sxs-lookup"><span data-stu-id="409d8-103">Blazor layouts</span></span>

<span data-ttu-id="409d8-104">By [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="409d8-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="409d8-105">Uygulamalar genellikle birden fazla bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="409d8-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="409d8-106">Düzen öğeleri, logolar, menüler ve telif hakkı iletiler gibi tüm bileşenlerini bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="409d8-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="409d8-107">Tüm uygulama bileşenlerinin bu düzen öğelerin kodunu kopyalama, etkili bir yaklaşım değildir.</span><span class="sxs-lookup"><span data-stu-id="409d8-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="409d8-108">Böyle çoğaltma korumak zor olabilir ve büyük olasılıkla zaman içinde tutarsız içeriği yol açar.</span><span class="sxs-lookup"><span data-stu-id="409d8-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="409d8-109">*Düzenleri* bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="409d8-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="409d8-110">Teknik olarak, bir düzen başka bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="409d8-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="409d8-111">Bir düzen Razor şablonu veya tanımlanan C# içerebilir ve kod [veri bağlama](xref:blazor/components#data-binding), [bağımlılık ekleme](xref:blazor/dependency-injection)ve diğer bileşenleri sıradan özellikleri.</span><span class="sxs-lookup"><span data-stu-id="409d8-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="409d8-112">İki ek yönleri etkinleştirmek bir *bileşen* içine bir *düzeni*</span><span class="sxs-lookup"><span data-stu-id="409d8-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="409d8-113">Düzen bileşen devralmalıdır `LayoutComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="409d8-113">The layout component must inherit from `LayoutComponentBase`.</span></span> <span data-ttu-id="409d8-114">`LayoutComponentBase` tanımlayan bir `Body` içinde düzenini işlenmek üzere içeriği özelliği.</span><span class="sxs-lookup"><span data-stu-id="409d8-114">`LayoutComponentBase` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="409d8-115">Düzen bileşen `Body` özelliği gövde içeriği olması gereken yerde belirtmek için Razor sözdizimi kullanılarak oluşturulması `@Body`.</span><span class="sxs-lookup"><span data-stu-id="409d8-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="409d8-116">İşleme sırasında `@Body` düzeni içerik ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="409d8-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="409d8-117">Aşağıdaki kod örneği, Razor şablonu Düzen bileşeninin gösterir.</span><span class="sxs-lookup"><span data-stu-id="409d8-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="409d8-118">Kullanımına dikkat edin `LayoutComponentBase` ve `@Body`:</span><span class="sxs-lookup"><span data-stu-id="409d8-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="409d8-119">Bir bileşenin bir düzen kullanın</span><span class="sxs-lookup"><span data-stu-id="409d8-119">Use a layout in a component</span></span>

<span data-ttu-id="409d8-120">Razor yönergesi kullanan `@layout` bileşene bir düzen uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="409d8-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="409d8-121">Bu yönerge içine derleyici dönüştürür bir `LayoutAttribute`, bileşen sınıfa uygulanır.</span><span class="sxs-lookup"><span data-stu-id="409d8-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="409d8-122">Aşağıdaki kod örneği kavramı gösterir.</span><span class="sxs-lookup"><span data-stu-id="409d8-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="409d8-123">Bu bileşen içeriği eklendiği *MasterLayout* konumunda `@Body`:</span><span class="sxs-lookup"><span data-stu-id="409d8-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="409d8-124">Merkezi Yerleşim Seçimi</span><span class="sxs-lookup"><span data-stu-id="409d8-124">Centralized layout selection</span></span>

<span data-ttu-id="409d8-125">Her bir klasör, bir uygulama, isteğe bağlı olarak adlandırılmış bir şablon dosyası içerebilir *_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="409d8-125">Every folder of a an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="409d8-126">Derleyici, Razor şablonları aynı klasörde yer alan ve yinelemeli olarak tüm alt klasörlerindeki tüm görünümü içeri aktarmalar dosyasında belirtilen yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="409d8-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="409d8-127">Bu nedenle, bir *_Imports.razor* dosyasını içeren `@layout MainLayout` klasör kullanımda bileşenlerinin tümünü sağlar *MainLayout* düzeni.</span><span class="sxs-lookup"><span data-stu-id="409d8-127">Therefore, a *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="409d8-128">Sürekli olarak eklemeniz gerekmez `@layout` tüm *.razor* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="409d8-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span> <span data-ttu-id="409d8-129">`@using` yönergeleri de aynı klasörde veya herhangi bir alt klasörde bileşenlerde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="409d8-129">`@using` directives are also applied to components in the same folder or any sub folders.</span></span>

<span data-ttu-id="409d8-130">Örneğin, aşağıdaki *_Imports.razor* dosya alır:</span><span class="sxs-lookup"><span data-stu-id="409d8-130">For example, the following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="409d8-131">`MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="409d8-131">`MainLayout`.</span></span>
* <span data-ttu-id="409d8-132">Tüm Razor bileşenlerin bir aynı klasörde ve alt klasörleri.</span><span class="sxs-lookup"><span data-stu-id="409d8-132">All Razor components in a the same folder and any sub folders.</span></span>
* <span data-ttu-id="409d8-133">`BlazorApp1.Data` Ad alanı.</span><span class="sxs-lookup"><span data-stu-id="409d8-133">The `BlazorApp1.Data` namespace.</span></span>
 
```cshtml
@layout MainLayout
@using Microsoft.AspNetCore.Components.
@using BlazorApp1.Data
```

<span data-ttu-id="409d8-134">Kullanım *_Imports.razor* dosya nasıl kullanabileceğiniz benzer *_viewımports.cshtml* Razor görünümleri ve sayfaları, ancak özellikle Razor bileşen dosyaları için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="409d8-134">Use of the *_Imports.razor* file is similar to how you can use *_ViewImports.cshtml* with Razor views and pages, but applied specifically to Razor component files.</span></span>

<span data-ttu-id="409d8-135">Varsayılan şablon kullanan Not *_Imports.razor* Düzen seçim mekanizması.</span><span class="sxs-lookup"><span data-stu-id="409d8-135">Note that the default template uses the *_Imports.razor* mechanism for layout selection.</span></span> <span data-ttu-id="409d8-136">Yeni oluşturulan bir uygulamayı içeren *_Imports.razor* dosyası *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="409d8-136">A newly created app contains the *_Imports.razor* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="409d8-137">İç içe geçmiş düzenleri</span><span class="sxs-lookup"><span data-stu-id="409d8-137">Nested layouts</span></span>

<span data-ttu-id="409d8-138">Uygulamalar, iç içe geçmiş yerleşimlerinin oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="409d8-138">Apps can consist of nested layouts.</span></span> <span data-ttu-id="409d8-139">Başka bir düzen sırayla başvuran bir düzen bir bileşene başvuruda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="409d8-139">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="409d8-140">Örneğin, iç içe geçme düzenleri, çok düzeyli menü yapısını yansıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="409d8-140">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="409d8-141">Aşağıdaki kod örnekleri, iç içe geçmiş düzenlerini kullanmayı göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="409d8-141">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="409d8-142">*EpisodesComponent.razor* göstermek için bileşeni dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="409d8-142">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="409d8-143">Bileşen düzenini başvuran Not `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="409d8-143">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="409d8-144">*EpisodesComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="409d8-144">*EpisodesComponent.razor*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="409d8-145">*MasterListLayout.razor* dosyası sağlar `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="409d8-145">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="409d8-146">Başka bir düzen düzenini başvuran `MasterLayout`, burada katıştırılmış geçiyor.</span><span class="sxs-lookup"><span data-stu-id="409d8-146">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="409d8-147">*MasterListLayout.razor*:</span><span class="sxs-lookup"><span data-stu-id="409d8-147">*MasterListLayout.razor*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="409d8-148">Son olarak, `MasterLayout` üstbilgi, altbilgi ve ana menüsü gibi üst düzey Düzen öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="409d8-148">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="409d8-149">*MasterLayout.razor*:</span><span class="sxs-lookup"><span data-stu-id="409d8-149">*MasterLayout.razor*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
