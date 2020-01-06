---
title: ASP.NET Core Razor bileşenleri oluşturma ve kullanma
author: guardrex
description: Veri bağlama, olayları işleme ve bileşen yaşam döngülerini yönetme dahil Razor bileşenleri oluşturmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/28/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: 87f21d84c17e5bbd1247bb955acee81384b890e7
ms.sourcegitcommit: 47d453f34b6fd0179119c572cb8be64c5365cbb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75597908"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="c39ae-103">ASP.NET Core Razor bileşenleri oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="c39ae-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="c39ae-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="c39ae-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="c39ae-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c39ae-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="c39ae-106"> uygulamalar, *bileşenleri*kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-106"> apps are built using *components*.</span></span> <span data-ttu-id="c39ae-107">Bir bileşen, bir sayfa, iletişim veya form gibi bir kullanıcı arabirimi (UI) öbekidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="c39ae-108">Bir bileşen, veri eklemek veya UI olaylarına yanıt vermek için gereken HTML işaretlemesini ve işleme mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="c39ae-109">Bileşenler esnek ve hafif.</span><span class="sxs-lookup"><span data-stu-id="c39ae-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="c39ae-110">Bunlar, iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="c39ae-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="c39ae-111">Component classes</span></span>

<span data-ttu-id="c39ae-112">Bileşenler, C# ve HTML Işaretlemesi kullanılarak [Razor](xref:mvc/views/razor) bileşen dosyalarında ( *. Razor*) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="c39ae-113">Blazor bir bileşen, bir *Razor bileşeni*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="c39ae-114">Bir bileşenin adı, büyük harfle başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="c39ae-115">Örneğin, *mycoolcomponent. Razor* geçerlidir ve *mycoolcomponent. Razor* geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="c39ae-116">Bir bileşen için Kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="c39ae-117">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="c39ae-118">Bir uygulama derlendiğinde, HTML biçimlendirme ve C# işleme mantığı bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="c39ae-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="c39ae-119">Oluşturulan sınıfın adı, dosyanın adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="c39ae-120">Bileşen sınıfının üyeleri bir `@code` bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="c39ae-121">`@code` bloğunda, bileşen durumu (özellikler, alanlar) olay işleme yöntemleriyle veya diğer bileşen mantığını tanımlamaya yönelik yöntemlerle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="c39ae-122">Birden fazla `@code` bloğu izin verilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="c39ae-123">ASP.NET Core 3,0 ' nin önceki önizlemelerinde, `@functions` blokları Razor bileşenlerinde `@code` bloklarında aynı amaçla kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="c39ae-124">`@functions` blokları Razor bileşenlerinde çalışmaya devam eder, ancak ASP.NET Core 3,0 Preview 6 veya sonraki bir sürümünde `@code` bloğunun kullanılmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="c39ae-125">Bileşen üyeleri, `@`ile başlayan ifadeleri kullanarak C# bileşenin işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="c39ae-126">Örneğin bir C# alan, alan adına `@` önek olarak işlenerek işlenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="c39ae-127">Aşağıdaki örnek değerlendirilir ve işler:</span><span class="sxs-lookup"><span data-stu-id="c39ae-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="c39ae-128">`font-style`için CSS özellik değerine `_headingFontStyle`.</span><span class="sxs-lookup"><span data-stu-id="c39ae-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="c39ae-129">`<h1>` öğesinin içeriğine `_headingText`.</span><span class="sxs-lookup"><span data-stu-id="c39ae-129">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="c39ae-130">Bileşen ilk olarak işlendikten sonra, bileşen işleme ağacını olaylara yanıt olarak yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="c39ae-131">, yeni işleme ağacını öncekiyle karşılaştırır ve tarayıcının Belge Nesne Modeli (DOM) üzerinde herhangi bir değişiklik uygular.</span><span class="sxs-lookup"><span data-stu-id="c39ae-131"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="c39ae-132">Bileşenler sıradan C# sınıflardır ve bir proje içinde herhangi bir yere yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="c39ae-133">Web sayfalarını üreten bileşenler genellikle *Sayfalar* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="c39ae-134">Sayfa olmayan bileşenler sıklıkla *paylaşılan* klasöre veya projeye eklenen özel bir klasöre yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="c39ae-135">Özel bir klasör kullanmak için, özel klasörün ad alanını üst bileşene ya da uygulamanın *_Imports. Razor* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="c39ae-136">Örneğin, aşağıdaki ad alanı, uygulamanın kök ad alanı `WebApplication`olduğunda *Bileşenler* klasöründeki bileşenleri kullanılabilir yapar:</span><span class="sxs-lookup"><span data-stu-id="c39ae-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```razor
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="c39ae-137">Bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme</span><span class="sxs-lookup"><span data-stu-id="c39ae-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="c39ae-138">Mevcut Razor Pages ve MVC uygulamalarıyla bileşenleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="c39ae-139">Razor bileşenleri kullanmak için mevcut sayfaları veya görünümleri yeniden yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="c39ae-140">Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden işlenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="c39ae-141">Bir sayfadan veya görünümden bir bileşeni işlemek için `Component` etiketi yardımcısını kullanın:</span><span class="sxs-lookup"><span data-stu-id="c39ae-141">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="c39ae-142">Parametreleri geçirme (örneğin, önceki örnekteki `IncrementAmount`) desteklenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-142">Passing parameters (for example, `IncrementAmount` in the preceding example) is supported.</span></span>

<span data-ttu-id="c39ae-143">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-143">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="c39ae-144">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-144">Is prerendered into the page.</span></span>
* <span data-ttu-id="c39ae-145">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="c39ae-145">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="c39ae-146">Açıklama</span><span class="sxs-lookup"><span data-stu-id="c39ae-146">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="c39ae-147">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-147">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="c39ae-148">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-148">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="c39ae-149">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-149">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="c39ae-150">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-150">Output from the component isn't included.</span></span> <span data-ttu-id="c39ae-151">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-151">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="c39ae-152">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-152">Renders the component into static HTML.</span></span> |

<span data-ttu-id="c39ae-153">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-153">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="c39ae-154">Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-154">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="c39ae-155">Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-155">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="c39ae-156">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-156">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="c39ae-157">Bileşenlerin nasıl işlendiği, bileşen durumu ve `Component` etiketi Yardımcısı hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c39ae-157">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="c39ae-158">Bir sayfadan veya görünümden bir bileşeni işlemek için `RenderComponentAsync<TComponent>` HTML yardımcı yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="c39ae-158">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

<span data-ttu-id="c39ae-159">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-159">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="c39ae-160">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-160">Is prerendered into the page.</span></span>
* <span data-ttu-id="c39ae-161">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="c39ae-161">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="c39ae-162">Açıklama</span><span class="sxs-lookup"><span data-stu-id="c39ae-162">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="c39ae-163">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-163">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="c39ae-164">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-164">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="c39ae-165">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-165">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="c39ae-166">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-166">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="c39ae-167">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-167">Output from the component isn't included.</span></span> <span data-ttu-id="c39ae-168">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-168">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="c39ae-169">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-169">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="c39ae-170">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-170">Renders the component into static HTML.</span></span> <span data-ttu-id="c39ae-171">Parametreler destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-171">Parameters are supported.</span></span> |

<span data-ttu-id="c39ae-172">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-172">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="c39ae-173">Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-173">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="c39ae-174">Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-174">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="c39ae-175">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-175">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="c39ae-176">Bileşenlerin nasıl işlendiği, bileşen durumu ve `RenderComponentAsync` HTML Yardımcısı hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c39ae-176">For more information on how components are rendered, component state, and the `RenderComponentAsync` HTML Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

## <a name="use-components"></a><span data-ttu-id="c39ae-177">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="c39ae-177">Use components</span></span>

<span data-ttu-id="c39ae-178">Bileşenler, HTML öğesi söz dizimini kullanarak bildirerek diğer bileşenleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-178">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="c39ae-179">Bir bileşeni kullanmak için biçimlendirme, etiket adının bileşen türü olduğu bir HTML etiketi gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="c39ae-179">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="c39ae-180">Öznitelik bağlama büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-180">Attribute binding is case sensitive.</span></span> <span data-ttu-id="c39ae-181">Örneğin, `@bind` geçerlidir ve `@Bind` geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-181">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="c39ae-182">*Index. Razor* dosyasında aşağıdaki biçimlendirme `HeadingComponent` örneği işler:</span><span class="sxs-lookup"><span data-stu-id="c39ae-182">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="c39ae-183">*Bileşenler/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-183">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="c39ae-184">Bir bileşen bir bileşen adıyla eşleşmeyen büyük harfle yazılmış bir HTML öğesi içeriyorsa, öğenin beklenmeyen bir adı olduğunu gösteren bir uyarı yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-184">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="c39ae-185">Bileşenin ad alanı için `@using` bir deyimin eklenmesi, bileşenin kullanılabilir olmasını sağlar ve bu da uyarıyı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-185">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="c39ae-186">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-186">Component parameters</span></span>

<span data-ttu-id="c39ae-187">Bileşenler, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanan *bileşen parametrelerine*sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-187">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="c39ae-188">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-188">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="c39ae-189">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-189">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="c39ae-190">Örnek uygulamadan aşağıdaki örnekte `ParentComponent`, `ChildComponent``Title` özelliğinin değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-190">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="c39ae-191">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-191">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="child-content"></a><span data-ttu-id="c39ae-192">Alt içerik</span><span class="sxs-lookup"><span data-stu-id="c39ae-192">Child content</span></span>

<span data-ttu-id="c39ae-193">Bileşenler, başka bir bileşenin içeriğini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-193">Components can set the content of another component.</span></span> <span data-ttu-id="c39ae-194">Atama bileşeni, alıcı bileşeni belirten Etiketler arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-194">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="c39ae-195">Aşağıdaki örnekte `ChildComponent`, işlemek için bir kullanıcı arabirimi segmentini temsil eden bir `RenderFragment`temsil eden bir `ChildContent` özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-195">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="c39ae-196">`ChildContent` değeri bileşenin, içeriğin işlenmesi gereken biçimlendirmesinde konumlandırılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-196">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="c39ae-197">`ChildContent` değeri üst bileşenden alınır ve önyükleme bölmesinin `panel-body`içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-197">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="c39ae-198">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-198">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="c39ae-199">`RenderFragment` içeriği alan özelliğin kurala göre `ChildContent` olarak adlandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-199">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="c39ae-200">Örnek uygulamadaki `ParentComponent`, içeriği `<ChildComponent>` etiketlerin içine yerleştirerek `ChildComponent` işlemek için içerik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-200">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="c39ae-201">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-201">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="c39ae-202">Öznitelik döndürme ve rastgele parametreler</span><span class="sxs-lookup"><span data-stu-id="c39ae-202">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="c39ae-203">Bileşenler, bileşen tarafından tanımlanan parametrelere ek olarak ek öznitelikler yakalayabilir ve işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-203">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="c39ae-204">Ek öznitelikler bir sözlükte yakalanabilir ve sonra bileşen [`@attributes`](xref:mvc/views/razor#attributes) Razor yönergesi kullanılarak işlendiğinde bir *öğe üzerine bırakılabilir* .</span><span class="sxs-lookup"><span data-stu-id="c39ae-204">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="c39ae-205">Bu senaryo, çeşitli özelleştirmeleri destekleyen bir işaretleme öğesi üreten bir bileşen tanımlarken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-205">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="c39ae-206">Örneğin, çok sayıda parametreyi destekleyen bir `<input>` öznitelikleri ayrı olarak tanımlamak sıkıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-206">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="c39ae-207">Aşağıdaki örnekte, ilk `<input>` öğesi (`id="useIndividualParams"`) bağımsız bileşen parametrelerini kullanır, ancak ikinci `<input>` öğesi (`id="useAttributesDict"`) öznitelik sponu kullanır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-207">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="c39ae-208">Parametrenin türü dize anahtarlarıyla `IEnumerable<KeyValuePair<string, object>>` uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-208">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="c39ae-209">`IReadOnlyDictionary<string, object>` kullanmak Bu senaryoda da bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-209">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="c39ae-210">Her iki yaklaşımın de kullanıldığı işlenen `<input>` öğeleri aynıdır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-210">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="c39ae-211">Rastgele öznitelikleri kabul etmek için, `CaptureUnmatchedValues` özelliği `true`olarak ayarlanan `[Parameter]` özniteliğini kullanarak bir bileşen parametresi tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="c39ae-211">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="c39ae-212">`[Parameter]` `CaptureUnmatchedValues` özelliği, parametrenin diğer bir parametreyle eşleşmeyen tüm özniteliklerle eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-212">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="c39ae-213">Bir bileşen yalnızca `CaptureUnmatchedValues`olan tek bir parametre tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-213">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="c39ae-214">`CaptureUnmatchedValues` ile kullanılan özellik türü, `Dictionary<string, object>` dize anahtarlarıyla atanabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-214">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="c39ae-215">`IEnumerable<KeyValuePair<string, object>>` veya `IReadOnlyDictionary<string, object>` Ayrıca bu senaryodaki seçeneklerdir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-215">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="c39ae-216">Öğe özniteliklerinin konumuna göre `@attributes` konumu önemlidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-216">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="c39ae-217">Öğe üzerinde `@attributes`, öznitelikler sağdan sola (son olarak) işlenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-217">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="c39ae-218">`Child` bileşeni tüketen bir bileşen için aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c39ae-218">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="c39ae-219">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-219">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="c39ae-220">*Childcomponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-220">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="c39ae-221">`Child` bileşenin `extra` özniteliği `@attributes`sağına ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-221">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="c39ae-222">`Parent` bileşenin işlenmiş `<div>`, öznitelikler sağdan sola (en son) işlendiği için ek özniteliğiyle geçirildiğinde `extra="5"` içerir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-222">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="c39ae-223">Aşağıdaki örnekte, `extra` ve `@attributes` sırası `Child` bileşeninin `<div>`tersine çevrilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-223">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="c39ae-224">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-224">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="c39ae-225">*Childcomponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-225">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="c39ae-226">`Parent` bileşenindeki işlenen `<div>`, ek öznitelik üzerinden geçirildiğinde `extra="10"` içerir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-226">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="c39ae-227">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="c39ae-227">Data binding</span></span>

<span data-ttu-id="c39ae-228">Hem bileşenlere hem de DOM öğelerine veri bağlama [`@bind`](xref:mvc/views/razor#bind) özniteliğiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-228">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="c39ae-229">Aşağıdaki örnek, bir `CurrentValue` özelliğini metin kutusu değerine bağlar:</span><span class="sxs-lookup"><span data-stu-id="c39ae-229">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="c39ae-230">Metin kutusu odağı kaybettiğinde, özelliğin değeri güncellenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-230">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="c39ae-231">Metin kutusu kullanıcı arabiriminde, özelliğin değerini değiştirme yanıt olarak değil, yalnızca bileşen işlendiğinde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-231">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="c39ae-232">Bileşenler olay işleyicisi kodu yürütüldükten sonra kendilerini oluşturduğundan, özellik güncelleştirmeleri *genellikle* olay işleyicisi tetiklendikten hemen sonra Kullanıcı arabirimine yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-232">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="c39ae-233">`CurrentValue` özelliği (`<input @bind="CurrentValue" />`) ile `@bind` kullanmak, temelde aşağıdakilere eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-233">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="c39ae-234">Bileşen işlendiğinde, giriş öğesinin `value` `CurrentValue` özelliğinden gelir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-234">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="c39ae-235">Kullanıcı metin kutusuna yazdığında ve öğe odağını değiştirdiğinde, `onchange` olayı tetiklenir ve `CurrentValue` özelliği değiştirilen değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-235">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="c39ae-236">`@bind`, tür dönüştürmelerin gerçekleştirildiği durumları işlediği için kod oluşturma daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-236">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="c39ae-237">Prensibi `@bind`, bir ifadenin geçerli değerini bir `value` özniteliğiyle ilişkilendirir ve kayıtlı işleyiciyi kullanarak değişiklikleri işler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-237">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="c39ae-238">`@bind` sözdizimiyle `onchange` olaylarının işlenmesine ek olarak, bir özellik veya alan, `event` parametreli bir [`@bind-value`](xref:mvc/views/razor#bind) özniteliği belirterek diğer olaylar kullanılarak da bağlanabilir ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="c39ae-238">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="c39ae-239">Aşağıdaki örnek, `oninput` olayı için `CurrentValue` özelliğini bağlar:</span><span class="sxs-lookup"><span data-stu-id="c39ae-239">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="c39ae-240">`onchange`aksine, öğe odağı kaybettiğinde harekete geçirilir `oninput` metin kutusunun değeri değiştiğinde harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-240">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="c39ae-241">**Ayrıştırılamayan değerler**</span><span class="sxs-lookup"><span data-stu-id="c39ae-241">**Unparsable values**</span></span>

<span data-ttu-id="c39ae-242">Bir Kullanıcı, bir veri sınırlama öğesine ayrıştırılamayan bir değer sağlıyorsa, bağlama olayı tetiklendiğinde, çözümlenemeyen değer otomatik olarak önceki değerine döndürülür.</span><span class="sxs-lookup"><span data-stu-id="c39ae-242">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="c39ae-243">Aşağıdaki senaryoyu ele alalım:</span><span class="sxs-lookup"><span data-stu-id="c39ae-243">Consider the following scenario:</span></span>

* <span data-ttu-id="c39ae-244">Bir `<input>` öğesi, `123`başlangıçtaki değeri olan bir `int` türüne bağlanır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-244">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="c39ae-245">Kullanıcı, öğe değerini sayfada `123.45` olarak güncelleştirir ve öğe odağını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-245">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="c39ae-246">Önceki senaryoda, öğenin değeri `123`olarak geri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="c39ae-246">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="c39ae-247">Değer `123.45` özgün `123`değerinin yararına reddedildiğinde, Kullanıcı değerinin kabul edilmediğini anlamıştır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-247">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="c39ae-248">Varsayılan olarak, bağlama öğenin `onchange` olayına (`@bind="{PROPERTY OR FIELD}"`) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-248">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="c39ae-249">Farklı bir olay ayarlamak için `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-249">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="c39ae-250">`oninput` olayı (`@bind-value:event="oninput"`) için yeniden sürüm, ayrıştırılamayan bir değer sunan herhangi bir tuş vuruşu sonrasında oluşur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-250">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="c39ae-251">`oninput` olayı `int`bağlantılı bir türle hedeflenirken, kullanıcının bir `.` karakteri yazmasının engellenmiş olması engellenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-251">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="c39ae-252">`.` bir karakter hemen kaldırılır, bu nedenle Kullanıcı yalnızca tam sayılara izin verilen anında geri bildirim alır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-252">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="c39ae-253">`oninput` olaylarındaki değerin geri döndürülmesi ideal olmayan, örneğin kullanıcının ayrıştırılamayan `<input>` bir değeri temizlemeye izin verilmesi gereken senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-253">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="c39ae-254">Alternatifler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-254">Alternatives include:</span></span>

* <span data-ttu-id="c39ae-255">`oninput` olayını kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-255">Don't use the `oninput` event.</span></span> <span data-ttu-id="c39ae-256">Öğe odağı kaybederene kadar geçersiz bir değer geri döndürülmediğinde, varsayılan `onchange` olayını (`@bind="{PROPERTY OR FIELD}"`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-256">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="c39ae-257">`int?` veya `string`gibi null yapılabilir bir türe bağlayın ve geçersiz girdileri işlemek için özel mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-257">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="c39ae-258">`InputNumber` veya `InputDate`gibi bir [form doğrulama bileşeni](xref:blazor/forms-validation)kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-258">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="c39ae-259">Form doğrulama bileşenlerinde geçersiz girişleri yönetmek için yerleşik destek vardır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-259">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="c39ae-260">Form doğrulama bileşenleri:</span><span class="sxs-lookup"><span data-stu-id="c39ae-260">Form validation components:</span></span>
  * <span data-ttu-id="c39ae-261">Kullanıcının geçersiz giriş sağlamasına ve ilişkili `EditContext`doğrulama hataları almasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-261">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="c39ae-262">Kullanıcı ek WebForm verisi girmeye uğramadan doğrulama hatalarını Kullanıcı ARABIRIMINDE görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-262">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="c39ae-263">**Genelleştirme**</span><span class="sxs-lookup"><span data-stu-id="c39ae-263">**Globalization**</span></span>

<span data-ttu-id="c39ae-264">`@bind` değerleri, geçerli kültürün kuralları kullanılarak görüntülenmek üzere biçimlendirilir ve ayrıştırılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-264">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="c39ae-265">Geçerli kültüre <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> özelliğinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-265">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="c39ae-266">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) aşağıdaki alan türleri için kullanılır (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="c39ae-266">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="c39ae-267">Yukarıdaki alan türleri:</span><span class="sxs-lookup"><span data-stu-id="c39ae-267">The preceding field types:</span></span>

* <span data-ttu-id="c39ae-268">, Uygun tarayıcı tabanlı biçimlendirme kuralları kullanılarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-268">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="c39ae-269">Serbest biçimli metin içeremez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-269">Can't contain free-form text.</span></span>
* <span data-ttu-id="c39ae-270">Tarayıcının uygulamasına göre Kullanıcı etkileşimi özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-270">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="c39ae-271">Aşağıdaki alan türleri belirli biçimlendirme gereksinimlerine sahiptir ve şu anda tüm büyük tarayıcılarda desteklenmediğinden Blazor tarafından desteklenmemektedir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-271">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="c39ae-272">`@bind`, bir değeri ayrıştırmak ve biçimlendirmek için bir <xref:System.Globalization.CultureInfo?displayProperty=fullName> sağlamak üzere `@bind:culture` parametresini destekler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-272">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="c39ae-273">`date` ve `number` alan türleri kullanılırken bir kültür belirtilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-273">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="c39ae-274">`date` ve `number`, gerekli kültürü sağlayan yerleşik Blazor desteğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-274">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="c39ae-275">Kullanıcının kültürünü ayarlama hakkında daha fazla bilgi için [Yerelleştirme](#localization) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-275">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="c39ae-276">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="c39ae-276">**Format strings**</span></span>

<span data-ttu-id="c39ae-277">Veri bağlama [`@bind:format`](xref:mvc/views/razor#bind)kullanarak <xref:System.DateTime> biçim dizeleriyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-277">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="c39ae-278">Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-278">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="c39ae-279">Yukarıdaki kodda `<input>` öğenin alan türü (`type`), `text`varsayılan olarak olur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-279">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="c39ae-280">`@bind:format` aşağıdaki .NET türlerini bağlamak için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-280">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="c39ae-281"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="c39ae-281"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="c39ae-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="c39ae-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="c39ae-283">`@bind:format` özniteliği, `<input>` öğesinin `value` uygulanacak tarih biçimini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-283">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="c39ae-284">Biçim Ayrıca, `onchange` bir olay gerçekleştiğinde değeri ayrıştırmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-284">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="c39ae-285">Blazor, tarihleri biçimlendirmek için yerleşik destek içerdiğinden `date` alanı türü için bir biçim belirtilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-285">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="c39ae-286">Önerinin artma, `yyyy-MM-dd` tarih biçimini yalnızca `date` alan türüyle bir biçim sağlanırsa doğru şekilde çalışacak şekilde kullanın:</span><span class="sxs-lookup"><span data-stu-id="c39ae-286">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

<span data-ttu-id="c39ae-287">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="c39ae-287">**Component parameters**</span></span>

<span data-ttu-id="c39ae-288">Bağlama bileşen parametrelerini tanır; burada `@bind-{property}`, bileşenler arasında bir özellik değeri bağlayabilirler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-288">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="c39ae-289">Aşağıdaki alt bileşen (`ChildComponent`) `Year` bir bileşen parametresine ve geri çağırmaya `YearChanged` sahiptir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-289">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="c39ae-290">`EventCallback<T>` [Eventcallback](#eventcallback) bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-290">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="c39ae-291">Aşağıdaki üst bileşen `ChildComponent` kullanır ve üst öğeden `ParentYear` parametresini alt bileşendeki `Year` parametresine bağlar:</span><span class="sxs-lookup"><span data-stu-id="c39ae-291">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="c39ae-292">`ParentComponent` yüklemek aşağıdaki biçimlendirmeyi üretir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-292">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="c39ae-293">`ParentYear` özelliğinin değeri `ParentComponent`düğme seçilerek değiştirilirse, `ChildComponent` `Year` özelliği güncellenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-293">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="c39ae-294">`Year` yeni değeri, `ParentComponent` yeniden eklendiğinde Kullanıcı arabiriminde işlenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-294">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="c39ae-295">`Year` parametresi, `Year` parametresinin türüyle eşleşen bir yardımcı `YearChanged` olayına sahip olduğundan bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-295">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="c39ae-296">Kurala göre `<ChildComponent @bind-Year="ParentYear" />` temelde yazmaya eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-296">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="c39ae-297">Genel olarak, bir özellik `@bind-property:event` özniteliği kullanılarak karşılık gelen bir olay işleyicisine bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-297">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="c39ae-298">Örneğin, özellik `MyProp` aşağıdaki iki öznitelik kullanılarak `MyEventHandler` bağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-298">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="c39ae-299">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="c39ae-299">Event handling</span></span>

<span data-ttu-id="c39ae-300">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-300">Razor components provide event handling features.</span></span> <span data-ttu-id="c39ae-301">`on{EVENT}` adlı bir HTML öğesi özniteliği için (örneğin, `onclick` ve `onsubmit`), temsilci türü belirtilmiş bir değer ile, Razor bileşenleri özniteliğin değerini bir olay işleyicisi olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-301">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="c39ae-302">Özniteliğin adı her zaman [`@on{EVENT}`](xref:mvc/views/razor#onevent)biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-302">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="c39ae-303">Aşağıdaki kod, Kullanıcı arabiriminde düğme seçildiğinde `UpdateHeading` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-303">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="c39ae-304">Aşağıdaki kod, Kullanıcı arabiriminde onay kutusu değiştirildiğinde `CheckChanged` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-304">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="c39ae-305">Olay işleyicileri Ayrıca zaman uyumsuz olabilir ve bir <xref:System.Threading.Tasks.Task>döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-305">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="c39ae-306">[Statehaschanged](xref:blazor/lifecycle#state-changes)el ile çağırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-306">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="c39ae-307">Özel durumlar oluştuğunda günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-307">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="c39ae-308">Aşağıdaki örnekte, düğme seçildiğinde `UpdateHeading` zaman uyumsuz olarak çağrılır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-308">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="c39ae-309">Olay bağımsız değişken türleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-309">Event argument types</span></span>

<span data-ttu-id="c39ae-310">Bazı olaylar için olay bağımsız değişkeni türlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-310">For some events, event argument types are permitted.</span></span> <span data-ttu-id="c39ae-311">Bu olay türlerinden birine erişim gerekmiyorsa, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-311">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="c39ae-312">Desteklenen `EventArgs` aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-312">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="c39ae-313">Olay</span><span class="sxs-lookup"><span data-stu-id="c39ae-313">Event</span></span>            | <span data-ttu-id="c39ae-314">Sınıf</span><span class="sxs-lookup"><span data-stu-id="c39ae-314">Class</span></span>                | <span data-ttu-id="c39ae-315">DOM olayları ve notları</span><span class="sxs-lookup"><span data-stu-id="c39ae-315">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="c39ae-316">Pano</span><span class="sxs-lookup"><span data-stu-id="c39ae-316">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="c39ae-317">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="c39ae-317">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="c39ae-318">Sürükle</span><span class="sxs-lookup"><span data-stu-id="c39ae-318">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="c39ae-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="c39ae-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="c39ae-320">`DataTransfer` ve `DataTransferItem` öğe verilerini sürüklemiş tutun.</span><span class="sxs-lookup"><span data-stu-id="c39ae-320">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="c39ae-321">Hata</span><span class="sxs-lookup"><span data-stu-id="c39ae-321">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="c39ae-322">Olay</span><span class="sxs-lookup"><span data-stu-id="c39ae-322">Event</span></span>            | `EventArgs`          | <span data-ttu-id="c39ae-323">*Genel*</span><span class="sxs-lookup"><span data-stu-id="c39ae-323">*General*</span></span><br><span data-ttu-id="c39ae-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="c39ae-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="c39ae-325">*Pano*</span><span class="sxs-lookup"><span data-stu-id="c39ae-325">*Clipboard*</span></span><br><span data-ttu-id="c39ae-326">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="c39ae-326">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="c39ae-327">*Giriş*</span><span class="sxs-lookup"><span data-stu-id="c39ae-327">*Input*</span></span><br><span data-ttu-id="c39ae-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="c39ae-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="c39ae-329">*Medyasını*</span><span class="sxs-lookup"><span data-stu-id="c39ae-329">*Media*</span></span><br><span data-ttu-id="c39ae-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="c39ae-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="c39ae-331">Odaklanma</span><span class="sxs-lookup"><span data-stu-id="c39ae-331">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="c39ae-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="c39ae-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="c39ae-333">`relatedTarget`için destek içermez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-333">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="c39ae-334">Giriş</span><span class="sxs-lookup"><span data-stu-id="c39ae-334">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="c39ae-335">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="c39ae-335">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="c39ae-336">Klavye</span><span class="sxs-lookup"><span data-stu-id="c39ae-336">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="c39ae-337">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="c39ae-337">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="c39ae-338">Fare</span><span class="sxs-lookup"><span data-stu-id="c39ae-338">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="c39ae-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="c39ae-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="c39ae-340">Fare işaretçisi</span><span class="sxs-lookup"><span data-stu-id="c39ae-340">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="c39ae-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="c39ae-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="c39ae-342">Fare tekeri</span><span class="sxs-lookup"><span data-stu-id="c39ae-342">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="c39ae-343">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="c39ae-343">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="c39ae-344">İlerleme durumu</span><span class="sxs-lookup"><span data-stu-id="c39ae-344">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="c39ae-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="c39ae-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="c39ae-346">Dokunmatik</span><span class="sxs-lookup"><span data-stu-id="c39ae-346">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="c39ae-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="c39ae-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="c39ae-348">`TouchPoint`, dokunmaya duyarlı bir cihazdaki tek bir iletişim noktasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c39ae-348">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="c39ae-349">Önceki tablodaki olayların özellikleri ve olay işleme davranışı hakkında bilgi için bkz. [başvuru kaynağında EventArgs sınıfları (ASPNET/AspNetCore Release/3.0 dalı)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="c39ae-349">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="c39ae-350">Lambda ifadeleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-350">Lambda expressions</span></span>

<span data-ttu-id="c39ae-351">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-351">Lambda expressions can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="c39ae-352">Genellikle, bir dizi öğe üzerinde yineleme yaparken olduğu gibi ek değerlerin üzerinde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-352">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="c39ae-353">Aşağıdaki örnek, her biri Kullanıcı arabiriminde seçildiğinde bir olay bağımsız değişkeni (`MouseEventArgs`) ve düğme numarası (`buttonNumber`) `UpdateHeading` çağıran üç düğme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c39ae-353">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="c39ae-354">Döngü değişkenini (`i`) doğrudan bir lambda ifadesinde bir `for` **döngüsünde kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="c39ae-354">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="c39ae-355">Aksi halde aynı değişken tüm lambda ifadeleri tarafından, `i`değerinin tüm Lambdalar ile aynı olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-355">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="c39ae-356">Her zaman değerini yerel bir değişkende (önceki örnekte`buttonNumber`) yakalayın ve sonra kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-356">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="c39ae-357">EventCallback</span><span class="sxs-lookup"><span data-stu-id="c39ae-357">EventCallback</span></span>

<span data-ttu-id="c39ae-358">İç içe bileşenler içeren yaygın bir senaryo, bir alt bileşen olayı gerçekleştiğinde bir üst bileşenin yöntemini çalıştırma, örneğin, alt öğe içinde bir `onclick` olayı oluştuğunda&mdash;.</span><span class="sxs-lookup"><span data-stu-id="c39ae-358">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="c39ae-359">Olayları bileşenler arasında göstermek için bir `EventCallback`kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-359">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="c39ae-360">Bir üst bileşen, bir alt bileşenin `EventCallback`bir geri çağırma yöntemi atayabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-360">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="c39ae-361">Örnek uygulamadaki `ChildComponent` (*Bileşenler/ChildComponent. Razor*), bir düğmenin `onclick` işleyicisinin, örneğin `ParentComponent`bir `EventCallback` temsilcisini almak üzere nasıl ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-361">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="c39ae-362">`EventCallback`, bir çevresel cihazdan `onclick` olayına uygun `MouseEventArgs`ile yazılır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-362">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="c39ae-363">`ParentComponent`, alt öğenin `EventCallback<T>` `ShowMessage` yöntemine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-363">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method.</span></span>

<span data-ttu-id="c39ae-364">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-364">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@messageText</b></p>

@code {
    private string messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="c39ae-365">`ChildComponent`düğme seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="c39ae-365">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="c39ae-366">`ParentComponent``ShowMessage` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-366">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="c39ae-367">`messageText` güncellenir ve `ParentComponent`görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-367">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="c39ae-368">Geri çağırma yönteminde (`ShowMessage`) [Statehaschanged](xref:blazor/lifecycle#state-changes) çağrısı gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-368">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="c39ae-369">`StateHasChanged`, alt olaylar, alt öğe içinde yürütülen olay işleyicilerinde bileşen rerendering tetiklenmesi gibi `ParentComponent`yeniden çalıştırmak için otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-369">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="c39ae-370">`EventCallback` ve `EventCallback<T>` zaman uyumsuz temsilcilere izin verir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-370">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="c39ae-371">`EventCallback<T>` kesin bir şekilde yazılır ve belirli bir bağımsız değişken türü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-371">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="c39ae-372">`EventCallback` zayıf ve bağımsız değişken türüne izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-372">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="c39ae-373">`InvokeAsync` ile bir `EventCallback` veya `EventCallback<T>` çağırın ve <xref:System.Threading.Tasks.Task>await:</span><span class="sxs-lookup"><span data-stu-id="c39ae-373">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="c39ae-374">Olay işleme ve bağlama bileşeni parametrelerini `EventCallback` ve `EventCallback<T>` kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-374">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="c39ae-375">`EventCallback`üzerinde türü kesin belirlenmiş `EventCallback<T>` tercih edin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-375">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="c39ae-376">`EventCallback<T>`, bileşenin kullanıcılarına daha iyi hata geri bildirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-376">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="c39ae-377">Diğer UI olay işleyicileriyle benzer şekilde, olay parametresini belirtmek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-377">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="c39ae-378">Geri çağırmaya hiçbir değer geçirilmemişse `EventCallback` kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-378">Use `EventCallback` when there's no value passed to the callback.</span></span>

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a><span data-ttu-id="c39ae-379">Varsayılan eylemleri engelle</span><span class="sxs-lookup"><span data-stu-id="c39ae-379">Prevent default actions</span></span>

<span data-ttu-id="c39ae-380">Bir olayın varsayılan eylemini engellemek için [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-380">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="c39ae-381">Giriş cihazında bir anahtar seçildiğinde ve öğe odağı bir metin kutusunda olduğunda, bir tarayıcı normalde metin kutusunda anahtarın karakterini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-381">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="c39ae-382">Aşağıdaki örnekte, `@onkeypress:preventDefault` Directive özniteliği belirtilerek varsayılan davranış engellenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-382">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="c39ae-383">Sayaç artar ve **+** anahtarı `<input>` öğenin değerine yakalanmaz:</span><span class="sxs-lookup"><span data-stu-id="c39ae-383">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="c39ae-384">`@on{EVENT}:preventDefault` özniteliğini bir değer olmadan belirtmek `@on{EVENT}:preventDefault="true"`eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-384">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="c39ae-385">Özniteliğin değeri de bir ifade olabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-385">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="c39ae-386">Aşağıdaki örnekte `_shouldPreventDefault`, `true` veya `false`olarak ayarlanan bir `bool` alandır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-386">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="c39ae-387">Varsayılan eylemi engellemek için bir olay işleyicisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-387">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="c39ae-388">Olay işleyicisi ve varsayılan eylem senaryolarına bağımsız olarak bir şekilde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-388">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="c39ae-389">Olay yaymayı durdur</span><span class="sxs-lookup"><span data-stu-id="c39ae-389">Stop event propagation</span></span>

<span data-ttu-id="c39ae-390">Olay yaymayı durdurmak için [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-390">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="c39ae-391">Aşağıdaki örnekte, onay kutusunun seçilmesi ikinci alt `<div>`, üst `<div>`yaymadan sonraki olayları engeller:</span><span class="sxs-lookup"><span data-stu-id="c39ae-391">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```

::: moniker-end

## <a name="chained-bind"></a><span data-ttu-id="c39ae-392">Zincirleme bağlama</span><span class="sxs-lookup"><span data-stu-id="c39ae-392">Chained bind</span></span>

<span data-ttu-id="c39ae-393">Yaygın bir senaryo, bir veri bağlama parametresini bileşen çıkışında bir sayfa öğesine zincirlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-393">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="c39ae-394">Birden çok bağlama düzeyi aynı anda gerçekleştiğinden, bu senaryoya *zincirleme bağlama* denir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-394">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="c39ae-395">Bir zincir bağlama, sayfanın öğesinde `@bind` sözdizimiyle uygulanamıyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-395">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="c39ae-396">Olay işleyicisi ve değeri ayrı olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-396">The event handler and value must be specified separately.</span></span> <span data-ttu-id="c39ae-397">Bununla birlikte, bir üst bileşen, bileşen parametresiyle `@bind` sözdizimini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-397">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="c39ae-398">Aşağıdaki `PasswordField` bileşeni (*Passwordfield. Razor*):</span><span class="sxs-lookup"><span data-stu-id="c39ae-398">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="c39ae-399">Bir `<input>` öğesinin değerini bir `Password` özelliğine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-399">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="c39ae-400">`Password` özelliğindeki değişiklikleri, bir [Eventcallback](#eventcallback)ile bir üst bileşene gösterir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-400">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="c39ae-401">`PasswordField` bileşeni başka bir bileşende kullanılır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-401">The `PasswordField` component is used in another component:</span></span>

```razor
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="c39ae-402">Önceki örnekteki parolada denetim veya tuzak hataları gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="c39ae-402">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="c39ae-403">`Password` için bir yedekleme alanı oluşturun (Aşağıdaki örnek kodda`password`).</span><span class="sxs-lookup"><span data-stu-id="c39ae-403">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="c39ae-404">`Password` ayarlayıcısı 'nda denetimleri veya yakalama hatalarını gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-404">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="c39ae-405">Aşağıdaki örnek, parolanın değerinde bir boşluk kullanılmışsa kullanıcıya anında geri bildirim sağlar:</span><span class="sxs-lookup"><span data-stu-id="c39ae-405">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="c39ae-406">Bileşenlere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="c39ae-406">Capture references to components</span></span>

<span data-ttu-id="c39ae-407">Bileşen başvuruları, bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe `Show` veya `Reset`gibi komutlar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-407">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="c39ae-408">Bir bileşen başvurusunu yakalamak için:</span><span class="sxs-lookup"><span data-stu-id="c39ae-408">To capture a component reference:</span></span>

* <span data-ttu-id="c39ae-409">Alt bileşene bir [`@ref`](xref:mvc/views/razor#ref) özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-409">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="c39ae-410">Alt bileşenle aynı türde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-410">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="c39ae-411">Bileşen işlendiğinde `loginDialog` alanı `MyLoginDialog` alt bileşen örneğiyle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-411">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="c39ae-412">Daha sonra bileşen örneğinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-412">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c39ae-413">`loginDialog` değişkeni yalnızca bileşen işlendikten sonra ve çıktısı `MyLoginDialog` öğesini içerdiğinde doldurulur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-413">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="c39ae-414">Bu noktaya kadar başvurulmasına hiçbir şey yok.</span><span class="sxs-lookup"><span data-stu-id="c39ae-414">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="c39ae-415">Bileşen işlemesini tamamladıktan sonra bileşen başvurularını işlemek için [Onafterrenderasync veya OnAfterRender yöntemlerini](xref:blazor/lifecycle#after-component-render)kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-415">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="c39ae-416">Bileşen başvurularını yakalama, [öğe başvurularını yakalamak](xref:blazor/javascript-interop#capture-references-to-elements)için benzer bir sözdizimi kullanın, bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-416">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="c39ae-417">Bileşen başvuruları yalnızca .NET kodunda kullanıldıkları&mdash;JavaScript koduna aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-417">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="c39ae-418">Alt bileşenlerin durumunu bulunmamalıdır için bileşen **başvurularını kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="c39ae-418">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="c39ae-419">Bunun yerine, alt bileşenlere veri geçirmek için normal bildirime dayalı parametreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-419">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="c39ae-420">Normal bildirime dayalı parametrelerin kullanımı, otomatik olarak doğru zamanların yeniden yönlendirmesi için alt bileşenlerde oluşur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-420">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="c39ae-421">Durumu güncelleştirmek için bileşen yöntemlerini dışarıdan çağır</span><span class="sxs-lookup"><span data-stu-id="c39ae-421">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="c39ae-422">, yürütmenin tek bir mantıksal iş parçacığını zorlamak için bir `SynchronizationContext` kullanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-422"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="c39ae-423">Bir bileşenin [yaşam döngüsü yöntemleri](xref:blazor/lifecycle) ve Blazor tarafından oluşturulan tüm olay geri çağırmaları bu `SynchronizationContext`yürütülür.</span><span class="sxs-lookup"><span data-stu-id="c39ae-423">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="c39ae-424">Bir bileşenin Zamanlayıcı veya diğer bildirimler gibi dış bir olaya göre güncellenmesi gerekir, Blazor`SynchronizationContext`dağıtım yapılacak `InvokeAsync` yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-424">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="c39ae-425">Örneğin, güncelleştirilmiş durumdaki herhangi bir dinleme bileşenine bildirimde bulunan bir *bildirim hizmeti* düşünün:</span><span class="sxs-lookup"><span data-stu-id="c39ae-425">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="c39ae-426">Bir bileşeni güncelleştirmek için `NotifierService` kullanımı:</span><span class="sxs-lookup"><span data-stu-id="c39ae-426">Usage of the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="c39ae-427">Yukarıdaki örnekte `NotifierService` bileşenin `OnNotify` yöntemini Blazor`SynchronizationContext`dışında çağırır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-427">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="c39ae-428">`InvokeAsync`, doğru bağlama geçmek ve bir işlemeyi kuyruğa almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-428">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="c39ae-429">Öğelerin ve bileşenlerin korunmasını denetlemek için \@anahtarını kullanın</span><span class="sxs-lookup"><span data-stu-id="c39ae-429">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="c39ae-430">Bir öğe veya bileşen listesi işlendiğinde ve öğeler ya da bileşenler daha sonra değiştiğinde, Blazoryayılma algoritması, önceki öğelerin veya bileşenlerin ne zaman tutulacağına ve model nesnelerinin bunlara nasıl eşleneceğine karar vermelidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-430">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="c39ae-431">Normalde, bu işlem otomatiktir ve yoksayılabilir, ancak işlemi denetlemek isteyebileceğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-431">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="c39ae-432">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c39ae-432">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="c39ae-433">`People` koleksiyonun içerikleri, ekli, silinmiş veya yeniden sıralanmış girdilerle değişebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-433">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="c39ae-434">Bileşen yeniden oluşturulduğunda `<DetailsEditor>` bileşeni farklı `Details` parametre değerleri almak için değişebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-434">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="c39ae-435">Bu, beklenenden daha karmaşık rerendering oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-435">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="c39ae-436">Bazı durumlarda rerendering, kayıp öğe odağı gibi görünür davranış farklılıklarına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-436">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="c39ae-437">Eşleme işlemi `@key` Directive özniteliğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-437">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="c39ae-438">`@key`, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için dağıtılmış algoritmaya neden olur:</span><span class="sxs-lookup"><span data-stu-id="c39ae-438">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="c39ae-439">`People` koleksiyonu değiştiğinde, yayılma algoritması `<DetailsEditor>` örnekleri ve `person` örnekleri arasındaki ilişkilendirmeyi korur:</span><span class="sxs-lookup"><span data-stu-id="c39ae-439">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="c39ae-440">Bir `Person` `People` listesinden silinirse, yalnızca ilgili `<DetailsEditor>` örneği kullanıcı arabiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-440">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="c39ae-441">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-441">Other instances are left unchanged.</span></span>
* <span data-ttu-id="c39ae-442">Listedeki bir konuma `Person` eklenirse, ilgili konuma bir yeni `<DetailsEditor>` örneği eklenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-442">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="c39ae-443">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-443">Other instances are left unchanged.</span></span>
* <span data-ttu-id="c39ae-444">`Person` girdileri yeniden sıralandıysanız, karşılık gelen `<DetailsEditor>` örnekleri korunur ve Kullanıcı arabiriminde yeniden sıralanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-444">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="c39ae-445">Bazı senaryolarda `@key` kullanımı, rerendering karmaşıklığını en aza indirir ve odak konumu gibi DOM 'ın durum bilgisi olan kısımlarıyla ilgili olası sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-445">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c39ae-446">Anahtarlar her kapsayıcı öğesi veya bileşeni için yereldir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-446">Keys are local to each container element or component.</span></span> <span data-ttu-id="c39ae-447">Anahtarlar belge genelinde küresel olarak karşılaştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-447">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="c39ae-448">\@anahtarı ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="c39ae-448">When to use \@key</span></span>

<span data-ttu-id="c39ae-449">Genellikle, bir liste işlendiğinde (örneğin, bir `@foreach` bloğunda) ve `@key`tanımlamak için uygun bir değer olduğunda `@key` kullanmak mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-449">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="c39ae-450">Ayrıca, bir nesne değiştiğinde Blazor bir öğeyi veya bileşen alt ağacını `@key` engellemek için de kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c39ae-450">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="c39ae-451">`@currentPerson` değişirse `@key` Attribute yönergesi Blazor `<div>` ve alt öğelerini atmayı ve yeni öğeler ve bileşenlerle Kullanıcı arabiriminde alt ağacı yeniden oluşturmayı zorlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-451">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="c39ae-452">`@currentPerson` değiştiğinde hiçbir Kullanıcı arabirimi durumunun korunmayacağını garanti etmeniz gerekirse bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-452">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="c39ae-453">\@anahtar ne zaman kullanılmaz</span><span class="sxs-lookup"><span data-stu-id="c39ae-453">When not to use \@key</span></span>

<span data-ttu-id="c39ae-454">`@key`bir performans maliyeti vardır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-454">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="c39ae-455">Performans maliyeti büyük değildir, ancak öğe veya bileşen koruma kurallarının uygulamanın avantajına göre denetlenmesi durumunda yalnızca `@key` belirtin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-455">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="c39ae-456">`@key` kullanılmasa bile, Blazor alt öğe ve bileşen örneklerini mümkün olduğunca korur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-456">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="c39ae-457">`@key` kullanmanın avantajı, model örneklerinin eşlemeyi seçme algoritması yerine, korunan bileşen örneklerine *nasıl* eşlendiğine ilişkin denetimdir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-457">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="c39ae-458">\@anahtarı için kullanılacak değerler</span><span class="sxs-lookup"><span data-stu-id="c39ae-458">What values to use for \@key</span></span>

<span data-ttu-id="c39ae-459">Genellikle, `@key`için aşağıdaki değer türlerinden birini sağlamak mantıklı olur:</span><span class="sxs-lookup"><span data-stu-id="c39ae-459">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="c39ae-460">Model nesne örnekleri (örneğin, önceki örnekte olduğu gibi `Person` örneği).</span><span class="sxs-lookup"><span data-stu-id="c39ae-460">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="c39ae-461">Bu, nesne başvurusu eşitliğine göre koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-461">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="c39ae-462">Benzersiz tanımlayıcılar (örneğin, `int`, `string`veya `Guid`için birincil anahtar değerleri).</span><span class="sxs-lookup"><span data-stu-id="c39ae-462">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="c39ae-463">`@key` için kullanılan değerlerin çakışmayın olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="c39ae-463">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="c39ae-464">Aynı üst öğe içinde çakışan değerler algılanırsa, eski öğeleri veya bileşenleri yeni öğe veya bileşenlere kesin bir şekilde eşlemediğinden Blazor bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-464">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="c39ae-465">Yalnızca nesne örnekleri veya birincil anahtar değerleri gibi farklı değerleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-465">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="c39ae-466">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="c39ae-466">Routing</span></span>

<span data-ttu-id="c39ae-467">Blazor yönlendirme, uygulamadaki her erişilebilir bileşene bir rota şablonu sağlayarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-467">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="c39ae-468">`@page` yönergesine sahip bir Razor dosyası derlendiğinde, oluşturulan sınıfa yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> verilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-468">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="c39ae-469">Çalışma zamanında, yönlendirici bileşen sınıflarını bir `RouteAttribute` arar ve hangi bileşenin istenen URL ile eşleşen bir rota şablonuna sahip olduğunu işler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-469">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="c39ae-470">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-470">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="c39ae-471">Aşağıdaki bileşen `/BlazorRoute` ve `/DifferentBlazorRoute`isteklerine yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-471">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="c39ae-472">*Pages/BlazorRoute. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-472">*Pages/BlazorRoute.razor*:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a><span data-ttu-id="c39ae-473">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-473">Route parameters</span></span>

<span data-ttu-id="c39ae-474">Bileşenler, `@page` yönergesinde belirtilen yol şablonundan yol parametreleri alabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-474">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="c39ae-475">Yönlendirici, karşılık gelen bileşen parametrelerini doldurmak için yol parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-475">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="c39ae-476">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-476">*Pages/RouteParameter.razor*:</span></span>

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

<span data-ttu-id="c39ae-477">İsteğe bağlı parametreler desteklenmez, bu nedenle yukarıdaki örnekte iki `@page` yönergesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-477">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="c39ae-478">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-478">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="c39ae-479">İkinci `@page` yönergesi `{text}` Route parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-479">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="c39ae-480">*Catch-all* parametre sözdizimi (`*`/`**`), birden çok klasör sınırları genelinde yolu yakalayan Razor bileşenlerinde ( *. Razor* **) desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="c39ae-480">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="c39ae-481">Kısmi sınıf desteği</span><span class="sxs-lookup"><span data-stu-id="c39ae-481">Partial class support</span></span>

<span data-ttu-id="c39ae-482">Razor bileşenleri kısmi sınıflar olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-482">Razor components are generated as partial classes.</span></span> <span data-ttu-id="c39ae-483">Razor bileşenleri aşağıdaki yaklaşımlardan birini kullanarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-483">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="c39ae-484">C#kod, tek bir dosyada HTML işaretlemesi ve Razor kodu ile bir [`@code`](xref:mvc/views/razor#code) bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-484">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="c39ae-485"> şablonlar, bu yaklaşımı kullanarak Razor bileşenlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-485"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="c39ae-486">C#kod, kısmi sınıf olarak tanımlanan bir arka plan kod dosyasına yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-486">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="c39ae-487">Aşağıdaki örnek, bir Blazor şablonundan oluşturulan bir uygulamada `@code` bloğu olan varsayılan `Counter` bileşenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-487">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="c39ae-488">HTML işaretleme, Razor kodu ve C# kod aynı dosyada:</span><span class="sxs-lookup"><span data-stu-id="c39ae-488">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="c39ae-489">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-489">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="c39ae-490">`Counter` bileşeni, kısmi bir sınıf içeren bir arka plan kod dosyası kullanılarak da oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-490">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="c39ae-491">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-491">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="c39ae-492">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-492">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

## <a name="specify-a-component-base-class"></a><span data-ttu-id="c39ae-493">Bir bileşen taban sınıfı belirtin</span><span class="sxs-lookup"><span data-stu-id="c39ae-493">Specify a component base class</span></span>

<span data-ttu-id="c39ae-494">`@inherits` yönergesi, bir bileşen için temel sınıf belirtmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-494">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="c39ae-495">[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) , bileşenin özelliklerini ve yöntemlerini sağlamak için bir bileşenin `BlazorRocksBase`temel sınıfı nasıl devralmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-495">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="c39ae-496">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-496">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="c39ae-497">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-497">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

<span data-ttu-id="c39ae-498">Temel sınıf `ComponentBase`türetmelidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-498">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="c39ae-499">Bileşenleri içeri aktar</span><span class="sxs-lookup"><span data-stu-id="c39ae-499">Import components</span></span>

<span data-ttu-id="c39ae-500">Razor ile yazılan bir bileşenin ad alanı temel alınarak belirlenir (öncelik sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="c39ae-500">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="c39ae-501">Razor dosyası ( *. Razor*) biçimlendirmesinde [`@namespace`](xref:mvc/views/razor#namespace) ataması (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="c39ae-501">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="c39ae-502">Projenin proje dosyasında `RootNamespace` (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="c39ae-502">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="c39ae-503">Proje dosyasının dosya adından ( *. csproj*) ve proje kökünden bileşen yolundan alınan proje adı.</span><span class="sxs-lookup"><span data-stu-id="c39ae-503">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="c39ae-504">Örneğin Framework, *{Project root}/Pages/Index.Razor* (*BlazorSample. csproj*) ad alanına `BlazorSample.Pages`çözümleniyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-504">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="c39ae-505">Bileşenler ad C# bağlama kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-505">Components follow C# name binding rules.</span></span> <span data-ttu-id="c39ae-506">Bu örnekteki `Index` bileşeni için, kapsamdaki bileşenler tüm bileşenlerdir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-506">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="c39ae-507">Aynı klasörde, *sayfalarda*.</span><span class="sxs-lookup"><span data-stu-id="c39ae-507">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="c39ae-508">Proje kökündeki, açıkça farklı bir ad alanı belirtmeyen bileşenler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-508">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="c39ae-509">Farklı bir ad alanında tanımlanan bileşenler, Razor 'nin [`@using`](xref:mvc/views/razor#using) yönergesi kullanılarak kapsam içine getirilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-509">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="c39ae-510">*BlazorSample/Shared/* klasöründe `NavMenu.razor`başka bir bileşen varsa, bileşen aşağıdaki `@using` ifadesiyle `Index.razor` kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-510">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="c39ae-511">Bileşenlere Ayrıca, [`@using`](xref:mvc/views/razor#using) yönergesini gerektirmeyen tam nitelikli adları kullanılarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-511">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="c39ae-512">`global::` niteleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-512">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="c39ae-513">Diğer ad `using` deyimleriyle bileşenleri içeri aktarma (örneğin, `@using Foo = Bar`) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-513">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="c39ae-514">Kısmen nitelenmiş adlar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-514">Partially qualified names aren't supported.</span></span> <span data-ttu-id="c39ae-515">Örneğin, `@using BlazorSample` eklemek ve `NavMenu.razor` başvurmak `<Shared.NavMenu></Shared.NavMenu>` desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-515">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="c39ae-516">Koşullu HTML öğesi öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-516">Conditional HTML element attributes</span></span>

<span data-ttu-id="c39ae-517">HTML öğesi öznitelikleri, .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-517">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="c39ae-518">Değer `false` veya `null`ise, öznitelik işlenmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-518">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="c39ae-519">Değer `true`ise, öznitelik küçültülmüş olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-519">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="c39ae-520">Aşağıdaki örnekte, `checked` öğenin biçimlendirmesinde işlenip işlenmeyeceğini `IsCompleted` belirler:</span><span class="sxs-lookup"><span data-stu-id="c39ae-520">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="c39ae-521">`IsCompleted` `true`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-521">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="c39ae-522">`IsCompleted` `false`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-522">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="c39ae-523">Daha fazla bilgi için bkz. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="c39ae-523">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="c39ae-524">.NET türü `bool`olduğunda, [Aria-basılan](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)gıbı bazı HTML öznitelikleri düzgün şekilde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-524">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="c39ae-525">Bu durumlarda, `bool`yerine `string` türü kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-525">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="c39ae-526">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="c39ae-526">Raw HTML</span></span>

<span data-ttu-id="c39ae-527">Dizeler normalde DOM metin düğümleri kullanılarak işlenir. Bu, içerdikleri tüm biçimlendirmenin yok sayıldığı ve değişmez değer olarak kabul edildiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-527">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="c39ae-528">Ham HTML işlemek için, HTML içeriğini bir `MarkupString` değerde sarın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-528">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="c39ae-529">Değer HTML veya SVG olarak ayrıştırılır ve DOM 'a eklenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-529">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="c39ae-530">Güvenilmeyen bir kaynaktan oluşturulan ham HTML işleme bir **güvenlik riskidir** ve kaçınılması gerekir!</span><span class="sxs-lookup"><span data-stu-id="c39ae-530">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="c39ae-531">Aşağıdaki örnek, bir bileşenin işlenmiş çıktısına statik HTML içeriği bloğunu eklemek için `MarkupString` türünü kullanmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-531">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="c39ae-532">Şablonlu bileşenler</span><span class="sxs-lookup"><span data-stu-id="c39ae-532">Templated components</span></span>

<span data-ttu-id="c39ae-533">Şablonlu bileşenler, bir veya daha fazla UI şablonunu parametre olarak kabul eden bileşenlerdir, daha sonra bileşen işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-533">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="c39ae-534">Şablonlu bileşenler, normal bileşenlerden daha yeniden kullanılabilir olan üst düzey bileşenleri yazmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-534">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="c39ae-535">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-535">A couple of examples include:</span></span>

* <span data-ttu-id="c39ae-536">Kullanıcının tablo üst bilgisi, satırları ve altbilgisi için şablon belirtmesini sağlayan tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="c39ae-536">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="c39ae-537">Bir kullanıcının bir listedeki öğeleri işlemek için şablon belirlemesine izin veren bir liste bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-537">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="c39ae-538">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-538">Template parameters</span></span>

<span data-ttu-id="c39ae-539">Şablonlu bir bileşen, `RenderFragment` veya `RenderFragment<T>`türünde bir veya daha fazla bileşen parametresi belirtilerek tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-539">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="c39ae-540">Bir işleme parçası, işlenecek Kullanıcı arabiriminin bir kesimini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c39ae-540">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="c39ae-541">`RenderFragment<T>`, işleme parçası çağrıldığında belirtilebildiği bir tür parametresi alır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-541">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="c39ae-542">`TableTemplate` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="c39ae-542">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="c39ae-543">Şablonlu bir bileşen kullanırken, şablon parametreleri parametre adlarıyla eşleşen alt öğeler (`TableHeader` ve aşağıdaki örnekte `RowTemplate`) kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-543">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="c39ae-544">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-544">Template context parameters</span></span>

<span data-ttu-id="c39ae-545">Öğe olarak geçirilen `RenderFragment<T>` bileşen bağımsız değişkenleri `context` adında örtük bir parametreye sahiptir (örneğin, yukarıdaki kod örneğinden, `@context.PetId`), ancak parametre adını alt öğe üzerindeki `Context` özniteliğini kullanarak değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-545">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="c39ae-546">Aşağıdaki örnekte, `RowTemplate` öğenin `Context` özniteliği `pet` parametresini belirtir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-546">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="c39ae-547">Alternatif olarak, bileşen öğesinde `Context` özniteliğini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-547">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="c39ae-548">Belirtilen `Context` özniteliği belirtilen tüm şablon parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-548">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="c39ae-549">Bu, örtük alt içerik (herhangi bir sarmalama alt öğesi olmadan) için içerik parametre adını belirtmek istediğinizde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-549">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="c39ae-550">Aşağıdaki örnekte, `Context` özniteliği `TableTemplate` öğesinde görünür ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-550">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="c39ae-551">Genel olarak yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="c39ae-551">Generic-typed components</span></span>

<span data-ttu-id="c39ae-552">Şablonlu bileşenler çoğunlukla genel olarak türdedir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-552">Templated components are often generically typed.</span></span> <span data-ttu-id="c39ae-553">Örneğin, bir genel `ListViewTemplate` bileşeni `IEnumerable<T>` değerlerini işlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-553">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="c39ae-554">Genel bir bileşen tanımlamak için [`@typeparam`](xref:mvc/views/razor#typeparam) yönergesini kullanarak tür parametrelerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="c39ae-554">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="c39ae-555">Genel türsüz bileşenleri kullanırken tür parametresi mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-555">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="c39ae-556">Aksi halde tür parametresi, tür parametresinin adıyla eşleşen bir öznitelik kullanılarak açıkça belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-556">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="c39ae-557">Aşağıdaki örnekte, `TItem="Pet"` türü belirtir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-557">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="c39ae-558">Değerleri ve parametreleri basamaklama</span><span class="sxs-lookup"><span data-stu-id="c39ae-558">Cascading values and parameters</span></span>

<span data-ttu-id="c39ae-559">Bazı senaryolarda, özellikle birden çok bileşen katmanı olduğunda, [bileşen parametreleri](#component-parameters)kullanarak bir üst bileşenden bir alt bileşene veri akışı yapmak uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-559">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="c39ae-560">Değerleri ve parametreleri basamaklama, bir üst bileşenin tüm alt bileşenlerine değer sağlaması için kullanışlı bir yol sağlayarak bu sorunu çözebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-560">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="c39ae-561">Basamaklı değerler ve parametreler, bileşenlerin koordinasyonu için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-561">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="c39ae-562">Tema örneği</span><span class="sxs-lookup"><span data-stu-id="c39ae-562">Theme example</span></span>

<span data-ttu-id="c39ae-563">Örnek uygulamadan aşağıdaki örnekte, `ThemeInfo` sınıfı, uygulamanın belirli bir bölümündeki tüm düğmelerin aynı stili paylaştığı şekilde bileşen hiyerarşisinin akışını yapmak için tema bilgilerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-563">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="c39ae-564">*Uıthemeclasses/Themeınfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="c39ae-564">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="c39ae-565">Bir üst bileşen basamaklı değer bileşeni kullanılarak basamaklı bir değer sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-565">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="c39ae-566">`CascadingValue` bileşeni, bileşen hiyerarşisinin bir alt ağacını sarmalanmış ve bu alt ağaçta bulunan tüm bileşenlere tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-566">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="c39ae-567">Örneğin, örnek uygulama, `@Body` özelliğinin düzen gövdesini oluşturan tüm bileşenler için bir geçişli parametre olarak uygulamanın düzenlerindeki tema bilgilerini (`ThemeInfo`) belirtir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-567">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="c39ae-568">`ButtonClass`, düzen bileşeninde `btn-success` bir değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-568">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="c39ae-569">Tüm alt bileşenler, `ThemeInfo` basamaklı nesne aracılığıyla bu özelliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-569">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="c39ae-570">`CascadingValuesParametersLayout` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="c39ae-570">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="c39ae-571">Basamaklı değerleri kullanmak için, bileşenler `[CascadingParameter]` özniteliği kullanarak Geçişli Parametreler bildirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-571">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="c39ae-572">Basamaklı değerler, türe göre basamaklı parametrelere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-572">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="c39ae-573">Örnek uygulamada `CascadingValuesParametersTheme` bileşeni, `ThemeInfo` geçişli değeri basamaklı bir parametreye bağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-573">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="c39ae-574">Parametresi, bileşen tarafından görünen düğmelerden birine ait CSS sınıfını ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-574">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="c39ae-575">`CascadingValuesParametersTheme` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="c39ae-575">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="c39ae-576">Aynı alt ağaç içindeki aynı türdeki birden çok değeri basamakla, her bir `CascadingValue` bileşenine ve karşılık gelen `CascadingParameter`benzersiz bir `Name` dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-576">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="c39ae-577">Aşağıdaki örnekte, iki `CascadingValue` bileşeni, `MyCascadingType` farklı örneklerini ada göre basamakla:</span><span class="sxs-lookup"><span data-stu-id="c39ae-577">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="c39ae-578">Alt bileşende, basamaklı parametreler değerlerini, üst bileşendeki ilgili basamaklı değerleri ada göre alır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-578">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="c39ae-579">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="c39ae-579">TabSet example</span></span>

<span data-ttu-id="c39ae-580">Basamaklı parametreler, bileşenlerin bileşen hiyerarşisinde işbirliği yapmasına de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-580">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="c39ae-581">Örneğin, örnek uygulamada aşağıdaki *Tabset* örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="c39ae-581">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="c39ae-582">Örnek uygulama, sekmelerin uygulandığı bir `ITab` arabirimine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-582">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="c39ae-583">`CascadingValuesParametersTabSet` bileşeni, çeşitli `Tab` bileşenleri içeren `TabSet` bileşenini kullanır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-583">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="c39ae-584">Alt `Tab` bileşenleri, `TabSet`açıkça parametre olarak geçirilmemektedir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-584">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="c39ae-585">Bunun yerine, alt `Tab` bileşenleri `TabSet`alt içeriğinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-585">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="c39ae-586">Ancak, `TabSet` üst bilgileri ve etkin sekmeyi işleyebilmesi için, her bir `Tab` bileşeni hakkında yine de bilmesi gerekir. Ek kod gerektirmeden bu koordinasyonu etkinleştirmek için, `TabSet` bileşen kendisini, alt `Tab` bileşenleri tarafından çekilen *basamaklı bir değer olarak sağlayabilir* .</span><span class="sxs-lookup"><span data-stu-id="c39ae-586">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="c39ae-587">`TabSet` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="c39ae-587">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="c39ae-588">Alt `Tab` bileşenleri, kapsayan `TabSet` kendisini basamaklı bir parametre olarak yakalar, bu nedenle `Tab` bileşenleri kendilerini `TabSet` ve bu sekmenin etkin olduğu koordine eder.</span><span class="sxs-lookup"><span data-stu-id="c39ae-588">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="c39ae-589">`Tab` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="c39ae-589">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="c39ae-590">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="c39ae-590">Razor templates</span></span>

<span data-ttu-id="c39ae-591">Oluşturma parçaları Razor şablonu sözdizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-591">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="c39ae-592">Razor şablonları, bir UI parçacığı tanımlamak ve aşağıdaki biçimi varsaymak için bir yoldur:</span><span class="sxs-lookup"><span data-stu-id="c39ae-592">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="c39ae-593">Aşağıdaki örnek, `RenderFragment` ve `RenderFragment<T>` değerlerinin nasıl belirtildiğini ve şablonlarının doğrudan bir bileşende nasıl işleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-593">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="c39ae-594">Oluşturma parçaları, [şablonlu bileşenlere](#templated-components)bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-594">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```razor
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="c39ae-595">Önceki kodun işlenmiş çıktısı:</span><span class="sxs-lookup"><span data-stu-id="c39ae-595">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="c39ae-596">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="c39ae-596">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="c39ae-597">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`, bileşenleri ve öğeleri işlemek için yöntemler sağlar ve C# kodu kodda el ile oluşturma dahil.</span><span class="sxs-lookup"><span data-stu-id="c39ae-597">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="c39ae-598">Bileşen oluşturmak için `RenderTreeBuilder` kullanımı gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-598">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="c39ae-599">Hatalı biçimlendirilmiş bir bileşen (örneğin, kapatılmamış bir biçimlendirme etiketi) tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-599">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="c39ae-600">Başka bir bileşende el ile yerleşik olarak kullanılabilecek aşağıdaki `PetDetails` bileşenini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c39ae-600">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="c39ae-601">Aşağıdaki örnekte, `CreateComponent` yöntemindeki döngü üç `PetDetails` bileşeni oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-601">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="c39ae-602">Bileşenleri oluşturmak için `RenderTreeBuilder` Yöntemler çağrılırken (`OpenComponent` ve `AddAttribute`), dizi numaraları kaynak kodu satır numaralarıdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-602">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="c39ae-603">Blazor farkı algoritması, ayrı çağrı etkinleştirmeleri değil, farklı kod satırlarına karşılık gelen sıra numaralarına dayanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-603">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="c39ae-604">`RenderTreeBuilder` yöntemlerle bir bileşen oluştururken, dizi numaraları için bağımsız değişkenleri sabit olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-604">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="c39ae-605">**Sıra numarasını oluşturmak için bir hesaplama veya sayaç kullanmak kötü performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="c39ae-605">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="c39ae-606">Daha fazla bilgi için bkz. [kod satırı numaralarıyla Ilgili sıra numaraları ve yürütme sırası çalışmıyor](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c39ae-606">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="c39ae-607">`BuiltContent` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="c39ae-607">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="c39ae-608">`Microsoft.AspNetCore.Components.RenderTree` türler, işleme işlemlerinin *sonuçlarının* işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-608">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="c39ae-609">Bunlar Blazor Framework uygulamasının iç ayrıntılardır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-609">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="c39ae-610">Bu türlerin *dengesizleşilmesi* ve gelecekteki sürümlerde değişikliğe tabi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-610">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="c39ae-611">Sıra numaraları, kod satırı numaralarıyla ilgilidir ve yürütme sırası değildir</span><span class="sxs-lookup"><span data-stu-id="c39ae-611">Sequence numbers relate to code line numbers and not execution order</span></span>

Blazor<span data-ttu-id="c39ae-612"> `.razor` dosyalar her zaman derlenir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-612"> `.razor` files are always compiled.</span></span> <span data-ttu-id="c39ae-613">Derleme adımı, çalışma zamanında uygulama performansını geliştiren bilgileri eklemek için kullanılabilir olduğundan, bu büyük olasılıkla `.razor` için harika bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-613">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="c39ae-614">Bu geliştirmelerin önemli bir örneği *sıra numarası*içerir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-614">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="c39ae-615">Sıra numaraları, hangi çıkışların ayrı ve sıralı kod satırlarından geldiğini çalışma zamanına işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="c39ae-615">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="c39ae-616">Çalışma zamanı, doğrusal bir zamanda, genel ağaç farkı algoritması için genellikle mümkün olandan çok daha hızlı olan etkili ağaç SLA 'ları oluşturmak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-616">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="c39ae-617">Aşağıdaki Razor bileşeni ( *. Razor*) dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c39ae-617">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="c39ae-618">Yukarıdaki kod, aşağıdakine benzer şekilde derlenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-618">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="c39ae-619">Kod ilk kez yürütüldüğünde, `someFlag` `true`, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-619">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="c39ae-620">Sequence</span><span class="sxs-lookup"><span data-stu-id="c39ae-620">Sequence</span></span> | <span data-ttu-id="c39ae-621">Tür</span><span class="sxs-lookup"><span data-stu-id="c39ae-621">Type</span></span>      | <span data-ttu-id="c39ae-622">Veri</span><span class="sxs-lookup"><span data-stu-id="c39ae-622">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="c39ae-623">0</span><span class="sxs-lookup"><span data-stu-id="c39ae-623">0</span></span>        | <span data-ttu-id="c39ae-624">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="c39ae-624">Text node</span></span> | <span data-ttu-id="c39ae-625">Birinci</span><span class="sxs-lookup"><span data-stu-id="c39ae-625">First</span></span>  |
| <span data-ttu-id="c39ae-626">1\.</span><span class="sxs-lookup"><span data-stu-id="c39ae-626">1</span></span>        | <span data-ttu-id="c39ae-627">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="c39ae-627">Text node</span></span> | <span data-ttu-id="c39ae-628">Saniye</span><span class="sxs-lookup"><span data-stu-id="c39ae-628">Second</span></span> |

<span data-ttu-id="c39ae-629">`someFlag` `false`hale geldiğini ve biçimlendirmenin yeniden işleneceğini varsayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-629">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="c39ae-630">Bu kez, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="c39ae-630">This time, the builder receives:</span></span>

| <span data-ttu-id="c39ae-631">Sequence</span><span class="sxs-lookup"><span data-stu-id="c39ae-631">Sequence</span></span> | <span data-ttu-id="c39ae-632">Tür</span><span class="sxs-lookup"><span data-stu-id="c39ae-632">Type</span></span>       | <span data-ttu-id="c39ae-633">Veri</span><span class="sxs-lookup"><span data-stu-id="c39ae-633">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="c39ae-634">1\.</span><span class="sxs-lookup"><span data-stu-id="c39ae-634">1</span></span>        | <span data-ttu-id="c39ae-635">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="c39ae-635">Text node</span></span>  | <span data-ttu-id="c39ae-636">Saniye</span><span class="sxs-lookup"><span data-stu-id="c39ae-636">Second</span></span> |

<span data-ttu-id="c39ae-637">Çalışma zamanı bir fark gerçekleştirdiğinde, sırasıyla `0` öğenin kaldırıldığını görür, bu nedenle aşağıdaki önemsiz *düzenleme betiğini*oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c39ae-637">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="c39ae-638">İlk metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-638">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="c39ae-639">Program aracılığıyla sıra numaraları oluşturursanız ne yanlış gider</span><span class="sxs-lookup"><span data-stu-id="c39ae-639">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="c39ae-640">Aşağıdaki işleme Ağacı Oluşturucusu mantığını yazmanız yerine düşünün:</span><span class="sxs-lookup"><span data-stu-id="c39ae-640">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="c39ae-641">Şimdi ilk çıktı:</span><span class="sxs-lookup"><span data-stu-id="c39ae-641">Now, the first output is:</span></span>

| <span data-ttu-id="c39ae-642">Sequence</span><span class="sxs-lookup"><span data-stu-id="c39ae-642">Sequence</span></span> | <span data-ttu-id="c39ae-643">Tür</span><span class="sxs-lookup"><span data-stu-id="c39ae-643">Type</span></span>      | <span data-ttu-id="c39ae-644">Veri</span><span class="sxs-lookup"><span data-stu-id="c39ae-644">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="c39ae-645">0</span><span class="sxs-lookup"><span data-stu-id="c39ae-645">0</span></span>        | <span data-ttu-id="c39ae-646">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="c39ae-646">Text node</span></span> | <span data-ttu-id="c39ae-647">Birinci</span><span class="sxs-lookup"><span data-stu-id="c39ae-647">First</span></span>  |
| <span data-ttu-id="c39ae-648">1\.</span><span class="sxs-lookup"><span data-stu-id="c39ae-648">1</span></span>        | <span data-ttu-id="c39ae-649">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="c39ae-649">Text node</span></span> | <span data-ttu-id="c39ae-650">Saniye</span><span class="sxs-lookup"><span data-stu-id="c39ae-650">Second</span></span> |

<span data-ttu-id="c39ae-651">Bu sonuç önceki bir durum ile aynıdır, bu nedenle olumsuz bir sorun yoktur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-651">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="c39ae-652">`someFlag` ikinci işleme `false` ve çıktı:</span><span class="sxs-lookup"><span data-stu-id="c39ae-652">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="c39ae-653">Sequence</span><span class="sxs-lookup"><span data-stu-id="c39ae-653">Sequence</span></span> | <span data-ttu-id="c39ae-654">Tür</span><span class="sxs-lookup"><span data-stu-id="c39ae-654">Type</span></span>      | <span data-ttu-id="c39ae-655">Veri</span><span class="sxs-lookup"><span data-stu-id="c39ae-655">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="c39ae-656">0</span><span class="sxs-lookup"><span data-stu-id="c39ae-656">0</span></span>        | <span data-ttu-id="c39ae-657">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="c39ae-657">Text node</span></span> | <span data-ttu-id="c39ae-658">Saniye</span><span class="sxs-lookup"><span data-stu-id="c39ae-658">Second</span></span> |

<span data-ttu-id="c39ae-659">Bu kez, fark algoritması *iki* değişikliğin oluştuğunu görür ve algoritma aşağıdaki düzenleme betiğini üretir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-659">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="c39ae-660">İlk metin düğümünün değerini `Second`olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-660">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="c39ae-661">İkinci metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-661">Remove the second text node.</span></span>

<span data-ttu-id="c39ae-662">Sıra numaralarının oluşturulması, özgün kodda `if/else` dalların ve döngülerin bulunduğu yer hakkındaki tüm yararlı bilgileri kaybetti.</span><span class="sxs-lookup"><span data-stu-id="c39ae-662">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="c39ae-663">Bu, daha önce olduğu gibi bir fark **ile iki kez** sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-663">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="c39ae-664">Bu, önemsiz bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-664">This is a trivial example.</span></span> <span data-ttu-id="c39ae-665">Karmaşık ve derin iç içe yapıları ve özellikle döngülerle daha gerçekçi durumlarda performans maliyeti daha önemlidir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-665">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="c39ae-666">Hangi döngü bloklarının veya dallarının eklendiğini veya kaldırıldığını hemen belirlemek yerine, fark algoritmasının işleme ağaçlarına katmaları ve genellikle eski ve yeni yapıların nasıl olduğu hakkında yanlış bir biçimde düzenleme betikleri oluşturması birbirleriyle ilişkilendir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-666">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="c39ae-667">Kılavuz ve ekibinizle</span><span class="sxs-lookup"><span data-stu-id="c39ae-667">Guidance and conclusions</span></span>

* <span data-ttu-id="c39ae-668">Sıra numaraları dinamik olarak oluşturulursa uygulama performansı de vardır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-668">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="c39ae-669">Altyapı, derleme zamanında yakalanmadığı takdirde gerekli bilgiler bulunmadığından, çalışma zamanında kendi sıra numaralarını otomatik olarak oluşturamaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-669">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="c39ae-670">El ile uygulanan `RenderTreeBuilder` mantığı uzun blokları yazmayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-670">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="c39ae-671">`.razor` dosyalarını tercih edin ve derleyicinin sıra numaralarıyla uğraşmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="c39ae-671">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="c39ae-672">El ile `RenderTreeBuilder` mantığın olmaması durumunda, uzun kod bloklarını `OpenRegion`/`CloseRegion` çağrılarına kaydırılmış daha küçük parçalara ayırın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-672">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="c39ae-673">Her bölge kendi ayrı dizi numaralarına sahiptir, bu nedenle her bölge içinde sıfırdan (veya herhangi bir rastgele sayıdan) yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-673">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="c39ae-674">Dizi numaraları sabit kodluysa, fark algoritması yalnızca değer değerinde sıra numaralarının artırılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-674">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="c39ae-675">İlk değer ve boşluklar ilgisiz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-675">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="c39ae-676">Tek bir seçenek, kod satırı numarasını sıra numarası olarak kullanmak veya sıfırdan başlayıp bir ya da yüzlerce (ya da tercih edilen aralığa) artırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-676">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="c39ae-677"> sıra numaralarını kullanır, diğer ağaç dağıtma Kullanıcı arabirimi çerçeveleri bunları kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-677"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="c39ae-678">Dizi numaraları kullanıldığında, yayılma çok daha hızlıdır ve Blazor, *. Razor* dosyaları yazan geliştiriciler için otomatik olarak sıra numaralarıyla ilgilenen bir derleme adımının avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-678">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="c39ae-679">Yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="c39ae-679">Localization</span></span>

Blazor<span data-ttu-id="c39ae-680"> Server uygulamaları, [Yerelleştirme ara yazılımı](xref:fundamentals/localization#localization-middleware)kullanılarak yerelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-680"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="c39ae-681">Ara yazılım, uygulamadan kaynak isteyen kullanıcılar için uygun kültürü seçer.</span><span class="sxs-lookup"><span data-stu-id="c39ae-681">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="c39ae-682">Kültür aşağıdaki yaklaşımlardan biri kullanılarak ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-682">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="c39ae-683">Çerezler</span><span class="sxs-lookup"><span data-stu-id="c39ae-683">Cookies</span></span>](#cookies)
* [<span data-ttu-id="c39ae-684">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="c39ae-684">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="c39ae-685">Daha fazla bilgi ve örnek için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="c39ae-685">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="c39ae-686">Bağlayıcıyı Uluslararası hale getirme için yapılandırma (Blazor WebAssembly)</span><span class="sxs-lookup"><span data-stu-id="c39ae-686">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="c39ae-687">Varsayılan olarak, Blazor WebAssembly uygulamaları için Blazorbağlayıcı yapılandırması, açıkça istenen yerel ayarlar dışında uluslararası duruma getirme bilgilerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-687">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="c39ae-688">Bağlayıcının davranışını denetleme hakkında daha fazla bilgi ve yönergeler için bkz. <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="c39ae-688">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="c39ae-689">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="c39ae-689">Cookies</span></span>

<span data-ttu-id="c39ae-690">Yerelleştirme kültürü tanımlama bilgisi kullanıcının kültürünü kalıcı hale getirebilirler.</span><span class="sxs-lookup"><span data-stu-id="c39ae-690">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="c39ae-691">Tanımlama bilgisi, uygulamanın ana bilgisayar sayfasının `OnGet` yöntemiyle oluşturulur (*Pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="c39ae-691">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="c39ae-692">Yerelleştirme ara yazılımı, sonraki isteklerde Kullanıcı kültürünü ayarlamak için tanımlama bilgilerini okur.</span><span class="sxs-lookup"><span data-stu-id="c39ae-692">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="c39ae-693">Tanımlama bilgisinin kullanımı, WebSocket bağlantısının kültürü doğru şekilde yaymasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-693">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="c39ae-694">Yerelleştirme şemaları URL yolunu veya sorgu dizesini temel alıyorsa, düzen WebSockets ile çalışmayabilir, bu nedenle kültürü kalıcı hale getiremeyebilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-694">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="c39ae-695">Bu nedenle, yerelleştirme kültürü tanımlama bilgisinin kullanılması önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-695">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="c39ae-696">Kültür bir yerelleştirme tanımlama bilgisinde kalıcı hale getirilir kültür atamak için herhangi bir teknik kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-696">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="c39ae-697">Uygulamanın zaten sunucu tarafı ASP.NET Core için bir yerelleştirme şeması varsa, uygulamanın var olan yerelleştirme altyapısını kullanmaya devam edin ve uygulamanın şeması içinde yerelleştirme kültür tanımlama bilgisini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-697">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="c39ae-698">Aşağıdaki örnekte, yerelleştirme ara yazılımı tarafından okunabilen bir tanımlama bilgisinde geçerli kültürün nasıl ayarlanacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-698">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="c39ae-699">Blazor Server uygulamasında aşağıdaki içeriklerle bir *Pages/Host. cshtml. cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="c39ae-699">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="c39ae-700">Yerelleştirme uygulamada işlenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-700">Localization is handled in the app:</span></span>

1. <span data-ttu-id="c39ae-701">Tarayıcı, uygulamaya bir ilk HTTP isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-701">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="c39ae-702">Kültür, yerelleştirme ara yazılımı tarafından atanır.</span><span class="sxs-lookup"><span data-stu-id="c39ae-702">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="c39ae-703">*_Host. cshtml. cs* içindeki `OnGet` yöntemi, yanıtın bir parçası olarak bir tanımlama bilgisinde kültürü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-703">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="c39ae-704">Tarayıcı, etkileşimli bir Blazor sunucusu oturumu oluşturmak için bir WebSocket bağlantısı açar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-704">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="c39ae-705">Yerelleştirme ara yazılımı tanımlama bilgisini okur ve kültürü atar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-705">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="c39ae-706">Blazor Server oturumu doğru kültür ile başlar.</span><span class="sxs-lookup"><span data-stu-id="c39ae-706">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="c39ae-707">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="c39ae-707">Provide UI to choose the culture</span></span>

<span data-ttu-id="c39ae-708">Bir kullanıcının bir kültür seçmesine izin vermek için kullanıcı ARABIRIMI sağlamak üzere bir *yeniden yönlendirme tabanlı yaklaşım* önerilir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-708">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="c39ae-709">Bu işlem, bir Kullanıcı bir oturum açma sayfasına yeniden yönlendirildiği ve ardından özgün kaynağa yeniden yönlendirildiği&mdash;bir Kullanıcı güvenli bir kaynağa erişmeyi denediğinde bir Web uygulamasında gerçekleşmelere benzer.</span><span class="sxs-lookup"><span data-stu-id="c39ae-709">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="c39ae-710">Uygulama, bir denetleyiciye yeniden yönlendirme yoluyla kullanıcının seçili kültürünü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-710">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="c39ae-711">Denetleyici kullanıcının seçili kültürünü bir tanımlama bilgisine ayarlar ve kullanıcıyı özgün URI 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-711">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="c39ae-712">Bir tanımlama bilgisinde kullanıcının seçili kültürünü ayarlamak ve özgün URI 'ye yeniden yönlendirmeyi gerçekleştirmek için sunucuda bir HTTP uç noktası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="c39ae-712">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="c39ae-713">Açık yeniden yönlendirme saldırılarını engellemek için `LocalRedirect` eylemi sonucunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-713">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="c39ae-714">Daha fazla bilgi için bkz. <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="c39ae-714">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="c39ae-715">Aşağıdaki bileşen, Kullanıcı bir kültür seçtiğinde ilk yeniden yönlendirmenin nasıl gerçekleştirileceği hakkında bir örnek göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-715">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="c39ae-716">Blazor uygulamalarında .NET yerelleştirme senaryolarını kullanma</span><span class="sxs-lookup"><span data-stu-id="c39ae-716">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="c39ae-717">Blazor uygulamalar içinde, aşağıdaki .NET yerelleştirme ve genelleştirme senaryoları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-717">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="c39ae-718">. NET ' in kaynak sistemi</span><span class="sxs-lookup"><span data-stu-id="c39ae-718">.NET's resources system</span></span>
* <span data-ttu-id="c39ae-719">Kültüre özgü sayı ve Tarih biçimlendirmesi</span><span class="sxs-lookup"><span data-stu-id="c39ae-719">Culture-specific number and date formatting</span></span>

Blazor<span data-ttu-id="c39ae-720">`@bind` işlevselliği, kullanıcının geçerli kültürüne göre Genelleştirme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-720">'s `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="c39ae-721">Daha fazla bilgi için [veri bağlama](#data-binding) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="c39ae-721">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="c39ae-722">Sınırlı bir ASP.NET Core yerelleştirme senaryosu kümesi şu anda desteklenmektedir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-722">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="c39ae-723">`IStringLocalizer<>` Blazor uygulamalarda *desteklenir* .</span><span class="sxs-lookup"><span data-stu-id="c39ae-723">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="c39ae-724">`IHtmlLocalizer<>`, `IViewLocalizer<>`ve veri ek açıklamaları yerelleştirme ASP.NET Core MVC senaryolarından ve Blazor uygulamalarında **desteklenmez** .</span><span class="sxs-lookup"><span data-stu-id="c39ae-724">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="c39ae-725">Daha fazla bilgi için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="c39ae-725">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="c39ae-726">Ölçeklenebilir vektör grafik (SVG) görüntüleri</span><span class="sxs-lookup"><span data-stu-id="c39ae-726">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="c39ae-727">Blazor HTML oluşturduğundan, ölçeklenebilir vektör grafik (SVG) görüntüleri ( *. SVG*) dahil olmak üzere tarayıcıda desteklenen görüntüler `<img>` etiketi aracılığıyla desteklenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-727">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="c39ae-728">Benzer şekilde, SVG görüntüleri bir stil sayfası dosyasının ( *. css*) CSS kurallarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="c39ae-728">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="c39ae-729">Ancak, satır içi SVG işaretlemesi tüm senaryolarda desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="c39ae-729">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="c39ae-730">Bir `<svg>` etiketini doğrudan bir bileşen dosyasına ( *. Razor*) yerleştirirseniz, temel görüntü işleme desteklenir, ancak birçok gelişmiş senaryo desteklenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-730">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="c39ae-731">Örneğin, `<use>` Etiketler Şu anda dikkate alınamaz ve `@bind` bazı SVG etiketleriyle kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-731">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="c39ae-732">Gelecekteki bir sürümde bu sınırlamaları ele almayı bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="c39ae-732">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c39ae-733">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c39ae-733">Additional resources</span></span>

* <span data-ttu-id="c39ae-734"><xref:security/blazor/server> &ndash;, kaynak tükenmesi ile Çekişmek zorunda olan Blazor sunucu uygulamaları oluşturmaya yönelik yönergeler Içerir.</span><span class="sxs-lookup"><span data-stu-id="c39ae-734"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
