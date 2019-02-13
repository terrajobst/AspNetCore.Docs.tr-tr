---
title: Razor bileşenleri düzenleri
author: guardrex
description: Yeniden kullanılabilir Düzen bileşenleri Blazor ve Razor bileşenleri uygulamaları oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159471"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="e6601-103">Razor bileşenleri düzenleri</span><span class="sxs-lookup"><span data-stu-id="e6601-103">Razor Components layouts</span></span>

<span data-ttu-id="e6601-104">By [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="e6601-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="e6601-105">Uygulamalar genellikle birden fazla sayfa içerir.</span><span class="sxs-lookup"><span data-stu-id="e6601-105">Apps typically contain more than one page.</span></span> <span data-ttu-id="e6601-106">Düzen öğeleri, logolar, menüler ve telif hakkı iletiler gibi tüm sayfalarında bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6601-106">Layout elements, such as menus, copyright messages, and logos, must be present on all pages.</span></span> <span data-ttu-id="e6601-107">Bu düzen öğelerin kod tüm uygulama sayfaları kopyalama etkili bir çözüm değildir.</span><span class="sxs-lookup"><span data-stu-id="e6601-107">Copying the code of these layout elements into all of the pages of an app isn't an efficient solution.</span></span> <span data-ttu-id="e6601-108">Böyle çoğaltma korumak zor olabilir ve büyük olasılıkla zaman içinde tutarsız içeriği yol açar.</span><span class="sxs-lookup"><span data-stu-id="e6601-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="e6601-109">*Düzenleri* bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="e6601-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="e6601-110">Teknik olarak, bir düzen başka bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="e6601-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="e6601-111">Bir düzen Razor şablonu veya tanımlanan C# kod ve veri bağlama, bağımlılık ekleme ve bileşenlerinin sıradan diğer özellikler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="e6601-111">A layout is defined in a Razor template or in C# code and can contain data binding, dependency injection, and other ordinary features of components.</span></span> <span data-ttu-id="e6601-112">İki ek yönleri etkinleştirmek bir *bileşen* içine bir *Düzen*:</span><span class="sxs-lookup"><span data-stu-id="e6601-112">Two additional aspects turn a *component* into a *layout*:</span></span>

* <span data-ttu-id="e6601-113">Düzen bileşen devralmalıdır `BlazorLayoutComponent`.</span><span class="sxs-lookup"><span data-stu-id="e6601-113">The layout component must inherit from `BlazorLayoutComponent`.</span></span> <span data-ttu-id="e6601-114">`BlazorLayoutComponent` tanımlayan bir `Body` içinde düzenini işlenmek üzere içeriği özelliği.</span><span class="sxs-lookup"><span data-stu-id="e6601-114">`BlazorLayoutComponent` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="e6601-115">Düzen bileşen `Body` özelliği gövde içeriği olması gereken yerde belirtmek için Razor sözdizimi kullanılarak oluşturulması `@Body`.</span><span class="sxs-lookup"><span data-stu-id="e6601-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="e6601-116">İşleme sırasında `@Body` düzeni içerik ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="e6601-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="e6601-117">Aşağıdaki kod örneği, Razor şablonu Düzen bileşeninin gösterir.</span><span class="sxs-lookup"><span data-stu-id="e6601-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="e6601-118">Kullanımına dikkat edin `BlazorLayoutComponent` ve `@Body`:</span><span class="sxs-lookup"><span data-stu-id="e6601-118">Note the use of `BlazorLayoutComponent` and `@Body`:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="e6601-119">Bir bileşenin bir düzen kullanın</span><span class="sxs-lookup"><span data-stu-id="e6601-119">Use a layout in a component</span></span>

<span data-ttu-id="e6601-120">Razor yönergesi kullanan `@layout` bileşene bir düzen uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="e6601-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="e6601-121">Bu yönerge içine derleyici dönüştürür bir `LayoutAttribute`, bileşen sınıfa uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e6601-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="e6601-122">Aşağıdaki kod örneği kavramı gösterir.</span><span class="sxs-lookup"><span data-stu-id="e6601-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="e6601-123">Bu bileşen içeriği eklendiği *MasterLayout* konumunda `@Body`:</span><span class="sxs-lookup"><span data-stu-id="e6601-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="e6601-124">Merkezi Yerleşim Seçimi</span><span class="sxs-lookup"><span data-stu-id="e6601-124">Centralized layout selection</span></span>

<span data-ttu-id="e6601-125">Her bir klasör, bir uygulama, isteğe bağlı olarak adlandırılmış bir şablon dosyası içerebilir *_viewımports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e6601-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="e6601-126">Derleyici, Razor şablonları aynı klasörde yer alan ve yinelemeli olarak tüm alt klasörlerindeki tüm görünümü içeri aktarmalar dosyasında belirtilen yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e6601-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="e6601-127">Bu nedenle, bir *_viewımports.cshtml* dosyasını içeren `@layout MainLayout` klasör kullanımda bileşenlerinin tümünü sağlar *MainLayout* düzeni.</span><span class="sxs-lookup"><span data-stu-id="e6601-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="e6601-128">Sürekli olarak eklemeniz gerekmez `@layout` tüm  *\*.cshtml* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="e6601-128">There's no need to repeatedly add `@layout` to all of the *\*.cshtml* files.</span></span>

<span data-ttu-id="e6601-129">Varsayılan şablon kullanan Not *_viewımports.cshtml* Düzen seçim mekanizması.</span><span class="sxs-lookup"><span data-stu-id="e6601-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="e6601-130">Yeni oluşturulan bir uygulamayı içeren *_viewımports.cshtml* dosyası *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="e6601-130">A newly created app contains the *_ViewImports.cshtml* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="e6601-131">İç içe geçmiş düzenleri</span><span class="sxs-lookup"><span data-stu-id="e6601-131">Nested layouts</span></span>

<span data-ttu-id="e6601-132">Uygulamalar, iç içe geçmiş yerleşimlerinin oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="e6601-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="e6601-133">Başka bir düzen sırayla başvuran bir düzen bir bileşene başvuruda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="e6601-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="e6601-134">Örneğin, iç içe geçme düzenleri, çok düzeyli menü yapısını yansıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e6601-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="e6601-135">Aşağıdaki kod örnekleri, iç içe geçmiş düzenlerini kullanmayı göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="e6601-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="e6601-136">*CustomersComponent.cshtml* göstermek için bileşeni dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="e6601-136">The *CustomersComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="e6601-137">Bileşen düzenini başvuran Not `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="e6601-137">Note that the component references the layout `MasterDataLayout`.</span></span>

<span data-ttu-id="e6601-138">*CustomersComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e6601-138">*CustomersComponent.cshtml*:</span></span>

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

<span data-ttu-id="e6601-139">*MasterDataLayout.cshtml* dosyası sağlar `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="e6601-139">The *MasterDataLayout.cshtml* file provides the `MasterDataLayout`.</span></span> <span data-ttu-id="e6601-140">Başka bir düzen düzenini başvuran `MainLayout`, burada katıştırılmış geçiyor.</span><span class="sxs-lookup"><span data-stu-id="e6601-140">The layout references another layout, `MainLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="e6601-141">*MasterDataLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e6601-141">*MasterDataLayout.cshtml*:</span></span>

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

<span data-ttu-id="e6601-142">Son olarak, `MainLayout` üstbilgi, altbilgi ve ana menüsü gibi üst düzey Düzen öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e6601-142">Finally, `MainLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="e6601-143">*MainLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e6601-143">*MainLayout.cshtml*:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
