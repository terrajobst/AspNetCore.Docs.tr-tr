---
title: ASP.NET Core Blazor düzenleri
author: guardrex
description: Blazor uygulamalar için yeniden kullanılabilir Düzen bileşenlerinin nasıl oluşturulacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/layouts
ms.openlocfilehash: 2d652e149381f0a93e3135da978ab5737d47c6f1
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538541"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="6c90d-103">ASP.NET Core Blazor düzenleri</span><span class="sxs-lookup"><span data-stu-id="6c90d-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="6c90d-104">By [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="6c90d-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="6c90d-105">Menüleri, telif hakkı iletiler ve şirket logoları gibi bazı uygulama öğeleri genellikle uygulamanın genel düzenin parçası olan ve uygulamadaki her bir bileşen tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c90d-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="6c90d-106">Tüm uygulama bileşenlerinin bu öğelerin kodunu kopyalama olmayan bir yaklaşım&mdash;öğelerden biri de bir güncelleştirme gerektiren her zaman, her bileşen güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="6c90d-107">Bu çoğaltma zor korumak ve zaman içinde tutarsız içeriği neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="6c90d-108">*Düzenleri* bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="6c90d-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="6c90d-109">Teknik olarak, bir düzen başka bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="6c90d-110">Bir düzen Razor şablonu veya tanımlanan C# kullanabilirsiniz ve kod [veri bağlama](xref:blazor/components#data-binding), [bağımlılık ekleme](xref:blazor/dependency-injection)ve diğer bileşeni senaryolar.</span><span class="sxs-lookup"><span data-stu-id="6c90d-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="6c90d-111">Açmak için bir *bileşen* içine bir *Düzen*, bileşen:</span><span class="sxs-lookup"><span data-stu-id="6c90d-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="6c90d-112">Devralınan `LayoutComponentBase`, tanımlayan bir `Body` özelliği düzeni içinde işlenmiş içeriği.</span><span class="sxs-lookup"><span data-stu-id="6c90d-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="6c90d-113">Razor sözdizimini kullanan `@Body` içeriği burada işlenir Düzen biçimlendirmede konumu belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="6c90d-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="6c90d-114">Aşağıdaki kod örneği, Razor şablonu Düzen bileşeninin gösterir *MainLayout.razor*.</span><span class="sxs-lookup"><span data-stu-id="6c90d-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="6c90d-115">Düzen devralan `LayoutComponentBase` ve ayarlar `@Body` gezinti çubuğunu ve altbilgi arasında:</span><span class="sxs-lookup"><span data-stu-id="6c90d-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="6c90d-116">Bir bileşenin bir düzen belirtin</span><span class="sxs-lookup"><span data-stu-id="6c90d-116">Specify a layout in a component</span></span>

<span data-ttu-id="6c90d-117">Razor yönergesi kullanan `@layout` bileşene bir düzen uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="6c90d-117">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="6c90d-118">Derleyici dönüştürür `@layout` içine bir `LayoutAttribute`, bileşen sınıfa uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6c90d-118">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="6c90d-119">İçerik aşağıdaki bir bileşenin *MasterList.razor*, eklendiği `MainLayout` konumunda `@Body`:</span><span class="sxs-lookup"><span data-stu-id="6c90d-119">The content of the following component, *MasterList.razor*, is inserted into the `MainLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="6c90d-120">Merkezi Yerleşim Seçimi</span><span class="sxs-lookup"><span data-stu-id="6c90d-120">Centralized layout selection</span></span>

<span data-ttu-id="6c90d-121">Bir uygulamanın her klasör isteğe bağlı olarak adlandırılmış bir şablon dosyası içerebilir *_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="6c90d-121">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="6c90d-122">Derleyici, Razor şablonları aynı klasörde yer alan ve yinelemeli olarak tüm alt klasörlerindeki tüm içeri aktarmaları dosyasında belirtilen yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-122">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="6c90d-123">Bu nedenle, bir *_Imports.razor* dosyasını içeren `@layout MainLayout` klasör kullanımda bileşenlerinin tümünü sağlar `MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="6c90d-123">Therefore, an *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use `MainLayout`.</span></span> <span data-ttu-id="6c90d-124">Sürekli olarak eklemeniz gerekmez `@layout MainLayout` tüm *.razor* klasör ve alt klasörleri içinde dosyaları.</span><span class="sxs-lookup"><span data-stu-id="6c90d-124">There's no need to repeatedly add `@layout MainLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="6c90d-125">`@using` yönergeleri, bileşenler için de aynı şekilde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6c90d-125">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="6c90d-126">Aşağıdaki *_Imports.razor* dosya alır:</span><span class="sxs-lookup"><span data-stu-id="6c90d-126">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="6c90d-127">`MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="6c90d-127">`MainLayout`.</span></span>
* <span data-ttu-id="6c90d-128">Tüm Razor bileşenlerin aynı klasöre ve alt klasörleri.</span><span class="sxs-lookup"><span data-stu-id="6c90d-128">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="6c90d-129">`BlazorApp1.Data` Ad alanı.</span><span class="sxs-lookup"><span data-stu-id="6c90d-129">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="6c90d-130">*_Imports.razor* dosya benzer [Razor görünümleri ve sayfalar için _viewımports.cshtml dosyası](xref:mvc/views/layout#importing-shared-directives) ancak uygulanan özellikle Razor bileşen dosyaları.</span><span class="sxs-lookup"><span data-stu-id="6c90d-130">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="6c90d-131">Blazor şablonlarını kullanma *_Imports.razor* eçimi dosyaları.</span><span class="sxs-lookup"><span data-stu-id="6c90d-131">The Blazor templates use *_Imports.razor* files for layout selection.</span></span> <span data-ttu-id="6c90d-132">Blazor şablondan oluşturulan bir uygulamayı içeren *_Imports.razor* projenin ve kök dosyasında *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c90d-132">An app created from a Blazor template contains the *_Imports.razor* file in the root of the project and in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="6c90d-133">İç içe geçmiş düzenleri</span><span class="sxs-lookup"><span data-stu-id="6c90d-133">Nested layouts</span></span>

<span data-ttu-id="6c90d-134">Uygulamalar, iç içe geçmiş yerleşimlerinin oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-134">Apps can consist of nested layouts.</span></span> <span data-ttu-id="6c90d-135">Başka bir düzen sırayla başvuran bir düzen bir bileşene başvuruda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-135">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="6c90d-136">Örneğin, iç içe geçme düzenleri, çok düzeyli menüsü yapısı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c90d-136">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="6c90d-137">Aşağıdaki örnekte, iç içe geçmiş düzenlerini kullanmayı seçmelerine gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-137">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="6c90d-138">*EpisodesComponent.razor* göstermek için bileşeni dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="6c90d-138">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="6c90d-139">Bileşen başvurularını `MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="6c90d-139">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="6c90d-140">*MasterListLayout.razor* dosyası sağlar `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="6c90d-140">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="6c90d-141">Başka bir düzen düzenini başvuran `MasterLayout`, burada oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6c90d-141">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="6c90d-142">`EpisodesComponent` Burada işlenen `@Body` görünür:</span><span class="sxs-lookup"><span data-stu-id="6c90d-142">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="6c90d-143">Son olarak, `MasterLayout` içinde *MasterLayout.razor* üst bilgi, ana menü ve alt bilgi gibi üst düzey Düzen öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="6c90d-143">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="6c90d-144">`MasterListLayout` ile `EpisodesComponent` nerede işlendiğini `@Body` görünür:</span><span class="sxs-lookup"><span data-stu-id="6c90d-144">`MasterListLayout` with `EpisodesComponent` are rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="6c90d-145">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6c90d-145">Additional resources</span></span>

* <xref:mvc/views/layout>
