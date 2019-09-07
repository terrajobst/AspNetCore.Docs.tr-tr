---
title: ASP.NET Core Blazor düzenleri
author: guardrex
description: Blazor uygulamaları için yeniden kullanılabilir düzen bileşenleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/layouts
ms.openlocfilehash: 05a38c10e18407d50422192ab1ddf3ff4b0f3a5b
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800367"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="afd34-103">ASP.NET Core Blazor düzenleri</span><span class="sxs-lookup"><span data-stu-id="afd34-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="afd34-104">Tarafından [Rainer Stropek](https://www.timecockpit.com) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="afd34-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="afd34-105">Menüler, telif hakkı iletileri ve şirket logoları gibi bazı uygulama öğeleri genellikle uygulamanın genel düzeninin parçasıdır ve uygulamadaki her bileşen tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afd34-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="afd34-106">Bu öğelerin kodunu bir uygulamanın tüm bileşenlerine kopyalamak, öğelerden biri bir güncelleştirme gerektirdiğinde her bileşenin güncelleştirilmesi gereken&mdash;etkili bir yaklaşım değildir.</span><span class="sxs-lookup"><span data-stu-id="afd34-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="afd34-107">Bu tür çoğaltmaya devam etmek zordur ve zaman içinde tutarsız içeriğe yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="afd34-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="afd34-108">*Düzenler* bu sorunu çözüyor.</span><span class="sxs-lookup"><span data-stu-id="afd34-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="afd34-109">Teknik olarak, düzen yalnızca başka bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="afd34-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="afd34-110">Bir düzen Razor şablonunda veya C# kodda tanımlanır ve [veri bağlama](xref:blazor/components#data-binding), [bağımlılık ekleme](xref:blazor/dependency-injection)ve diğer bileşen senaryolarını kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="afd34-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="afd34-111">Bir *bileşeni* bir *düzene*dönüştürmek için bileşen:</span><span class="sxs-lookup"><span data-stu-id="afd34-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="afd34-112">Düzen içindeki `LayoutComponentBase`işlenmiş içerik için bir `Body` özellik tanımlayan öğesinden devralır.</span><span class="sxs-lookup"><span data-stu-id="afd34-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="afd34-113">İçeriğin işlendiği yerleşim `@Body` biçimlendirmesinde konumu belirtmek için Razor söz dizimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="afd34-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="afd34-114">Aşağıdaki kod örneğinde, *mainlayout. Razor*olan bir düzen bileşeninin Razor şablonu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="afd34-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="afd34-115">Düzen, gezinti `LayoutComponentBase` çubuğu ve alt `@Body` bilgi arasında devralır ve Ayarlar:</span><span class="sxs-lookup"><span data-stu-id="afd34-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="afd34-116">Blazor uygulama şablonlarından birini temel alan bir uygulamada, `MainLayout` bileşen (*mainlayout. Razor*) uygulamanın *paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="afd34-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="afd34-117">Varsayılan düzen</span><span class="sxs-lookup"><span data-stu-id="afd34-117">Default layout</span></span>

<span data-ttu-id="afd34-118">Uygulamanın App `Router` *. Razor* dosyasındaki bileşende varsayılan uygulama yerleşimini belirtin.</span><span class="sxs-lookup"><span data-stu-id="afd34-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="afd34-119">Varsayılan Blazor `Router` şablonları tarafından belirtilen aşağıdaki bileşen, `MainLayout` bileşene varsayılan düzeni ayarlar:</span><span class="sxs-lookup"><span data-stu-id="afd34-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="afd34-120">İçerik için `NotFound` varsayılan bir düzen sağlamak üzere `NotFound` bir `LayoutView` içerik belirtin:</span><span class="sxs-lookup"><span data-stu-id="afd34-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="afd34-121">`Router` Bileşen hakkında daha fazla bilgi için bkz <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="afd34-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="afd34-122">Bir bileşende düzen belirtme</span><span class="sxs-lookup"><span data-stu-id="afd34-122">Specify a layout in a component</span></span>

<span data-ttu-id="afd34-123">Bir bileşene düzen uygulamak `@layout` için Razor yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="afd34-123">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="afd34-124">Derleyici, bileşen `@layout` sınıfına uygulanan `LayoutAttribute`öğesine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="afd34-124">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="afd34-125">Aşağıdaki `MasterList` bileşenin içeriği `MasterLayout` konumuna`@Body`öğesine eklenir:</span><span class="sxs-lookup"><span data-stu-id="afd34-125">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="afd34-126">Merkezi düzen seçimi</span><span class="sxs-lookup"><span data-stu-id="afd34-126">Centralized layout selection</span></span>

<span data-ttu-id="afd34-127">Bir uygulamanın her klasörü, isteğe bağlı olarak *_ımports. Razor*adlı bir şablon dosyası içerebilir.</span><span class="sxs-lookup"><span data-stu-id="afd34-127">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="afd34-128">Derleyici tüm Razor şablonlarındaki içeri aktarmalar dosyasında belirtilen yönergeleri aynı klasörde ve özyinelemeli olarak tüm alt klasörlerinde içerir.</span><span class="sxs-lookup"><span data-stu-id="afd34-128">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="afd34-129">Bu nedenle, bir *_ımports. Razor* dosyası `@layout MyCoolLayout` , bir klasördeki tüm bileşenlerin kullanımını `MyCoolLayout`sağlar.</span><span class="sxs-lookup"><span data-stu-id="afd34-129">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="afd34-130">Klasör ve alt klasörlerdeki tüm `@layout MyCoolLayout` *. Razor* dosyalarına tekrar tekrar ekleme gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="afd34-130">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="afd34-131">`@using`yönergeler aynı zamanda bileşenlere aynı şekilde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="afd34-131">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="afd34-132">Aşağıdaki *_ımports. Razor* dosyası içeri aktarmaları:</span><span class="sxs-lookup"><span data-stu-id="afd34-132">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="afd34-133">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="afd34-133">`MyCoolLayout`.</span></span>
* <span data-ttu-id="afd34-134">Aynı klasörde ve alt klasörlerde bulunan tüm Razor bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="afd34-134">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="afd34-135">`BlazorApp1.Data` Ad alanı.</span><span class="sxs-lookup"><span data-stu-id="afd34-135">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="afd34-136">*_Imports. Razor* dosyası, [Razor görünümleri ve sayfaları Için _Viewwimports. cshtml dosyasına](xref:mvc/views/layout#importing-shared-directives) benzer ancak özellikle Razor bileşen dosyalarına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="afd34-136">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="afd34-137">İç içe düzenleri</span><span class="sxs-lookup"><span data-stu-id="afd34-137">Nested layouts</span></span>

<span data-ttu-id="afd34-138">Uygulamalar iç içe düzenleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="afd34-138">Apps can consist of nested layouts.</span></span> <span data-ttu-id="afd34-139">Bir bileşen, daha sonra başka bir düzene başvuruda bulunan bir düzene başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="afd34-139">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="afd34-140">Örneğin, iç içe düzenleri çok düzeyli bir menü yapısı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afd34-140">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="afd34-141">Aşağıdaki örnek, iç içe düzenleri nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="afd34-141">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="afd34-142">*Epısodescomponent. Razor* dosyası görüntülenecek bileşendir.</span><span class="sxs-lookup"><span data-stu-id="afd34-142">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="afd34-143">Bileşen öğesine başvurur `MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="afd34-143">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="afd34-144">*Masterlistlayout. Razor* dosyası sağlar `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="afd34-144">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="afd34-145">Düzen, nerede işlendiği başka bir `MasterLayout`düzene başvurur.</span><span class="sxs-lookup"><span data-stu-id="afd34-145">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="afd34-146">`EpisodesComponent`, görüntülendiği yerde `@Body` işlenir:</span><span class="sxs-lookup"><span data-stu-id="afd34-146">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="afd34-147">Son olarak `MasterLayout` , *masterlayout. Razor* içinde üstbilgi, ana menü ve altbilgi gibi en üst düzey düzen öğelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="afd34-147">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="afd34-148">`MasterListLayout`ile birlikte `EpisodesComponent` `@Body` işlendiğinde, şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="afd34-148">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="afd34-149">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="afd34-149">Additional resources</span></span>

* <xref:mvc/views/layout>
