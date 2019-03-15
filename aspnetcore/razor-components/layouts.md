---
title: Razor bileşenleri düzenleri
author: guardrex
description: Razor bileşenleri uygulamalar için yeniden kullanılabilir Düzen bileşenlerinin nasıl oluşturulacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: razor-components/layouts
ms.openlocfilehash: ae2fe0208de1439958cf1e5ef1897a3a0c2dfb3f
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978297"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="1a969-103">Razor bileşenleri düzenleri</span><span class="sxs-lookup"><span data-stu-id="1a969-103">Razor Components layouts</span></span>

<span data-ttu-id="1a969-104">By [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="1a969-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="1a969-105">Uygulamalar genellikle birden fazla bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="1a969-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="1a969-106">Düzen öğeleri, logolar, menüler ve telif hakkı iletiler gibi tüm bileşenlerini bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1a969-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="1a969-107">Tüm uygulama bileşenlerinin bu düzen öğelerin kodunu kopyalama, etkili bir yaklaşım değildir.</span><span class="sxs-lookup"><span data-stu-id="1a969-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="1a969-108">Böyle çoğaltma korumak zor olabilir ve büyük olasılıkla zaman içinde tutarsız içeriği yol açar.</span><span class="sxs-lookup"><span data-stu-id="1a969-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="1a969-109">*Düzenleri* bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="1a969-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="1a969-110">Teknik olarak, bir düzen başka bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="1a969-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="1a969-111">Bir düzen Razor şablonu veya tanımlanan C# içerebilir ve kod [veri bağlama](xref:razor-components/components#data-binding), [bağımlılık ekleme](xref:razor-components/dependency-injection)ve diğer bileşenleri sıradan özellikleri.</span><span class="sxs-lookup"><span data-stu-id="1a969-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:razor-components/components#data-binding), [dependency injection](xref:razor-components/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="1a969-112">İki ek yönleri etkinleştirmek bir *bileşen* içine bir *düzeni*</span><span class="sxs-lookup"><span data-stu-id="1a969-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="1a969-113">Düzen bileşen devralmalıdır `LayoutComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="1a969-113">The layout component must inherit from `LayoutComponentBase`.</span></span> <span data-ttu-id="1a969-114">`LayoutComponentBase` tanımlayan bir `Body` içinde düzenini işlenmek üzere içeriği özelliği.</span><span class="sxs-lookup"><span data-stu-id="1a969-114">`LayoutComponentBase` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="1a969-115">Düzen bileşen `Body` özelliği gövde içeriği olması gereken yerde belirtmek için Razor sözdizimi kullanılarak oluşturulması `@Body`.</span><span class="sxs-lookup"><span data-stu-id="1a969-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="1a969-116">İşleme sırasında `@Body` düzeni içerik ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="1a969-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="1a969-117">Aşağıdaki kod örneği, Razor şablonu Düzen bileşeninin gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a969-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="1a969-118">Kullanımına dikkat edin `LayoutComponentBase` ve `@Body`:</span><span class="sxs-lookup"><span data-stu-id="1a969-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml?highlight=1,13)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="1a969-119">Bir bileşenin bir düzen kullanın</span><span class="sxs-lookup"><span data-stu-id="1a969-119">Use a layout in a component</span></span>

<span data-ttu-id="1a969-120">Razor yönergesi kullanan `@layout` bileşene bir düzen uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="1a969-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="1a969-121">Bu yönerge içine derleyici dönüştürür bir `LayoutAttribute`, bileşen sınıfa uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1a969-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="1a969-122">Aşağıdaki kod örneği kavramı gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a969-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="1a969-123">Bu bileşen içeriği eklendiği *MasterLayout* konumunda `@Body`:</span><span class="sxs-lookup"><span data-stu-id="1a969-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="1a969-124">Merkezi Yerleşim Seçimi</span><span class="sxs-lookup"><span data-stu-id="1a969-124">Centralized layout selection</span></span>

<span data-ttu-id="1a969-125">Her bir klasör, bir uygulama, isteğe bağlı olarak adlandırılmış bir şablon dosyası içerebilir *_viewımports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a969-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="1a969-126">Derleyici, Razor şablonları aynı klasörde yer alan ve yinelemeli olarak tüm alt klasörlerindeki tüm görünümü içeri aktarmalar dosyasında belirtilen yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="1a969-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="1a969-127">Bu nedenle, bir *_viewımports.cshtml* dosyasını içeren `@layout MainLayout` klasör kullanımda bileşenlerinin tümünü sağlar *MainLayout* düzeni.</span><span class="sxs-lookup"><span data-stu-id="1a969-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="1a969-128">Sürekli olarak eklemeniz gerekmez `@layout` tüm *.razor* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="1a969-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="1a969-129">Varsayılan şablon kullanan Not *_viewımports.cshtml* Düzen seçim mekanizması.</span><span class="sxs-lookup"><span data-stu-id="1a969-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="1a969-130">Yeni oluşturulan bir uygulamayı içeren *_viewımports.cshtml* dosyası *bileşenleri/sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="1a969-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="1a969-131">İç içe geçmiş düzenleri</span><span class="sxs-lookup"><span data-stu-id="1a969-131">Nested layouts</span></span>

<span data-ttu-id="1a969-132">Uygulamalar, iç içe geçmiş yerleşimlerinin oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="1a969-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="1a969-133">Başka bir düzen sırayla başvuran bir düzen bir bileşene başvuruda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1a969-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="1a969-134">Örneğin, iç içe geçme düzenleri, çok düzeyli menü yapısını yansıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1a969-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="1a969-135">Aşağıdaki kod örnekleri, iç içe geçmiş düzenlerini kullanmayı göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="1a969-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="1a969-136">*EpisodesComponent.cshtml* göstermek için bileşeni dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="1a969-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="1a969-137">Bileşen düzenini başvuran Not `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="1a969-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="1a969-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a969-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="1a969-139">*MasterListLayout.cshtml* dosyası sağlar `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="1a969-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="1a969-140">Başka bir düzen düzenini başvuran `MasterLayout`, burada katıştırılmış geçiyor.</span><span class="sxs-lookup"><span data-stu-id="1a969-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="1a969-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a969-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="1a969-142">Son olarak, `MasterLayout` üstbilgi, altbilgi ve ana menüsü gibi üst düzey Düzen öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="1a969-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="1a969-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a969-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
