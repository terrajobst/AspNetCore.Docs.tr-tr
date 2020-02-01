---
title: ASP.NET Core Razor bileşenleri oluşturma ve kullanma
author: guardrex
description: Veri bağlama, olayları işleme ve bileşen yaşam döngülerini yönetme dahil Razor bileşenleri oluşturmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: d6ba60b20d21636c7f780a80d8fbdb152505a3a3
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928254"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="93b3f-103">ASP.NET Core Razor bileşenleri oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="93b3f-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="93b3f-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="93b3f-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="93b3f-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93b3f-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="93b3f-106">Blazor uygulamaları, *bileşenleri*kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="93b3f-107">Bir bileşen, bir sayfa, iletişim veya form gibi bir kullanıcı arabirimi (UI) öbekidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="93b3f-108">Bir bileşen, veri eklemek veya UI olaylarına yanıt vermek için gereken HTML işaretlemesini ve işleme mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="93b3f-109">Bileşenler esnek ve hafif.</span><span class="sxs-lookup"><span data-stu-id="93b3f-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="93b3f-110">Bunlar, iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="93b3f-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="93b3f-111">Component classes</span></span>

<span data-ttu-id="93b3f-112">Bileşenler, C# ve HTML Işaretlemesi kullanılarak [Razor](xref:mvc/views/razor) bileşen dosyalarında ( *. Razor*) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="93b3f-113">Blazor içindeki bir bileşen, bir *Razor bileşeni*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="93b3f-114">Bir bileşenin adı, büyük harfle başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="93b3f-115">Örneğin, *mycoolcomponent. Razor* geçerlidir ve *mycoolcomponent. Razor* geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="93b3f-116">Bir bileşen için Kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="93b3f-117">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="93b3f-118">Bir uygulama derlendiğinde, HTML biçimlendirme ve C# işleme mantığı bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="93b3f-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="93b3f-119">Oluşturulan sınıfın adı, dosyanın adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="93b3f-120">Bileşen sınıfının üyeleri bir `@code` bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="93b3f-121">`@code` bloğunda, bileşen durumu (özellikler, alanlar) olay işleme yöntemleriyle veya diğer bileşen mantığını tanımlamaya yönelik yöntemlerle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="93b3f-122">Birden fazla `@code` bloğu izin verilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="93b3f-123">Bileşen üyeleri, `@`ile başlayan ifadeleri kullanarak C# bileşenin işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="93b3f-124">Örneğin bir C# alan, alan adına `@` önek olarak işlenerek işlenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="93b3f-125">Aşağıdaki örnek değerlendirilir ve işler:</span><span class="sxs-lookup"><span data-stu-id="93b3f-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="93b3f-126">`font-style`için CSS özellik değerine `_headingFontStyle`.</span><span class="sxs-lookup"><span data-stu-id="93b3f-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="93b3f-127">`<h1>` öğesinin içeriğine `_headingText`.</span><span class="sxs-lookup"><span data-stu-id="93b3f-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="93b3f-128">Bileşen ilk olarak işlendikten sonra, bileşen işleme ağacını olaylara yanıt olarak yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="93b3f-129">Blazor ardından yeni işleme ağacını önceki bir ile karşılaştırır ve tarayıcının Belge Nesne Modeli (DOM) üzerinde herhangi bir değişiklik uygular.</span><span class="sxs-lookup"><span data-stu-id="93b3f-129">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="93b3f-130">Bileşenler sıradan C# sınıflardır ve bir proje içinde herhangi bir yere yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="93b3f-131">Web sayfalarını üreten bileşenler genellikle *Sayfalar* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="93b3f-132">Sayfa olmayan bileşenler sıklıkla *paylaşılan* klasöre veya projeye eklenen özel bir klasöre yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="93b3f-133">Genellikle, bir bileşenin ad alanı uygulamanın kök ad alanından ve uygulamanın içindeki konum (klasör) ile türetilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="93b3f-134">Uygulamanın kök ad alanı `BlazorApp` ve `Counter` bileşeni *Sayfalar* klasöründe yer alıyorsa:</span><span class="sxs-lookup"><span data-stu-id="93b3f-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="93b3f-135">`Counter` bileşenin ad alanı `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="93b3f-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="93b3f-136">Bileşenin tam nitelikli tür adı `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="93b3f-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="93b3f-137">Daha fazla bilgi için [bileşenleri Içeri aktarma](#import-components) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="93b3f-138">Özel bir klasör kullanmak için, özel klasörün ad alanını üst bileşene ya da uygulamanın *_Imports. Razor* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="93b3f-139">Örneğin, aşağıdaki ad alanı, uygulamanın kök ad alanı `BlazorApp`olduğunda *Bileşenler* klasöründeki bileşenleri kullanılabilir yapar:</span><span class="sxs-lookup"><span data-stu-id="93b3f-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="93b3f-140">Bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme</span><span class="sxs-lookup"><span data-stu-id="93b3f-140">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="93b3f-141">Razor bileşenleri, Razor Pages ve MVC uygulamalarıyla tümleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-141">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="93b3f-142">Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden alınabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-142">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="93b3f-143">Razor bileşenlerini barındırmak üzere bir Razor Pages veya MVC uygulaması hazırlamak için, <xref:blazor/hosting-models#integrate-razor-components-into-razor-pages-and-mvc-apps> makalesindeki *Razor bileşenlerini tümleştirme Razor Pages ve MVC Apps* bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-143">To prepare a Razor Pages or MVC app to host Razor components, follow the guidance in the *Integrate Razor components into Razor Pages and MVC apps* section of the <xref:blazor/hosting-models#integrate-razor-components-into-razor-pages-and-mvc-apps> article.</span></span>

<span data-ttu-id="93b3f-144">Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü/görünümü veya *_ViewImports. cshtml* dosyasını temsil eden ad alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-144">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="93b3f-145">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="93b3f-145">In the following example:</span></span>

* <span data-ttu-id="93b3f-146">`MyAppNamespace` uygulamanın ad alanıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-146">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="93b3f-147">*Bileşenler adlı bir* klasör bileşenleri tutmak için kullanılmazsa `Components`, bileşenlerin bulunduğu klasöre geçin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-147">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```csharp
@using MyAppNamespace.Components
```

<span data-ttu-id="93b3f-148">*_ViewImports. cshtml* dosyası, bir Razor Pages uygulamasının *Sayfalar* klasöründe veya bir MVC uygulamasının *views* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-148">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="93b3f-149">Bir sayfadan veya görünümden bir bileşeni işlemek için `Component` etiketi yardımcısını kullanın:</span><span class="sxs-lookup"><span data-stu-id="93b3f-149">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="93b3f-150">Parametre türünün JSON serileştirilebilir olması gerekir, bu, genellikle türün bir varsayılan oluşturucuya ve ayarlanabilir özelliklere sahip olması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-150">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="93b3f-151">Örneğin, `IncrementAmount` için bir değer belirtebilirsiniz çünkü `IncrementAmount` türü, JSON seri hale getirici tarafından desteklenen bir temel tür `int`.</span><span class="sxs-lookup"><span data-stu-id="93b3f-151">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="93b3f-152">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-152">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="93b3f-153">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-153">Is prerendered into the page.</span></span>
* <span data-ttu-id="93b3f-154">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="93b3f-154">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="93b3f-155">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93b3f-155">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="93b3f-156">Bileşeni statik HTML olarak işler ve bir Blazor Server uygulaması için işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-156">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="93b3f-157">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-157">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="93b3f-158">Bir Blazor sunucu uygulaması için işaretleyici işler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-158">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="93b3f-159">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-159">Output from the component isn't included.</span></span> <span data-ttu-id="93b3f-160">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-160">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="93b3f-161">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-161">Renders the component into static HTML.</span></span> |

<span data-ttu-id="93b3f-162">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-162">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="93b3f-163">Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-163">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="93b3f-164">Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-164">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="93b3f-165">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="93b3f-165">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="93b3f-166">Bileşenlerin nasıl işlendiği, bileşen durumu ve `Component` etiketi Yardımcısı hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="93b3f-166">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

## <a name="tag-helpers-arent-used-in-components"></a><span data-ttu-id="93b3f-167">Etiket Yardımcıları bileşenlerde kullanılmıyor</span><span class="sxs-lookup"><span data-stu-id="93b3f-167">Tag Helpers aren't used in components</span></span>

<span data-ttu-id="93b3f-168">Razor bileşenlerinde ( *. Razor* dosyaları) [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-168">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="93b3f-169">Blazor ' de etiket Yardımcısı benzeri işlevsellik sağlamak için, etiket Yardımcısı ile aynı işlevselliğe sahip bir bileşen oluşturun ve bunun yerine bileşeni kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-169">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="93b3f-170">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="93b3f-170">Use components</span></span>

<span data-ttu-id="93b3f-171">Bileşenler, HTML öğesi söz dizimini kullanarak bildirerek diğer bileşenleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-171">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="93b3f-172">Bir bileşeni kullanmak için biçimlendirme, etiket adının bileşen türü olduğu bir HTML etiketi gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="93b3f-172">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="93b3f-173">Öznitelik bağlama büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-173">Attribute binding is case sensitive.</span></span> <span data-ttu-id="93b3f-174">Örneğin, `@bind` geçerlidir ve `@Bind` geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-174">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="93b3f-175">*Index. Razor* dosyasında aşağıdaki biçimlendirme `HeadingComponent` örneği işler:</span><span class="sxs-lookup"><span data-stu-id="93b3f-175">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="93b3f-176">*Bileşenler/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-176">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="93b3f-177">Bir bileşen bir bileşen adıyla eşleşmeyen büyük harfle yazılmış bir HTML öğesi içeriyorsa, öğenin beklenmeyen bir adı olduğunu gösteren bir uyarı yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-177">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="93b3f-178">Bileşenin ad alanı için `@using` bir deyimin eklenmesi, bileşenin kullanılabilir olmasını sağlar ve bu da uyarıyı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-178">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="93b3f-179">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-179">Component parameters</span></span>

<span data-ttu-id="93b3f-180">Bileşenler, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanan *bileşen parametrelerine*sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-180">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="93b3f-181">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-181">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="93b3f-182">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="93b3f-183">Örnek uygulamadan aşağıdaki örnekte `ParentComponent`, `ChildComponent``Title` özelliğinin değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-183">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="93b3f-184">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-184">*Pages/ParentComponent.razor*:</span></span>

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

## <a name="child-content"></a><span data-ttu-id="93b3f-185">Alt içerik</span><span class="sxs-lookup"><span data-stu-id="93b3f-185">Child content</span></span>

<span data-ttu-id="93b3f-186">Bileşenler, başka bir bileşenin içeriğini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-186">Components can set the content of another component.</span></span> <span data-ttu-id="93b3f-187">Atama bileşeni, alıcı bileşeni belirten Etiketler arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-187">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="93b3f-188">Aşağıdaki örnekte `ChildComponent`, işlemek için bir kullanıcı arabirimi segmentini temsil eden bir `RenderFragment`temsil eden bir `ChildContent` özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-188">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="93b3f-189">`ChildContent` değeri bileşenin, içeriğin işlenmesi gereken biçimlendirmesinde konumlandırılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-189">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="93b3f-190">`ChildContent` değeri üst bileşenden alınır ve önyükleme bölmesinin `panel-body`içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-190">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="93b3f-191">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-191">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="93b3f-192">`RenderFragment` içeriği alan özelliğin kurala göre `ChildContent` olarak adlandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-192">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="93b3f-193">Örnek uygulamadaki `ParentComponent`, içeriği `<ChildComponent>` etiketlerin içine yerleştirerek `ChildComponent` işlemek için içerik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-193">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="93b3f-194">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-194">*Pages/ParentComponent.razor*:</span></span>

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

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="93b3f-195">Öznitelik döndürme ve rastgele parametreler</span><span class="sxs-lookup"><span data-stu-id="93b3f-195">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="93b3f-196">Bileşenler, bileşen tarafından tanımlanan parametrelere ek olarak ek öznitelikler yakalayabilir ve işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-196">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="93b3f-197">Ek öznitelikler bir sözlükte yakalanabilir ve sonra bileşen [`@attributes`](xref:mvc/views/razor#attributes) Razor yönergesi kullanılarak işlendiğinde bir *öğe üzerine bırakılabilir* .</span><span class="sxs-lookup"><span data-stu-id="93b3f-197">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="93b3f-198">Bu senaryo, çeşitli özelleştirmeleri destekleyen bir işaretleme öğesi üreten bir bileşen tanımlarken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-198">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="93b3f-199">Örneğin, çok sayıda parametreyi destekleyen bir `<input>` öznitelikleri ayrı olarak tanımlamak sıkıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-199">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="93b3f-200">Aşağıdaki örnekte, ilk `<input>` öğesi (`id="useIndividualParams"`) bağımsız bileşen parametrelerini kullanır, ancak ikinci `<input>` öğesi (`id="useAttributesDict"`) öznitelik sponu kullanır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-200">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="93b3f-201">Parametrenin türü dize anahtarlarıyla `IEnumerable<KeyValuePair<string, object>>` uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-201">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="93b3f-202">`IReadOnlyDictionary<string, object>` kullanmak Bu senaryoda da bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-202">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="93b3f-203">Her iki yaklaşımın de kullanıldığı işlenen `<input>` öğeleri aynıdır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-203">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="93b3f-204">Rastgele öznitelikleri kabul etmek için, `CaptureUnmatchedValues` özelliği `true`olarak ayarlanan `[Parameter]` özniteliğini kullanarak bir bileşen parametresi tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="93b3f-204">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="93b3f-205">`[Parameter]` `CaptureUnmatchedValues` özelliği, parametrenin diğer bir parametreyle eşleşmeyen tüm özniteliklerle eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-205">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="93b3f-206">Bir bileşen yalnızca `CaptureUnmatchedValues`olan tek bir parametre tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-206">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="93b3f-207">`CaptureUnmatchedValues` ile kullanılan özellik türü, `Dictionary<string, object>` dize anahtarlarıyla atanabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-207">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="93b3f-208">`IEnumerable<KeyValuePair<string, object>>` veya `IReadOnlyDictionary<string, object>` Ayrıca bu senaryodaki seçeneklerdir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-208">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="93b3f-209">Öğe özniteliklerinin konumuna göre `@attributes` konumu önemlidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-209">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="93b3f-210">Öğe üzerinde `@attributes`, öznitelikler sağdan sola (son olarak) işlenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-210">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="93b3f-211">`Child` bileşeni tüketen bir bileşen için aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="93b3f-211">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="93b3f-212">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-212">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="93b3f-213">*Childcomponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-213">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="93b3f-214">`Child` bileşenin `extra` özniteliği `@attributes`sağına ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-214">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="93b3f-215">`Parent` bileşenin işlenmiş `<div>`, öznitelikler sağdan sola (en son) işlendiği için ek özniteliğiyle geçirildiğinde `extra="5"` içerir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-215">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="93b3f-216">Aşağıdaki örnekte, `extra` ve `@attributes` sırası `Child` bileşeninin `<div>`tersine çevrilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-216">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="93b3f-217">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-217">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="93b3f-218">*Childcomponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-218">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="93b3f-219">`Parent` bileşenindeki işlenen `<div>`, ek öznitelik üzerinden geçirildiğinde `extra="10"` içerir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-219">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="93b3f-220">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="93b3f-220">Data binding</span></span>

<span data-ttu-id="93b3f-221">Hem bileşenlere hem de DOM öğelerine veri bağlama [`@bind`](xref:mvc/views/razor#bind) özniteliğiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-221">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="93b3f-222">Aşağıdaki örnek, bir `CurrentValue` özelliğini metin kutusu değerine bağlar:</span><span class="sxs-lookup"><span data-stu-id="93b3f-222">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="93b3f-223">Metin kutusu odağı kaybettiğinde, özelliğin değeri güncellenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-223">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="93b3f-224">Metin kutusu kullanıcı arabiriminde, özelliğin değerini değiştirme yanıt olarak değil, yalnızca bileşen işlendiğinde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-224">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="93b3f-225">Bileşenler olay işleyicisi kodu yürütüldükten sonra kendilerini oluşturduğundan, özellik güncelleştirmeleri *genellikle* olay işleyicisi tetiklendikten hemen sonra Kullanıcı arabirimine yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-225">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="93b3f-226">`CurrentValue` özelliği (`<input @bind="CurrentValue" />`) ile `@bind` kullanmak, temelde aşağıdakilere eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-226">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="93b3f-227">Bileşen işlendiğinde, giriş öğesinin `value` `CurrentValue` özelliğinden gelir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-227">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="93b3f-228">Kullanıcı metin kutusuna yazdığında ve öğe odağını değiştirdiğinde, `onchange` olayı tetiklenir ve `CurrentValue` özelliği değiştirilen değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-228">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="93b3f-229">`@bind`, tür dönüştürmelerin gerçekleştirildiği durumları işlediği için kod oluşturma daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-229">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="93b3f-230">Prensibi `@bind`, bir ifadenin geçerli değerini bir `value` özniteliğiyle ilişkilendirir ve kayıtlı işleyiciyi kullanarak değişiklikleri işler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-230">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="93b3f-231">`@bind` sözdizimiyle `onchange` olaylarının işlenmesine ek olarak, bir özellik veya alan, `event` parametreli bir [`@bind-value`](xref:mvc/views/razor#bind) özniteliği belirterek diğer olaylar kullanılarak da bağlanabilir ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="93b3f-231">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="93b3f-232">Aşağıdaki örnek, `oninput` olayı için `CurrentValue` özelliğini bağlar:</span><span class="sxs-lookup"><span data-stu-id="93b3f-232">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="93b3f-233">`onchange`aksine, öğe odağı kaybettiğinde harekete geçirilir `oninput` metin kutusunun değeri değiştiğinde harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-233">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="93b3f-234">önceki örnekteki `@bind-value` bağlar:</span><span class="sxs-lookup"><span data-stu-id="93b3f-234">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="93b3f-235">Öğenin `value` özniteliğinde belirtilen ifade (`CurrentValue`).</span><span class="sxs-lookup"><span data-stu-id="93b3f-235">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="93b3f-236">`@bind-value:event`tarafından belirtilen olaya yönelik değişiklik olayı temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="93b3f-236">A change event delegate to the event specified by `@bind-value:event`.</span></span>

<span data-ttu-id="93b3f-237">**Ayrıştırılamayan değerler**</span><span class="sxs-lookup"><span data-stu-id="93b3f-237">**Unparsable values**</span></span>

<span data-ttu-id="93b3f-238">Bir Kullanıcı, bir veri sınırlama öğesine ayrıştırılamayan bir değer sağlıyorsa, bağlama olayı tetiklendiğinde, çözümlenemeyen değer otomatik olarak önceki değerine döndürülür.</span><span class="sxs-lookup"><span data-stu-id="93b3f-238">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="93b3f-239">Aşağıdaki senaryoyu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="93b3f-239">Consider the following scenario:</span></span>

* <span data-ttu-id="93b3f-240">Bir `<input>` öğesi, `123`başlangıçtaki değeri olan bir `int` türüne bağlanır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-240">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="93b3f-241">Kullanıcı, öğe değerini sayfada `123.45` olarak güncelleştirir ve öğe odağını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-241">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="93b3f-242">Önceki senaryoda, öğenin değeri `123`olarak geri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="93b3f-242">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="93b3f-243">Değer `123.45` özgün `123`değerinin yararına reddedildiğinde, Kullanıcı değerinin kabul edilmediğini anlamıştır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-243">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="93b3f-244">Varsayılan olarak, bağlama öğenin `onchange` olayına (`@bind="{PROPERTY OR FIELD}"`) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-244">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="93b3f-245">Farklı bir olay ayarlamak için `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-245">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="93b3f-246">`oninput` olayı (`@bind-value:event="oninput"`) için yeniden sürüm, ayrıştırılamayan bir değer sunan herhangi bir tuş vuruşu sonrasında oluşur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-246">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="93b3f-247">`oninput` olayı `int`bağlantılı bir türle hedeflenirken, kullanıcının bir `.` karakteri yazmasının engellenmiş olması engellenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-247">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="93b3f-248">`.` bir karakter hemen kaldırılır, bu nedenle Kullanıcı yalnızca tam sayılara izin verilen anında geri bildirim alır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-248">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="93b3f-249">`oninput` olaylarındaki değerin geri döndürülmesi ideal olmayan, örneğin kullanıcının ayrıştırılamayan `<input>` bir değeri temizlemeye izin verilmesi gereken senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-249">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="93b3f-250">Alternatifler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-250">Alternatives include:</span></span>

* <span data-ttu-id="93b3f-251">`oninput` olayını kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-251">Don't use the `oninput` event.</span></span> <span data-ttu-id="93b3f-252">Öğe odağı kaybederene kadar geçersiz bir değer geri döndürülmediğinde, varsayılan `onchange` olayını (`@bind="{PROPERTY OR FIELD}"`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-252">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="93b3f-253">`int?` veya `string`gibi null yapılabilir bir türe bağlayın ve geçersiz girdileri işlemek için özel mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-253">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="93b3f-254">`InputNumber` veya `InputDate`gibi bir [form doğrulama bileşeni](xref:blazor/forms-validation)kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-254">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="93b3f-255">Form doğrulama bileşenlerinde geçersiz girişleri yönetmek için yerleşik destek vardır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-255">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="93b3f-256">Form doğrulama bileşenleri:</span><span class="sxs-lookup"><span data-stu-id="93b3f-256">Form validation components:</span></span>
  * <span data-ttu-id="93b3f-257">Kullanıcının geçersiz giriş sağlamasına ve ilişkili `EditContext`doğrulama hataları almasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-257">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="93b3f-258">Kullanıcı ek WebForm verisi girmeye uğramadan doğrulama hatalarını Kullanıcı ARABIRIMINDE görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-258">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="93b3f-259">**Genelleştirme**</span><span class="sxs-lookup"><span data-stu-id="93b3f-259">**Globalization**</span></span>

<span data-ttu-id="93b3f-260">`@bind` değerleri, geçerli kültürün kuralları kullanılarak görüntülenmek üzere biçimlendirilir ve ayrıştırılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-260">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="93b3f-261">Geçerli kültüre <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> özelliğinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-261">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="93b3f-262">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) aşağıdaki alan türleri için kullanılır (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="93b3f-262">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="93b3f-263">Yukarıdaki alan türleri:</span><span class="sxs-lookup"><span data-stu-id="93b3f-263">The preceding field types:</span></span>

* <span data-ttu-id="93b3f-264">, Uygun tarayıcı tabanlı biçimlendirme kuralları kullanılarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-264">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="93b3f-265">Serbest biçimli metin içeremez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-265">Can't contain free-form text.</span></span>
* <span data-ttu-id="93b3f-266">Tarayıcının uygulamasına göre Kullanıcı etkileşimi özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-266">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="93b3f-267">Aşağıdaki alan türleri özel biçimlendirme gereksinimlerine sahiptir ve şu anda tüm büyük tarayıcılarda desteklenmediği için Blazor tarafından desteklenmemektedir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-267">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="93b3f-268">`@bind`, bir değeri ayrıştırmak ve biçimlendirmek için bir <xref:System.Globalization.CultureInfo?displayProperty=fullName> sağlamak üzere `@bind:culture` parametresini destekler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-268">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="93b3f-269">`date` ve `number` alan türleri kullanılırken bir kültür belirtilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-269">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="93b3f-270">`date` ve `number`, gerekli kültürü sağlayan yerleşik Blazor desteğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-270">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="93b3f-271">Kullanıcının kültürünü ayarlama hakkında daha fazla bilgi için [Yerelleştirme](#localization) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-271">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="93b3f-272">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="93b3f-272">**Format strings**</span></span>

<span data-ttu-id="93b3f-273">Veri bağlama [`@bind:format`](xref:mvc/views/razor#bind)kullanarak <xref:System.DateTime> biçim dizeleriyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-273">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="93b3f-274">Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-274">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="93b3f-275">Yukarıdaki kodda `<input>` öğenin alan türü (`type`), `text`varsayılan olarak olur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-275">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="93b3f-276">`@bind:format` aşağıdaki .NET türlerini bağlamak için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-276">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="93b3f-277"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="93b3f-277"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="93b3f-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="93b3f-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="93b3f-279">`@bind:format` özniteliği, `<input>` öğesinin `value` uygulanacak tarih biçimini belirtir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-279">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="93b3f-280">Biçim Ayrıca, `onchange` bir olay gerçekleştiğinde değeri ayrıştırmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-280">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="93b3f-281">Blazor 'in tarihleri biçimlendirmek için yerleşik desteği olduğundan `date` alanı türü için bir biçim belirtilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-281">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="93b3f-282">Önerinin artma, `yyyy-MM-dd` tarih biçimini yalnızca `date` alan türüyle bir biçim sağlanırsa doğru şekilde çalışacak şekilde kullanın:</span><span class="sxs-lookup"><span data-stu-id="93b3f-282">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

<span data-ttu-id="93b3f-283">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="93b3f-283">**Component parameters**</span></span>

<span data-ttu-id="93b3f-284">Bağlama bileşen parametrelerini tanır; burada `@bind-{property}`, bileşenler arasında bir özellik değeri bağlayabilirler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-284">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="93b3f-285">Aşağıdaki alt bileşen (`ChildComponent`) `Year` bir bileşen parametresine ve geri çağırmaya `YearChanged` sahiptir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-285">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="93b3f-286">`EventCallback<T>` [Eventcallback](#eventcallback) bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-286">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="93b3f-287">Aşağıdaki üst bileşen `ChildComponent` kullanır ve üst öğeden `ParentYear` parametresini alt bileşendeki `Year` parametresine bağlar:</span><span class="sxs-lookup"><span data-stu-id="93b3f-287">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="93b3f-288">`ParentComponent` yüklemek aşağıdaki biçimlendirmeyi üretir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-288">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="93b3f-289">`ParentYear` özelliğinin değeri `ParentComponent`düğme seçilerek değiştirilirse, `ChildComponent` `Year` özelliği güncellenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-289">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="93b3f-290">`Year` yeni değeri, `ParentComponent` yeniden eklendiğinde Kullanıcı arabiriminde işlenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-290">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="93b3f-291">`Year` parametresi, `Year` parametresinin türüyle eşleşen bir yardımcı `YearChanged` olayına sahip olduğundan bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-291">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="93b3f-292">Kurala göre `<ChildComponent @bind-Year="ParentYear" />` temelde yazmaya eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-292">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="93b3f-293">Genel olarak, bir özellik `@bind-property:event` özniteliği kullanılarak karşılık gelen bir olay işleyicisine bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-293">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="93b3f-294">Örneğin, özellik `MyProp` aşağıdaki iki öznitelik kullanılarak `MyEventHandler` bağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-294">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

<span data-ttu-id="93b3f-295">**Radyo düğmeleri**</span><span class="sxs-lookup"><span data-stu-id="93b3f-295">**Radio buttons**</span></span>

<span data-ttu-id="93b3f-296">Bir form içindeki radyo düğmelerine bağlama hakkında bilgi için bkz. <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="93b3f-296">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>

## <a name="event-handling"></a><span data-ttu-id="93b3f-297">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="93b3f-297">Event handling</span></span>

<span data-ttu-id="93b3f-298">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-298">Razor components provide event handling features.</span></span> <span data-ttu-id="93b3f-299">`on{EVENT}` adlı bir HTML öğesi özniteliği için (örneğin, `onclick` ve `onsubmit`), temsilci türü belirtilmiş bir değer ile, Razor bileşenleri özniteliğin değerini bir olay işleyicisi olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-299">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="93b3f-300">Özniteliğin adı her zaman [`@on{EVENT}`](xref:mvc/views/razor#onevent)biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-300">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="93b3f-301">Aşağıdaki kod, Kullanıcı arabiriminde düğme seçildiğinde `UpdateHeading` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-301">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="93b3f-302">Aşağıdaki kod, Kullanıcı arabiriminde onay kutusu değiştirildiğinde `CheckChanged` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-302">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="93b3f-303">Olay işleyicileri Ayrıca zaman uyumsuz olabilir ve bir <xref:System.Threading.Tasks.Task>döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-303">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="93b3f-304">[Statehaschanged](xref:blazor/lifecycle#state-changes)el ile çağırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-304">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="93b3f-305">Özel durumlar oluştuğunda günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-305">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="93b3f-306">Aşağıdaki örnekte, düğme seçildiğinde `UpdateHeading` zaman uyumsuz olarak çağrılır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-306">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="93b3f-307">Olay bağımsız değişken türleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-307">Event argument types</span></span>

<span data-ttu-id="93b3f-308">Bazı olaylar için olay bağımsız değişkeni türlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-308">For some events, event argument types are permitted.</span></span> <span data-ttu-id="93b3f-309">Bu olay türlerinden birine erişim gerekmiyorsa, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-309">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="93b3f-310">Desteklenen `EventArgs` aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-310">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="93b3f-311">Olay</span><span class="sxs-lookup"><span data-stu-id="93b3f-311">Event</span></span>            | <span data-ttu-id="93b3f-312">Sınıf</span><span class="sxs-lookup"><span data-stu-id="93b3f-312">Class</span></span>                | <span data-ttu-id="93b3f-313">DOM olayları ve notları</span><span class="sxs-lookup"><span data-stu-id="93b3f-313">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="93b3f-314">Pano</span><span class="sxs-lookup"><span data-stu-id="93b3f-314">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="93b3f-315">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="93b3f-315">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="93b3f-316">Sürükle</span><span class="sxs-lookup"><span data-stu-id="93b3f-316">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="93b3f-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="93b3f-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="93b3f-318">`DataTransfer` ve `DataTransferItem` öğe verilerini sürüklemiş tutun.</span><span class="sxs-lookup"><span data-stu-id="93b3f-318">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="93b3f-319">Hata</span><span class="sxs-lookup"><span data-stu-id="93b3f-319">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="93b3f-320">Olay</span><span class="sxs-lookup"><span data-stu-id="93b3f-320">Event</span></span>            | `EventArgs`          | <span data-ttu-id="93b3f-321">*Genel*</span><span class="sxs-lookup"><span data-stu-id="93b3f-321">*General*</span></span><br><span data-ttu-id="93b3f-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="93b3f-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="93b3f-323">*Pano*</span><span class="sxs-lookup"><span data-stu-id="93b3f-323">*Clipboard*</span></span><br><span data-ttu-id="93b3f-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="93b3f-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="93b3f-325">*Giriş*</span><span class="sxs-lookup"><span data-stu-id="93b3f-325">*Input*</span></span><br><span data-ttu-id="93b3f-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="93b3f-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="93b3f-327">*Medyasını*</span><span class="sxs-lookup"><span data-stu-id="93b3f-327">*Media*</span></span><br><span data-ttu-id="93b3f-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="93b3f-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="93b3f-329">Çı</span><span class="sxs-lookup"><span data-stu-id="93b3f-329">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="93b3f-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="93b3f-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="93b3f-331">`relatedTarget`için destek içermez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-331">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="93b3f-332">Giriş</span><span class="sxs-lookup"><span data-stu-id="93b3f-332">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="93b3f-333">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="93b3f-333">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="93b3f-334">Klavye</span><span class="sxs-lookup"><span data-stu-id="93b3f-334">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="93b3f-335">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="93b3f-335">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="93b3f-336">Tığında</span><span class="sxs-lookup"><span data-stu-id="93b3f-336">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="93b3f-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="93b3f-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="93b3f-338">Fare işaretçisi</span><span class="sxs-lookup"><span data-stu-id="93b3f-338">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="93b3f-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="93b3f-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="93b3f-340">Fare tekerleği</span><span class="sxs-lookup"><span data-stu-id="93b3f-340">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="93b3f-341">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="93b3f-341">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="93b3f-342">İlerleme durumu</span><span class="sxs-lookup"><span data-stu-id="93b3f-342">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="93b3f-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="93b3f-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="93b3f-344">Dokunmatik</span><span class="sxs-lookup"><span data-stu-id="93b3f-344">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="93b3f-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="93b3f-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="93b3f-346">`TouchPoint`, dokunmaya duyarlı bir cihazdaki tek bir iletişim noktasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="93b3f-346">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="93b3f-347">Önceki tablodaki olayların özellikleri ve olay işleme davranışı hakkında bilgi için bkz. [başvuru kaynağında EventArgs sınıfları (DotNet/aspnetcore Release/3.1 dalı)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="93b3f-347">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="93b3f-348">Lambda ifadeleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-348">Lambda expressions</span></span>

<span data-ttu-id="93b3f-349">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-349">Lambda expressions can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="93b3f-350">Genellikle, bir dizi öğe üzerinde yineleme yaparken olduğu gibi ek değerlerin üzerinde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-350">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="93b3f-351">Aşağıdaki örnek, her biri Kullanıcı arabiriminde seçildiğinde bir olay bağımsız değişkeni (`MouseEventArgs`) ve düğme numarası (`buttonNumber`) `UpdateHeading` çağıran üç düğme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="93b3f-351">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="93b3f-352">Döngü değişkenini (`i`) doğrudan bir lambda ifadesinde bir `for` **döngüsünde kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="93b3f-352">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="93b3f-353">Aksi halde aynı değişken tüm lambda ifadeleri tarafından, `i`değerinin tüm Lambdalar ile aynı olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-353">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="93b3f-354">Her zaman değerini yerel bir değişkende (önceki örnekte`buttonNumber`) yakalayın ve sonra kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-354">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="93b3f-355">EventCallback</span><span class="sxs-lookup"><span data-stu-id="93b3f-355">EventCallback</span></span>

<span data-ttu-id="93b3f-356">İç içe bileşenler içeren yaygın bir senaryo, bir alt bileşen olayı gerçekleştiğinde bir üst bileşenin yöntemini çalıştırma, örneğin, alt öğe içinde bir `onclick` olayı oluştuğunda&mdash;.</span><span class="sxs-lookup"><span data-stu-id="93b3f-356">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="93b3f-357">Olayları bileşenler arasında göstermek için bir `EventCallback`kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-357">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="93b3f-358">Bir üst bileşen, bir alt bileşenin `EventCallback`bir geri çağırma yöntemi atayabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-358">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="93b3f-359">Örnek uygulamadaki `ChildComponent` (*Bileşenler/ChildComponent. Razor*), bir düğmenin `onclick` işleyicisinin, örneğin `ParentComponent`bir `EventCallback` temsilcisini almak üzere nasıl ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-359">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="93b3f-360">`EventCallback`, bir çevresel cihazdan `onclick` olayına uygun `MouseEventArgs`ile yazılır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-360">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="93b3f-361">`ParentComponent`, alt öğenin `EventCallback<T>` (`OnClick`) `ShowMessage` yöntemine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-361">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClick`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="93b3f-362">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-362">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="93b3f-363">`ChildComponent`düğme seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="93b3f-363">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="93b3f-364">`ParentComponent``ShowMessage` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-364">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="93b3f-365">`_messageText` güncellenir ve `ParentComponent`görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-365">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="93b3f-366">Geri çağırma yönteminde (`ShowMessage`) [Statehaschanged](xref:blazor/lifecycle#state-changes) çağrısı gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-366">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="93b3f-367">`StateHasChanged`, alt olaylar, alt öğe içinde yürütülen olay işleyicilerinde bileşen rerendering tetiklenmesi gibi `ParentComponent`yeniden çalıştırmak için otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-367">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="93b3f-368">`EventCallback` ve `EventCallback<T>` zaman uyumsuz temsilcilere izin verir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-368">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="93b3f-369">`EventCallback<T>` kesin bir şekilde yazılır ve belirli bir bağımsız değişken türü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-369">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="93b3f-370">`EventCallback` zayıf ve bağımsız değişken türüne izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="93b3f-370">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="93b3f-371">`InvokeAsync` ile bir `EventCallback` veya `EventCallback<T>` çağırın ve <xref:System.Threading.Tasks.Task>await:</span><span class="sxs-lookup"><span data-stu-id="93b3f-371">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="93b3f-372">Olay işleme ve bağlama bileşeni parametrelerini `EventCallback` ve `EventCallback<T>` kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-372">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="93b3f-373">`EventCallback`üzerinde türü kesin belirlenmiş `EventCallback<T>` tercih edin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-373">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="93b3f-374">`EventCallback<T>`, bileşenin kullanıcılarına daha iyi hata geri bildirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-374">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="93b3f-375">Diğer UI olay işleyicileriyle benzer şekilde, olay parametresini belirtmek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-375">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="93b3f-376">Geri çağırmaya hiçbir değer geçirilmemişse `EventCallback` kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-376">Use `EventCallback` when there's no value passed to the callback.</span></span>

### <a name="prevent-default-actions"></a><span data-ttu-id="93b3f-377">Varsayılan eylemleri engelle</span><span class="sxs-lookup"><span data-stu-id="93b3f-377">Prevent default actions</span></span>

<span data-ttu-id="93b3f-378">Bir olayın varsayılan eylemini engellemek için [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-378">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="93b3f-379">Giriş cihazında bir anahtar seçildiğinde ve öğe odağı bir metin kutusunda olduğunda, bir tarayıcı normalde metin kutusunda anahtarın karakterini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-379">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="93b3f-380">Aşağıdaki örnekte, `@onkeypress:preventDefault` Directive özniteliği belirtilerek varsayılan davranış engellenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-380">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="93b3f-381">Sayaç artar ve **+** anahtarı `<input>` öğenin değerine yakalanmaz:</span><span class="sxs-lookup"><span data-stu-id="93b3f-381">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

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

<span data-ttu-id="93b3f-382">`@on{EVENT}:preventDefault` özniteliğini bir değer olmadan belirtmek `@on{EVENT}:preventDefault="true"`eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-382">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="93b3f-383">Özniteliğin değeri de bir ifade olabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-383">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="93b3f-384">Aşağıdaki örnekte `_shouldPreventDefault`, `true` veya `false`olarak ayarlanan bir `bool` alandır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-384">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="93b3f-385">Varsayılan eylemi engellemek için bir olay işleyicisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-385">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="93b3f-386">Olay işleyicisi ve varsayılan eylem senaryolarına bağımsız olarak bir şekilde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-386">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="93b3f-387">Olay yaymayı durdur</span><span class="sxs-lookup"><span data-stu-id="93b3f-387">Stop event propagation</span></span>

<span data-ttu-id="93b3f-388">Olay yaymayı durdurmak için [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-388">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="93b3f-389">Aşağıdaki örnekte, onay kutusunun seçilmesi ikinci alt `<div>`, üst `<div>`yaymadan sonraki olayları engeller:</span><span class="sxs-lookup"><span data-stu-id="93b3f-389">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

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

## <a name="chained-bind"></a><span data-ttu-id="93b3f-390">Zincirleme bağlama</span><span class="sxs-lookup"><span data-stu-id="93b3f-390">Chained bind</span></span>

<span data-ttu-id="93b3f-391">Yaygın bir senaryo, bir veri bağlama parametresini bileşen çıkışında bir sayfa öğesine zincirlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-391">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="93b3f-392">Birden çok bağlama düzeyi aynı anda gerçekleştiğinden, bu senaryoya *zincirleme bağlama* denir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-392">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="93b3f-393">Bir zincir bağlama, sayfanın öğesinde `@bind` sözdizimiyle uygulanamıyor.</span><span class="sxs-lookup"><span data-stu-id="93b3f-393">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="93b3f-394">Olay işleyicisi ve değeri ayrı olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-394">The event handler and value must be specified separately.</span></span> <span data-ttu-id="93b3f-395">Bununla birlikte, bir üst bileşen, bileşen parametresiyle `@bind` sözdizimini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-395">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="93b3f-396">Aşağıdaki `PasswordField` bileşeni (*Passwordfield. Razor*):</span><span class="sxs-lookup"><span data-stu-id="93b3f-396">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="93b3f-397">Bir `<input>` öğesinin değerini bir `Password` özelliğine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-397">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="93b3f-398">`Password` özelliğindeki değişiklikleri, bir [Eventcallback](#eventcallback)ile bir üst bileşene gösterir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-398">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

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
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="93b3f-399">`PasswordField` bileşeni başka bir bileşende kullanılır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-399">The `PasswordField` component is used in another component:</span></span>

```razor
<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="93b3f-400">Önceki örnekteki parolada denetim veya tuzak hataları gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="93b3f-400">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="93b3f-401">`Password` için bir yedekleme alanı oluşturun (Aşağıdaki örnek kodda`_password`).</span><span class="sxs-lookup"><span data-stu-id="93b3f-401">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="93b3f-402">`Password` ayarlayıcısı 'nda denetimleri veya yakalama hatalarını gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-402">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="93b3f-403">Aşağıdaki örnek, parolanın değerinde bir boşluk kullanılmışsa kullanıcıya anında geri bildirim sağlar:</span><span class="sxs-lookup"><span data-stu-id="93b3f-403">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
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
        _showPassword = !_showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="93b3f-404">Bileşenlere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="93b3f-404">Capture references to components</span></span>

<span data-ttu-id="93b3f-405">Bileşen başvuruları, bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe `Show` veya `Reset`gibi komutlar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-405">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="93b3f-406">Bir bileşen başvurusunu yakalamak için:</span><span class="sxs-lookup"><span data-stu-id="93b3f-406">To capture a component reference:</span></span>

* <span data-ttu-id="93b3f-407">Alt bileşene bir [`@ref`](xref:mvc/views/razor#ref) özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-407">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="93b3f-408">Alt bileşenle aynı türde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-408">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="93b3f-409">Bileşen işlendiğinde `_loginDialog` alanı `MyLoginDialog` alt bileşen örneğiyle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-409">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="93b3f-410">Daha sonra bileşen örneğinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-410">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93b3f-411">`_loginDialog` değişkeni yalnızca bileşen işlendikten sonra ve çıktısı `MyLoginDialog` öğesini içerdiğinde doldurulur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-411">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="93b3f-412">Bu noktaya kadar başvurulmasına hiçbir şey yok.</span><span class="sxs-lookup"><span data-stu-id="93b3f-412">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="93b3f-413">Bileşen işlemesini tamamladıktan sonra bileşen başvurularını işlemek için [Onafterrenderasync veya OnAfterRender yöntemlerini](xref:blazor/lifecycle#after-component-render)kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-413">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="93b3f-414">Bileşen başvurularını yakalama, [öğe başvurularını yakalamak](xref:blazor/javascript-interop#capture-references-to-elements)için benzer bir sözdizimi kullanın, bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-414">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="93b3f-415">Bileşen başvuruları yalnızca .NET kodunda kullanıldıkları&mdash;JavaScript koduna aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-415">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="93b3f-416">Alt bileşenlerin durumunu bulunmamalıdır için bileşen **başvurularını kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="93b3f-416">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="93b3f-417">Bunun yerine, alt bileşenlere veri geçirmek için normal bildirime dayalı parametreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-417">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="93b3f-418">Normal bildirime dayalı parametrelerin kullanımı, otomatik olarak doğru zamanların yeniden yönlendirmesi için alt bileşenlerde oluşur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-418">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="93b3f-419">Durumu güncelleştirmek için bileşen yöntemlerini dışarıdan çağır</span><span class="sxs-lookup"><span data-stu-id="93b3f-419">Invoke component methods externally to update state</span></span>

<span data-ttu-id="93b3f-420">Blazor, yürütmenin tek bir mantıksal iş parçacığını zorlamak için bir `SynchronizationContext` kullanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-420">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="93b3f-421">Bir bileşenin [yaşam döngüsü yöntemleri](xref:blazor/lifecycle) ve Blazor tarafından oluşturulan tüm olay geri çağırmaları bu `SynchronizationContext`yürütülür.</span><span class="sxs-lookup"><span data-stu-id="93b3f-421">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="93b3f-422">Bir bileşenin bir zamanlayıcı veya diğer bildirimler gibi dış bir olaya göre güncellenmesi gerekir, `SynchronizationContext`Blazor ' ye gönderilecek `InvokeAsync` yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-422">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="93b3f-423">Örneğin, güncelleştirilmiş durumdaki herhangi bir dinleme bileşenine bildirimde bulunan bir *bildirim hizmeti* düşünün:</span><span class="sxs-lookup"><span data-stu-id="93b3f-423">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="93b3f-424">Bir bileşeni güncelleştirmek için `NotifierService` kullanımı:</span><span class="sxs-lookup"><span data-stu-id="93b3f-424">Usage of the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="93b3f-425">Yukarıdaki örnekte `NotifierService` bileşenin `OnNotify` yöntemini Blazor `SynchronizationContext`dışında çağırır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-425">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="93b3f-426">`InvokeAsync`, doğru bağlama geçmek ve bir işlemeyi kuyruğa almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-426">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="93b3f-427">Öğelerin ve bileşenlerin korunmasını denetlemek için \@anahtarını kullanın</span><span class="sxs-lookup"><span data-stu-id="93b3f-427">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="93b3f-428">Bir öğe veya bileşen listesi işlenirken ve öğeler ya da bileşenler daha sonra değiştiğinde, Blazor 'in yayılma algoritması, önceki öğelerin veya bileşenlerin ne zaman tutulacağına ve model nesnelerinin bunlara nasıl eşleneceğine karar vermelidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-428">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="93b3f-429">Normalde, bu işlem otomatiktir ve yoksayılabilir, ancak işlemi denetlemek isteyebileceğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-429">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="93b3f-430">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="93b3f-430">Consider the following example:</span></span>

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

<span data-ttu-id="93b3f-431">`People` koleksiyonun içerikleri, ekli, silinmiş veya yeniden sıralanmış girdilerle değişebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-431">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="93b3f-432">Bileşen yeniden oluşturulduğunda `<DetailsEditor>` bileşeni farklı `Details` parametre değerleri almak için değişebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-432">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="93b3f-433">Bu, beklenenden daha karmaşık rerendering oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-433">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="93b3f-434">Bazı durumlarda rerendering, kayıp öğe odağı gibi görünür davranış farklılıklarına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-434">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="93b3f-435">Eşleme işlemi `@key` Directive özniteliğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-435">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="93b3f-436">`@key`, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için dağıtılmış algoritmaya neden olur:</span><span class="sxs-lookup"><span data-stu-id="93b3f-436">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="93b3f-437">`People` koleksiyonu değiştiğinde, yayılma algoritması `<DetailsEditor>` örnekleri ve `person` örnekleri arasındaki ilişkilendirmeyi korur:</span><span class="sxs-lookup"><span data-stu-id="93b3f-437">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="93b3f-438">Bir `Person` `People` listesinden silinirse, yalnızca ilgili `<DetailsEditor>` örneği kullanıcı arabiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-438">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="93b3f-439">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-439">Other instances are left unchanged.</span></span>
* <span data-ttu-id="93b3f-440">Listedeki bir konuma `Person` eklenirse, ilgili konuma bir yeni `<DetailsEditor>` örneği eklenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-440">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="93b3f-441">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-441">Other instances are left unchanged.</span></span>
* <span data-ttu-id="93b3f-442">`Person` girdileri yeniden sıralandıysanız, karşılık gelen `<DetailsEditor>` örnekleri korunur ve Kullanıcı arabiriminde yeniden sıralanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-442">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="93b3f-443">Bazı senaryolarda `@key` kullanımı, rerendering karmaşıklığını en aza indirir ve odak konumu gibi DOM 'ın durum bilgisi olan kısımlarıyla ilgili olası sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-443">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93b3f-444">Anahtarlar her kapsayıcı öğesi veya bileşeni için yereldir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-444">Keys are local to each container element or component.</span></span> <span data-ttu-id="93b3f-445">Anahtarlar belge genelinde küresel olarak karşılaştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-445">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="93b3f-446">\@anahtarı ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="93b3f-446">When to use \@key</span></span>

<span data-ttu-id="93b3f-447">Genellikle, bir liste işlendiğinde (örneğin, bir `@foreach` bloğunda) ve `@key`tanımlamak için uygun bir değer olduğunda `@key` kullanmak mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-447">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="93b3f-448">Bir nesne değiştiğinde Blazor 'in bir öğeyi veya bileşen alt ağacını engellemesini engellemek için `@key` de kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="93b3f-448">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="93b3f-449">`@currentPerson` değişirse `@key` Attribute yönergesi, tüm `<div>` ve alt öğelerini atmayı ve yeni öğeler ve bileşenlerle Kullanıcı arabiriminde alt ağacı yeniden oluşturmayı zorlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-449">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="93b3f-450">`@currentPerson` değiştiğinde hiçbir Kullanıcı arabirimi durumunun korunmayacağını garanti etmeniz gerekirse bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-450">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="93b3f-451">\@anahtar ne zaman kullanılmaz</span><span class="sxs-lookup"><span data-stu-id="93b3f-451">When not to use \@key</span></span>

<span data-ttu-id="93b3f-452">`@key`bir performans maliyeti vardır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-452">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="93b3f-453">Performans maliyeti büyük değildir, ancak öğe veya bileşen koruma kurallarının uygulamanın avantajına göre denetlenmesi durumunda yalnızca `@key` belirtin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-453">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="93b3f-454">`@key` kullanılmasa bile, Blazor alt öğe ve bileşen örneklerini mümkün olduğunca korur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-454">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="93b3f-455">`@key` kullanmanın avantajı, model örneklerinin eşlemeyi seçme algoritması yerine, korunan bileşen örneklerine *nasıl* eşlendiğine ilişkin denetimdir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-455">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="93b3f-456">\@anahtarı için kullanılacak değerler</span><span class="sxs-lookup"><span data-stu-id="93b3f-456">What values to use for \@key</span></span>

<span data-ttu-id="93b3f-457">Genellikle, `@key`için aşağıdaki değer türlerinden birini sağlamak mantıklı olur:</span><span class="sxs-lookup"><span data-stu-id="93b3f-457">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="93b3f-458">Model nesne örnekleri (örneğin, önceki örnekte olduğu gibi `Person` örneği).</span><span class="sxs-lookup"><span data-stu-id="93b3f-458">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="93b3f-459">Bu, nesne başvurusu eşitliğine göre koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-459">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="93b3f-460">Benzersiz tanımlayıcılar (örneğin, `int`, `string`veya `Guid`için birincil anahtar değerleri).</span><span class="sxs-lookup"><span data-stu-id="93b3f-460">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="93b3f-461">`@key` için kullanılan değerlerin çakışmayın olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="93b3f-461">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="93b3f-462">Aynı üst öğe içinde çakışan değerler algılanırsa, eski öğeleri veya bileşenleri yeni öğe veya bileşenlere kesin bir şekilde eşlemediğinden Blazor bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-462">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="93b3f-463">Yalnızca nesne örnekleri veya birincil anahtar değerleri gibi farklı değerleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-463">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="93b3f-464">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="93b3f-464">Routing</span></span>

<span data-ttu-id="93b3f-465">Blazor ' de yönlendirme, uygulamadaki her erişilebilir bileşene bir rota şablonu sunarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-465">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="93b3f-466">`@page` yönergesine sahip bir Razor dosyası derlendiğinde, oluşturulan sınıfa yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> verilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-466">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="93b3f-467">Çalışma zamanında, yönlendirici bileşen sınıflarını bir `RouteAttribute` arar ve hangi bileşenin istenen URL ile eşleşen bir rota şablonuna sahip olduğunu işler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-467">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="93b3f-468">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-468">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="93b3f-469">Aşağıdaki bileşen `/BlazorRoute` ve `/DifferentBlazorRoute`isteklerine yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-469">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="93b3f-470">*Pages/BlazorRoute. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-470">*Pages/BlazorRoute.razor*:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a><span data-ttu-id="93b3f-471">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-471">Route parameters</span></span>

<span data-ttu-id="93b3f-472">Bileşenler, `@page` yönergesinde belirtilen yol şablonundan yol parametreleri alabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-472">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="93b3f-473">Yönlendirici, karşılık gelen bileşen parametrelerini doldurmak için yol parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-473">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="93b3f-474">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-474">*Pages/RouteParameter.razor*:</span></span>

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

<span data-ttu-id="93b3f-475">İsteğe bağlı parametreler desteklenmez, bu nedenle yukarıdaki örnekte iki `@page` yönergesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-475">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="93b3f-476">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-476">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="93b3f-477">İkinci `@page` yönergesi `{text}` Route parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-477">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="93b3f-478">*Catch-all* parametre sözdizimi (`*`/`**`), birden çok klasör sınırları genelinde yolu yakalayan Razor bileşenlerinde ( *. Razor* **) desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="93b3f-478">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="93b3f-479">Kısmi sınıf desteği</span><span class="sxs-lookup"><span data-stu-id="93b3f-479">Partial class support</span></span>

<span data-ttu-id="93b3f-480">Razor bileşenleri kısmi sınıflar olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-480">Razor components are generated as partial classes.</span></span> <span data-ttu-id="93b3f-481">Razor bileşenleri aşağıdaki yaklaşımlardan birini kullanarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-481">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="93b3f-482">C#kod, tek bir dosyada HTML işaretlemesi ve Razor kodu ile bir [`@code`](xref:mvc/views/razor#code) bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-482">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="93b3f-483">Blazor şablonları bu yaklaşımı kullanarak Razor bileşenlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-483">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="93b3f-484">C#kod, kısmi sınıf olarak tanımlanan bir arka plan kod dosyasına yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-484">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="93b3f-485">Aşağıdaki örnek, Blazor şablonundan oluşturulan bir uygulamada bir `@code` bloğu olan varsayılan `Counter` bileşenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-485">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="93b3f-486">HTML işaretleme, Razor kodu ve C# kod aynı dosyada:</span><span class="sxs-lookup"><span data-stu-id="93b3f-486">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="93b3f-487">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-487">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="93b3f-488">`Counter` bileşeni, kısmi bir sınıf içeren bir arka plan kod dosyası kullanılarak da oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-488">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="93b3f-489">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-489">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="93b3f-490">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-490">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="93b3f-491">Gerekli olan ad alanlarını kısmi sınıf dosyasına gereken şekilde ekleyin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-491">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="93b3f-492">Razor bileşenleri tarafından kullanılan tipik ad alanları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-492">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="93b3f-493">Temel sınıf belirtin</span><span class="sxs-lookup"><span data-stu-id="93b3f-493">Specify a base class</span></span>

<span data-ttu-id="93b3f-494">[`@inherits`](xref:mvc/views/razor#inherits) yönergesi, bir bileşen için temel sınıf belirtmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-494">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="93b3f-495">Aşağıdaki örnek, bileşenin özelliklerini ve yöntemlerini sağlamak için bir bileşenin `BlazorRocksBase`bir temel sınıfı nasıl devralmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-495">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="93b3f-496">Temel sınıf `ComponentBase`türetmelidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-496">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="93b3f-497">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-497">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="93b3f-498">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-498">*BlazorRocksBase.cs*:</span></span>

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

## <a name="specify-an-attribute"></a><span data-ttu-id="93b3f-499">Bir öznitelik belirtin</span><span class="sxs-lookup"><span data-stu-id="93b3f-499">Specify an attribute</span></span>

<span data-ttu-id="93b3f-500">Özellikler Razor bileşenlerinde [`@attribute`](xref:mvc/views/razor#attribute) yönergesi ile belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-500">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="93b3f-501">Aşağıdaki örnek bileşen sınıfına `[Authorize]` özniteliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="93b3f-501">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="93b3f-502">Bileşenleri içeri aktar</span><span class="sxs-lookup"><span data-stu-id="93b3f-502">Import components</span></span>

<span data-ttu-id="93b3f-503">Razor ile yazılan bir bileşenin ad alanı temel alınarak belirlenir (öncelik sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="93b3f-503">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="93b3f-504">Razor dosyası ( *. Razor*) biçimlendirmesinde [`@namespace`](xref:mvc/views/razor#namespace) ataması (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="93b3f-504">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="93b3f-505">Projenin proje dosyasında `RootNamespace` (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="93b3f-505">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="93b3f-506">Proje dosyasının dosya adından ( *. csproj*) ve proje kökünden bileşen yolundan alınan proje adı.</span><span class="sxs-lookup"><span data-stu-id="93b3f-506">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="93b3f-507">Örneğin Framework, *{Project root}/Pages/Index.Razor* (*BlazorSample. csproj*) ad alanına `BlazorSample.Pages`çözümleniyor.</span><span class="sxs-lookup"><span data-stu-id="93b3f-507">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="93b3f-508">Bileşenler ad C# bağlama kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-508">Components follow C# name binding rules.</span></span> <span data-ttu-id="93b3f-509">Bu örnekteki `Index` bileşeni için, kapsamdaki bileşenler tüm bileşenlerdir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-509">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="93b3f-510">Aynı klasörde, *sayfalarda*.</span><span class="sxs-lookup"><span data-stu-id="93b3f-510">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="93b3f-511">Proje kökündeki, açıkça farklı bir ad alanı belirtmeyen bileşenler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-511">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="93b3f-512">Farklı bir ad alanında tanımlanan bileşenler, Razor 'nin [`@using`](xref:mvc/views/razor#using) yönergesi kullanılarak kapsam içine getirilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-512">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="93b3f-513">*BlazorSample/Shared/* klasöründe `NavMenu.razor`başka bir bileşen varsa, bileşen aşağıdaki `@using` ifadesiyle `Index.razor` kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-513">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="93b3f-514">Bileşenlere Ayrıca, [`@using`](xref:mvc/views/razor#using) yönergesini gerektirmeyen tam nitelikli adları kullanılarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-514">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="93b3f-515">`global::` niteleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="93b3f-515">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="93b3f-516">Diğer ad `using` deyimleriyle bileşenleri içeri aktarma (örneğin, `@using Foo = Bar`) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-516">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="93b3f-517">Kısmen nitelenmiş adlar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-517">Partially qualified names aren't supported.</span></span> <span data-ttu-id="93b3f-518">Örneğin, `@using BlazorSample` eklemek ve `NavMenu.razor` başvurmak `<Shared.NavMenu></Shared.NavMenu>` desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-518">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="93b3f-519">Koşullu HTML öğesi öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-519">Conditional HTML element attributes</span></span>

<span data-ttu-id="93b3f-520">HTML öğesi öznitelikleri, .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-520">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="93b3f-521">Değer `false` veya `null`ise, öznitelik işlenmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-521">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="93b3f-522">Değer `true`ise, öznitelik küçültülmüş olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-522">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="93b3f-523">Aşağıdaki örnekte, `checked` öğenin biçimlendirmesinde işlenip işlenmeyeceğini `IsCompleted` belirler:</span><span class="sxs-lookup"><span data-stu-id="93b3f-523">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="93b3f-524">`IsCompleted` `true`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-524">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="93b3f-525">`IsCompleted` `false`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-525">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="93b3f-526">Daha fazla bilgi için bkz. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="93b3f-526">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="93b3f-527">.NET türü `bool`olduğunda, [Aria-basılan](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)gıbı bazı HTML öznitelikleri düzgün şekilde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-527">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="93b3f-528">Bu durumlarda, `bool`yerine `string` türü kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-528">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="93b3f-529">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="93b3f-529">Raw HTML</span></span>

<span data-ttu-id="93b3f-530">Dizeler normalde DOM metin düğümleri kullanılarak işlenir. Bu, içerdikleri tüm biçimlendirmenin yok sayıldığı ve değişmez değer olarak kabul edildiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-530">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="93b3f-531">Ham HTML işlemek için, HTML içeriğini bir `MarkupString` değerde sarın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-531">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="93b3f-532">Değer HTML veya SVG olarak ayrıştırılır ve DOM 'a eklenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-532">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="93b3f-533">Güvenilmeyen bir kaynaktan oluşturulan ham HTML işleme bir **güvenlik riskidir** ve kaçınılması gerekir!</span><span class="sxs-lookup"><span data-stu-id="93b3f-533">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="93b3f-534">Aşağıdaki örnek, bir bileşenin işlenmiş çıktısına statik HTML içeriği bloğunu eklemek için `MarkupString` türünü kullanmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-534">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="93b3f-535">Şablonlu bileşenler</span><span class="sxs-lookup"><span data-stu-id="93b3f-535">Templated components</span></span>

<span data-ttu-id="93b3f-536">Şablonlu bileşenler, bir veya daha fazla UI şablonunu parametre olarak kabul eden bileşenlerdir, daha sonra bileşen işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-536">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="93b3f-537">Şablonlu bileşenler, normal bileşenlerden daha yeniden kullanılabilir olan üst düzey bileşenleri yazmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-537">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="93b3f-538">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-538">A couple of examples include:</span></span>

* <span data-ttu-id="93b3f-539">Kullanıcının tablo üst bilgisi, satırları ve altbilgisi için şablon belirtmesini sağlayan tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="93b3f-539">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="93b3f-540">Bir kullanıcının bir listedeki öğeleri işlemek için şablon belirlemesine izin veren bir liste bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-540">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="93b3f-541">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-541">Template parameters</span></span>

<span data-ttu-id="93b3f-542">Şablonlu bir bileşen, `RenderFragment` veya `RenderFragment<T>`türünde bir veya daha fazla bileşen parametresi belirtilerek tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-542">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="93b3f-543">Bir işleme parçası, işlenecek Kullanıcı arabiriminin bir kesimini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="93b3f-543">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="93b3f-544">`RenderFragment<T>`, işleme parçası çağrıldığında belirtilebildiği bir tür parametresi alır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-544">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="93b3f-545">`TableTemplate` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="93b3f-545">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="93b3f-546">Şablonlu bir bileşen kullanırken, şablon parametreleri parametre adlarıyla eşleşen alt öğeler (`TableHeader` ve aşağıdaki örnekte `RowTemplate`) kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-546">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="93b3f-547">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-547">Template context parameters</span></span>

<span data-ttu-id="93b3f-548">Öğe olarak geçirilen `RenderFragment<T>` bileşen bağımsız değişkenleri `context` adında örtük bir parametreye sahiptir (örneğin, yukarıdaki kod örneğinden, `@context.PetId`), ancak parametre adını alt öğe üzerindeki `Context` özniteliğini kullanarak değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-548">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="93b3f-549">Aşağıdaki örnekte, `RowTemplate` öğenin `Context` özniteliği `pet` parametresini belirtir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-549">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="93b3f-550">Alternatif olarak, bileşen öğesinde `Context` özniteliğini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-550">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="93b3f-551">Belirtilen `Context` özniteliği belirtilen tüm şablon parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-551">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="93b3f-552">Bu, örtük alt içerik (herhangi bir sarmalama alt öğesi olmadan) için içerik parametre adını belirtmek istediğinizde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-552">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="93b3f-553">Aşağıdaki örnekte, `Context` özniteliği `TableTemplate` öğesinde görünür ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-553">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="93b3f-554">Genel olarak yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="93b3f-554">Generic-typed components</span></span>

<span data-ttu-id="93b3f-555">Şablonlu bileşenler çoğunlukla genel olarak türdedir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-555">Templated components are often generically typed.</span></span> <span data-ttu-id="93b3f-556">Örneğin, bir genel `ListViewTemplate` bileşeni `IEnumerable<T>` değerlerini işlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-556">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="93b3f-557">Genel bir bileşen tanımlamak için [`@typeparam`](xref:mvc/views/razor#typeparam) yönergesini kullanarak tür parametrelerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="93b3f-557">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="93b3f-558">Genel türsüz bileşenleri kullanırken tür parametresi mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-558">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="93b3f-559">Aksi halde tür parametresi, tür parametresinin adıyla eşleşen bir öznitelik kullanılarak açıkça belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-559">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="93b3f-560">Aşağıdaki örnekte, `TItem="Pet"` türü belirtir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-560">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="93b3f-561">Değerleri ve parametreleri basamaklama</span><span class="sxs-lookup"><span data-stu-id="93b3f-561">Cascading values and parameters</span></span>

<span data-ttu-id="93b3f-562">Bazı senaryolarda, özellikle birden çok bileşen katmanı olduğunda, [bileşen parametreleri](#component-parameters)kullanarak bir üst bileşenden bir alt bileşene veri akışı yapmak uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-562">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="93b3f-563">Değerleri ve parametreleri basamaklama, bir üst bileşenin tüm alt bileşenlerine değer sağlaması için kullanışlı bir yol sağlayarak bu sorunu çözebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-563">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="93b3f-564">Basamaklı değerler ve parametreler, bileşenlerin koordinasyonu için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-564">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="93b3f-565">Tema örneği</span><span class="sxs-lookup"><span data-stu-id="93b3f-565">Theme example</span></span>

<span data-ttu-id="93b3f-566">Örnek uygulamadan aşağıdaki örnekte, `ThemeInfo` sınıfı, uygulamanın belirli bir bölümündeki tüm düğmelerin aynı stili paylaştığı şekilde bileşen hiyerarşisinin akışını yapmak için tema bilgilerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-566">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="93b3f-567">*Uıthemeclasses/Themeınfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="93b3f-567">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="93b3f-568">Bir üst bileşen basamaklı değer bileşeni kullanılarak basamaklı bir değer sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-568">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="93b3f-569">`CascadingValue` bileşeni, bileşen hiyerarşisinin bir alt ağacını sarmalanmış ve bu alt ağaçta bulunan tüm bileşenlere tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-569">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="93b3f-570">Örneğin, örnek uygulama, `@Body` özelliğinin düzen gövdesini oluşturan tüm bileşenler için bir geçişli parametre olarak uygulamanın düzenlerindeki tema bilgilerini (`ThemeInfo`) belirtir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-570">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="93b3f-571">`ButtonClass`, düzen bileşeninde `btn-success` bir değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-571">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="93b3f-572">Tüm alt bileşenler, `ThemeInfo` basamaklı nesne aracılığıyla bu özelliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-572">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="93b3f-573">`CascadingValuesParametersLayout` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="93b3f-573">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="93b3f-574">Basamaklı değerleri kullanmak için, bileşenler `[CascadingParameter]` özniteliği kullanarak Geçişli Parametreler bildirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-574">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="93b3f-575">Basamaklı değerler, türe göre basamaklı parametrelere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-575">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="93b3f-576">Örnek uygulamada `CascadingValuesParametersTheme` bileşeni, `ThemeInfo` geçişli değeri basamaklı bir parametreye bağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-576">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="93b3f-577">Parametresi, bileşen tarafından görünen düğmelerden birine ait CSS sınıfını ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-577">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="93b3f-578">`CascadingValuesParametersTheme` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="93b3f-578">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="93b3f-579">Aynı alt ağaç içindeki aynı türdeki birden çok değeri basamakla, her bir `CascadingValue` bileşenine ve karşılık gelen `CascadingParameter`benzersiz bir `Name` dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-579">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="93b3f-580">Aşağıdaki örnekte, iki `CascadingValue` bileşeni, `MyCascadingType` farklı örneklerini ada göre basamakla:</span><span class="sxs-lookup"><span data-stu-id="93b3f-580">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="93b3f-581">Alt bileşende, basamaklı parametreler değerlerini, üst bileşendeki ilgili basamaklı değerleri ada göre alır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-581">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="93b3f-582">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="93b3f-582">TabSet example</span></span>

<span data-ttu-id="93b3f-583">Basamaklı parametreler, bileşenlerin bileşen hiyerarşisinde işbirliği yapmasına de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-583">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="93b3f-584">Örneğin, örnek uygulamada aşağıdaki *Tabset* örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="93b3f-584">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="93b3f-585">Örnek uygulama, sekmelerin uygulandığı bir `ITab` arabirimine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-585">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="93b3f-586">`CascadingValuesParametersTabSet` bileşeni, çeşitli `Tab` bileşenleri içeren `TabSet` bileşenini kullanır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-586">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

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

<span data-ttu-id="93b3f-587">Alt `Tab` bileşenleri, `TabSet`açıkça parametre olarak geçirilmemektedir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-587">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="93b3f-588">Bunun yerine, alt `Tab` bileşenleri `TabSet`alt içeriğinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-588">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="93b3f-589">Ancak, `TabSet` üst bilgileri ve etkin sekmeyi işleyebilmesi için, her bir `Tab` bileşeni hakkında yine de bilmesi gerekir. Ek kod gerektirmeden bu koordinasyonu etkinleştirmek için, `TabSet` bileşen kendisini, alt `Tab` bileşenleri tarafından çekilen *basamaklı bir değer olarak sağlayabilir* .</span><span class="sxs-lookup"><span data-stu-id="93b3f-589">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="93b3f-590">`TabSet` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="93b3f-590">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="93b3f-591">Alt `Tab` bileşenleri, kapsayan `TabSet` kendisini basamaklı bir parametre olarak yakalar, bu nedenle `Tab` bileşenleri kendilerini `TabSet` ve bu sekmenin etkin olduğu koordine eder.</span><span class="sxs-lookup"><span data-stu-id="93b3f-591">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="93b3f-592">`Tab` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="93b3f-592">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="93b3f-593">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="93b3f-593">Razor templates</span></span>

<span data-ttu-id="93b3f-594">Oluşturma parçaları Razor şablonu sözdizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-594">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="93b3f-595">Razor şablonları, bir UI parçacığı tanımlamak ve aşağıdaki biçimi varsaymak için bir yoldur:</span><span class="sxs-lookup"><span data-stu-id="93b3f-595">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="93b3f-596">Aşağıdaki örnek, `RenderFragment` ve `RenderFragment<T>` değerlerinin nasıl belirtildiğini ve şablonlarının doğrudan bir bileşende nasıl işleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-596">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="93b3f-597">Oluşturma parçaları, [şablonlu bileşenlere](#templated-components)bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-597">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="93b3f-598">Önceki kodun işlenmiş çıktısı:</span><span class="sxs-lookup"><span data-stu-id="93b3f-598">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="93b3f-599">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="93b3f-599">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="93b3f-600">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`, bileşenleri ve öğeleri işlemek için yöntemler sağlar ve C# kodu kodda el ile oluşturma dahil.</span><span class="sxs-lookup"><span data-stu-id="93b3f-600">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="93b3f-601">Bileşen oluşturmak için `RenderTreeBuilder` kullanımı gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-601">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="93b3f-602">Hatalı biçimlendirilmiş bir bileşen (örneğin, kapatılmamış bir biçimlendirme etiketi) tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-602">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="93b3f-603">Başka bir bileşende el ile yerleşik olarak kullanılabilecek aşağıdaki `PetDetails` bileşenini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="93b3f-603">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="93b3f-604">Aşağıdaki örnekte, `CreateComponent` yöntemindeki döngü üç `PetDetails` bileşeni oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-604">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="93b3f-605">Bileşenleri oluşturmak için `RenderTreeBuilder` Yöntemler çağrılırken (`OpenComponent` ve `AddAttribute`), dizi numaraları kaynak kodu satır numaralarıdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-605">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="93b3f-606">Blazor fark algoritması, ayrı çağrı etkinleştirmeleri değil ayrı kod satırlarına karşılık gelen sıra numaralarına dayanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-606">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="93b3f-607">`RenderTreeBuilder` yöntemlerle bir bileşen oluştururken, dizi numaraları için bağımsız değişkenleri sabit olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-607">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="93b3f-608">**Sıra numarasını oluşturmak için bir hesaplama veya sayaç kullanmak kötü performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="93b3f-608">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="93b3f-609">Daha fazla bilgi için bkz. [kod satırı numaralarıyla Ilgili sıra numaraları ve yürütme sırası çalışmıyor](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.</span><span class="sxs-lookup"><span data-stu-id="93b3f-609">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="93b3f-610">`BuiltContent` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="93b3f-610">`BuiltContent` component:</span></span>

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
> <span data-ttu-id="93b3f-611">`Microsoft.AspNetCore.Components.RenderTree` türler, işleme işlemlerinin *sonuçlarının* işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-611">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="93b3f-612">Bunlar, Blazor Framework uygulamasının iç ayrıntılardır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-612">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="93b3f-613">Bu türlerin *dengesizleşilmesi* ve gelecekteki sürümlerde değişikliğe tabi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-613">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="93b3f-614">Sıra numaraları, kod satırı numaralarıyla ilgilidir ve yürütme sırası değildir</span><span class="sxs-lookup"><span data-stu-id="93b3f-614">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="93b3f-615">Blazor `.razor` dosyaları her zaman derlenir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-615">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="93b3f-616">Derleme adımı, çalışma zamanında uygulama performansını geliştiren bilgileri eklemek için kullanılabilir olduğundan, bu büyük olasılıkla `.razor` için harika bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-616">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="93b3f-617">Bu geliştirmelerin önemli bir örneği *sıra numarası*içerir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-617">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="93b3f-618">Sıra numaraları, hangi çıkışların ayrı ve sıralı kod satırlarından geldiğini çalışma zamanına işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="93b3f-618">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="93b3f-619">Çalışma zamanı, doğrusal bir zamanda, genel ağaç farkı algoritması için genellikle mümkün olandan çok daha hızlı olan etkili ağaç SLA 'ları oluşturmak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-619">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="93b3f-620">Aşağıdaki Razor bileşeni ( *. Razor*) dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="93b3f-620">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="93b3f-621">Yukarıdaki kod, aşağıdakine benzer şekilde derlenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-621">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="93b3f-622">Kod ilk kez yürütüldüğünde, `someFlag` `true`, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-622">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="93b3f-623">Sequence</span><span class="sxs-lookup"><span data-stu-id="93b3f-623">Sequence</span></span> | <span data-ttu-id="93b3f-624">Tür</span><span class="sxs-lookup"><span data-stu-id="93b3f-624">Type</span></span>      | <span data-ttu-id="93b3f-625">Veri</span><span class="sxs-lookup"><span data-stu-id="93b3f-625">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="93b3f-626">0</span><span class="sxs-lookup"><span data-stu-id="93b3f-626">0</span></span>        | <span data-ttu-id="93b3f-627">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="93b3f-627">Text node</span></span> | <span data-ttu-id="93b3f-628">adı</span><span class="sxs-lookup"><span data-stu-id="93b3f-628">First</span></span>  |
| <span data-ttu-id="93b3f-629">1\.</span><span class="sxs-lookup"><span data-stu-id="93b3f-629">1</span></span>        | <span data-ttu-id="93b3f-630">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="93b3f-630">Text node</span></span> | <span data-ttu-id="93b3f-631">Saniye</span><span class="sxs-lookup"><span data-stu-id="93b3f-631">Second</span></span> |

<span data-ttu-id="93b3f-632">`someFlag` `false`hale geldiğini ve biçimlendirmenin yeniden işleneceğini varsayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-632">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="93b3f-633">Bu kez, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="93b3f-633">This time, the builder receives:</span></span>

| <span data-ttu-id="93b3f-634">Sequence</span><span class="sxs-lookup"><span data-stu-id="93b3f-634">Sequence</span></span> | <span data-ttu-id="93b3f-635">Tür</span><span class="sxs-lookup"><span data-stu-id="93b3f-635">Type</span></span>       | <span data-ttu-id="93b3f-636">Veri</span><span class="sxs-lookup"><span data-stu-id="93b3f-636">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="93b3f-637">1\.</span><span class="sxs-lookup"><span data-stu-id="93b3f-637">1</span></span>        | <span data-ttu-id="93b3f-638">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="93b3f-638">Text node</span></span>  | <span data-ttu-id="93b3f-639">Saniye</span><span class="sxs-lookup"><span data-stu-id="93b3f-639">Second</span></span> |

<span data-ttu-id="93b3f-640">Çalışma zamanı bir fark gerçekleştirdiğinde, sırasıyla `0` öğenin kaldırıldığını görür, bu nedenle aşağıdaki önemsiz *düzenleme betiğini*oluşturur:</span><span class="sxs-lookup"><span data-stu-id="93b3f-640">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="93b3f-641">İlk metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-641">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="93b3f-642">Program aracılığıyla sıra numaraları oluşturursanız ne yanlış gider</span><span class="sxs-lookup"><span data-stu-id="93b3f-642">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="93b3f-643">Aşağıdaki işleme Ağacı Oluşturucusu mantığını yazmanız yerine düşünün:</span><span class="sxs-lookup"><span data-stu-id="93b3f-643">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="93b3f-644">Şimdi ilk çıktı:</span><span class="sxs-lookup"><span data-stu-id="93b3f-644">Now, the first output is:</span></span>

| <span data-ttu-id="93b3f-645">Sequence</span><span class="sxs-lookup"><span data-stu-id="93b3f-645">Sequence</span></span> | <span data-ttu-id="93b3f-646">Tür</span><span class="sxs-lookup"><span data-stu-id="93b3f-646">Type</span></span>      | <span data-ttu-id="93b3f-647">Veri</span><span class="sxs-lookup"><span data-stu-id="93b3f-647">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="93b3f-648">0</span><span class="sxs-lookup"><span data-stu-id="93b3f-648">0</span></span>        | <span data-ttu-id="93b3f-649">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="93b3f-649">Text node</span></span> | <span data-ttu-id="93b3f-650">adı</span><span class="sxs-lookup"><span data-stu-id="93b3f-650">First</span></span>  |
| <span data-ttu-id="93b3f-651">1\.</span><span class="sxs-lookup"><span data-stu-id="93b3f-651">1</span></span>        | <span data-ttu-id="93b3f-652">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="93b3f-652">Text node</span></span> | <span data-ttu-id="93b3f-653">Saniye</span><span class="sxs-lookup"><span data-stu-id="93b3f-653">Second</span></span> |

<span data-ttu-id="93b3f-654">Bu sonuç önceki bir durum ile aynıdır, bu nedenle olumsuz bir sorun yoktur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-654">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="93b3f-655">`someFlag` ikinci işleme `false` ve çıktı:</span><span class="sxs-lookup"><span data-stu-id="93b3f-655">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="93b3f-656">Sequence</span><span class="sxs-lookup"><span data-stu-id="93b3f-656">Sequence</span></span> | <span data-ttu-id="93b3f-657">Tür</span><span class="sxs-lookup"><span data-stu-id="93b3f-657">Type</span></span>      | <span data-ttu-id="93b3f-658">Veri</span><span class="sxs-lookup"><span data-stu-id="93b3f-658">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="93b3f-659">0</span><span class="sxs-lookup"><span data-stu-id="93b3f-659">0</span></span>        | <span data-ttu-id="93b3f-660">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="93b3f-660">Text node</span></span> | <span data-ttu-id="93b3f-661">Saniye</span><span class="sxs-lookup"><span data-stu-id="93b3f-661">Second</span></span> |

<span data-ttu-id="93b3f-662">Bu kez, fark algoritması *iki* değişikliğin oluştuğunu görür ve algoritma aşağıdaki düzenleme betiğini üretir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-662">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="93b3f-663">İlk metin düğümünün değerini `Second`olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-663">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="93b3f-664">İkinci metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-664">Remove the second text node.</span></span>

<span data-ttu-id="93b3f-665">Sıra numaralarının oluşturulması, özgün kodda `if/else` dalların ve döngülerin bulunduğu yer hakkındaki tüm yararlı bilgileri kaybetti.</span><span class="sxs-lookup"><span data-stu-id="93b3f-665">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="93b3f-666">Bu, daha önce olduğu gibi bir fark **ile iki kez** sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-666">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="93b3f-667">Bu, önemsiz bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-667">This is a trivial example.</span></span> <span data-ttu-id="93b3f-668">Karmaşık ve derin iç içe yapıları ve özellikle döngülerle daha gerçekçi durumlarda performans maliyeti daha önemlidir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-668">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="93b3f-669">Hangi döngü bloklarının veya dallarının eklendiğini veya kaldırıldığını hemen belirlemek yerine, fark algoritmasının işleme ağaçlarına katmaları ve genellikle eski ve yeni yapıların nasıl olduğu hakkında yanlış bir biçimde düzenleme betikleri oluşturması birbirleriyle ilişkilendir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-669">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="93b3f-670">Kılavuz ve ekibinizle</span><span class="sxs-lookup"><span data-stu-id="93b3f-670">Guidance and conclusions</span></span>

* <span data-ttu-id="93b3f-671">Sıra numaraları dinamik olarak oluşturulursa uygulama performansı de vardır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-671">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="93b3f-672">Altyapı, derleme zamanında yakalanmadığı takdirde gerekli bilgiler bulunmadığından, çalışma zamanında kendi sıra numaralarını otomatik olarak oluşturamaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-672">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="93b3f-673">El ile uygulanan `RenderTreeBuilder` mantığı uzun blokları yazmayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-673">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="93b3f-674">`.razor` dosyalarını tercih edin ve derleyicinin sıra numaralarıyla uğraşmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="93b3f-674">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="93b3f-675">El ile `RenderTreeBuilder` mantığın olmaması durumunda, uzun kod bloklarını `OpenRegion`/`CloseRegion` çağrılarına kaydırılmış daha küçük parçalara ayırın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-675">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="93b3f-676">Her bölge kendi ayrı dizi numaralarına sahiptir, bu nedenle her bölge içinde sıfırdan (veya herhangi bir rastgele sayıdan) yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-676">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="93b3f-677">Dizi numaraları sabit kodluysa, fark algoritması yalnızca değer değerinde sıra numaralarının artırılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-677">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="93b3f-678">İlk değer ve boşluklar ilgisiz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-678">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="93b3f-679">Tek bir seçenek, kod satırı numarasını sıra numarası olarak kullanmak veya sıfırdan başlayıp bir ya da yüzlerce (ya da tercih edilen aralığa) artırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-679">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="93b3f-680"> sıra numaralarını kullanır, diğer ağaç dağıtma Kullanıcı arabirimi çerçeveleri bunları kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-680"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="93b3f-681">Dizi numaraları kullanıldığında, yayılma çok daha hızlıdır ve Blazor, *. Razor* dosyaları yazan geliştiriciler için otomatik olarak sıra numaralarıyla ilgilenen bir derleme adımının avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-681">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="93b3f-682">Yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="93b3f-682">Localization</span></span>

Blazor<span data-ttu-id="93b3f-683"> Server uygulamaları, [Yerelleştirme ara yazılımı](xref:fundamentals/localization#localization-middleware)kullanılarak yerelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-683"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="93b3f-684">Ara yazılım, uygulamadan kaynak isteyen kullanıcılar için uygun kültürü seçer.</span><span class="sxs-lookup"><span data-stu-id="93b3f-684">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="93b3f-685">Kültür aşağıdaki yaklaşımlardan biri kullanılarak ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-685">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="93b3f-686">Çerezler</span><span class="sxs-lookup"><span data-stu-id="93b3f-686">Cookies</span></span>](#cookies)
* [<span data-ttu-id="93b3f-687">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="93b3f-687">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="93b3f-688">Daha fazla bilgi ve örnek için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="93b3f-688">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="93b3f-689">Bağlayıcıyı Uluslararası hale getirme için yapılandırma (Blazor WebAssembly)</span><span class="sxs-lookup"><span data-stu-id="93b3f-689">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="93b3f-690">Varsayılan olarak, Blazor WebAssembly uygulamaları için Blazorbağlayıcı yapılandırması, açıkça istenen yerel ayarlar dışında uluslararası duruma getirme bilgilerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-690">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="93b3f-691">Bağlayıcının davranışını denetleme hakkında daha fazla bilgi ve yönergeler için bkz. <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="93b3f-691">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="93b3f-692">Özgü</span><span class="sxs-lookup"><span data-stu-id="93b3f-692">Cookies</span></span>

<span data-ttu-id="93b3f-693">Yerelleştirme kültürü tanımlama bilgisi kullanıcının kültürünü kalıcı hale getirebilirler.</span><span class="sxs-lookup"><span data-stu-id="93b3f-693">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="93b3f-694">Tanımlama bilgisi, uygulamanın ana bilgisayar sayfasının `OnGet` yöntemiyle oluşturulur (*Pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="93b3f-694">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="93b3f-695">Yerelleştirme ara yazılımı, sonraki isteklerde Kullanıcı kültürünü ayarlamak için tanımlama bilgilerini okur.</span><span class="sxs-lookup"><span data-stu-id="93b3f-695">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="93b3f-696">Tanımlama bilgisinin kullanımı, WebSocket bağlantısının kültürü doğru şekilde yaymasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-696">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="93b3f-697">Yerelleştirme şemaları URL yolunu veya sorgu dizesini temel alıyorsa, düzen WebSockets ile çalışmayabilir, bu nedenle kültürü kalıcı hale getiremeyebilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-697">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="93b3f-698">Bu nedenle, yerelleştirme kültürü tanımlama bilgisinin kullanılması önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-698">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="93b3f-699">Kültür bir yerelleştirme tanımlama bilgisinde kalıcı hale getirilir kültür atamak için herhangi bir teknik kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-699">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="93b3f-700">Uygulamanın zaten sunucu tarafı ASP.NET Core için bir yerelleştirme şeması varsa, uygulamanın var olan yerelleştirme altyapısını kullanmaya devam edin ve uygulamanın şeması içinde yerelleştirme kültür tanımlama bilgisini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-700">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="93b3f-701">Aşağıdaki örnekte, yerelleştirme ara yazılımı tarafından okunabilen bir tanımlama bilgisinde geçerli kültürün nasıl ayarlanacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-701">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="93b3f-702">Blazor Server uygulamasında aşağıdaki içeriklerle bir *Pages/Host. cshtml. cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="93b3f-702">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="93b3f-703">Yerelleştirme uygulamada işlenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-703">Localization is handled in the app:</span></span>

1. <span data-ttu-id="93b3f-704">Tarayıcı, uygulamaya bir ilk HTTP isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-704">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="93b3f-705">Kültür, yerelleştirme ara yazılımı tarafından atanır.</span><span class="sxs-lookup"><span data-stu-id="93b3f-705">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="93b3f-706">*_Host. cshtml. cs* içindeki `OnGet` yöntemi, yanıtın bir parçası olarak bir tanımlama bilgisinde kültürü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-706">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="93b3f-707">Tarayıcı, etkileşimli bir Blazor sunucusu oturumu oluşturmak için bir WebSocket bağlantısı açar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-707">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="93b3f-708">Yerelleştirme ara yazılımı tanımlama bilgisini okur ve kültürü atar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-708">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="93b3f-709">Blazor Server oturumu doğru kültür ile başlar.</span><span class="sxs-lookup"><span data-stu-id="93b3f-709">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="93b3f-710">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="93b3f-710">Provide UI to choose the culture</span></span>

<span data-ttu-id="93b3f-711">Bir kullanıcının bir kültür seçmesine izin vermek için kullanıcı ARABIRIMI sağlamak üzere bir *yeniden yönlendirme tabanlı yaklaşım* önerilir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-711">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="93b3f-712">Bu işlem, bir Kullanıcı bir oturum açma sayfasına yeniden yönlendirildiği ve ardından özgün kaynağa yeniden yönlendirildiği&mdash;bir Kullanıcı güvenli bir kaynağa erişmeyi denediğinde bir Web uygulamasında gerçekleşmelere benzer.</span><span class="sxs-lookup"><span data-stu-id="93b3f-712">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="93b3f-713">Uygulama, bir denetleyiciye yeniden yönlendirme yoluyla kullanıcının seçili kültürünü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-713">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="93b3f-714">Denetleyici kullanıcının seçili kültürünü bir tanımlama bilgisine ayarlar ve kullanıcıyı özgün URI 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-714">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="93b3f-715">Bir tanımlama bilgisinde kullanıcının seçili kültürünü ayarlamak ve özgün URI 'ye yeniden yönlendirmeyi gerçekleştirmek için sunucuda bir HTTP uç noktası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="93b3f-715">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="93b3f-716">Açık yeniden yönlendirme saldırılarını engellemek için `LocalRedirect` eylemi sonucunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-716">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="93b3f-717">Daha fazla bilgi için bkz. <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="93b3f-717">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="93b3f-718">Aşağıdaki bileşen, Kullanıcı bir kültür seçtiğinde ilk yeniden yönlendirmenin nasıl gerçekleştirileceği hakkında bir örnek göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-718">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="93b3f-719">Blazor uygulamalarında .NET yerelleştirme senaryolarını kullanma</span><span class="sxs-lookup"><span data-stu-id="93b3f-719">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="93b3f-720">Blazor uygulamalar içinde, aşağıdaki .NET yerelleştirme ve genelleştirme senaryoları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-720">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="93b3f-721">. NET ' in kaynak sistemi</span><span class="sxs-lookup"><span data-stu-id="93b3f-721">.NET's resources system</span></span>
* <span data-ttu-id="93b3f-722">Kültüre özgü sayı ve Tarih biçimlendirmesi</span><span class="sxs-lookup"><span data-stu-id="93b3f-722">Culture-specific number and date formatting</span></span>

Blazor<span data-ttu-id="93b3f-723">`@bind` işlevselliği, kullanıcının geçerli kültürüne göre Genelleştirme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-723">'s `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="93b3f-724">Daha fazla bilgi için [veri bağlama](#data-binding) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="93b3f-724">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="93b3f-725">Sınırlı bir ASP.NET Core yerelleştirme senaryosu kümesi şu anda desteklenmektedir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-725">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="93b3f-726">`IStringLocalizer<>` Blazor uygulamalarda *desteklenir* .</span><span class="sxs-lookup"><span data-stu-id="93b3f-726">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="93b3f-727">`IHtmlLocalizer<>`, `IViewLocalizer<>`ve veri ek açıklamaları yerelleştirme ASP.NET Core MVC senaryolarından ve Blazor uygulamalarında **desteklenmez** .</span><span class="sxs-lookup"><span data-stu-id="93b3f-727">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="93b3f-728">Daha fazla bilgi için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="93b3f-728">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="93b3f-729">Ölçeklenebilir vektör grafik (SVG) görüntüleri</span><span class="sxs-lookup"><span data-stu-id="93b3f-729">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="93b3f-730">Blazor HTML oluşturduğundan, ölçeklenebilir vektör grafik (SVG) görüntüleri ( *. SVG*) dahil olmak üzere tarayıcıda desteklenen görüntüler `<img>` etiketi aracılığıyla desteklenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-730">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="93b3f-731">Benzer şekilde, SVG görüntüleri bir stil sayfası dosyasının ( *. css*) CSS kurallarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="93b3f-731">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="93b3f-732">Ancak, satır içi SVG işaretlemesi tüm senaryolarda desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="93b3f-732">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="93b3f-733">Bir `<svg>` etiketini doğrudan bir bileşen dosyasına ( *. Razor*) yerleştirirseniz, temel görüntü işleme desteklenir, ancak birçok gelişmiş senaryo desteklenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-733">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="93b3f-734">Örneğin, `<use>` Etiketler Şu anda dikkate alınamaz ve `@bind` bazı SVG etiketleriyle kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-734">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="93b3f-735">Gelecekteki bir sürümde bu sınırlamaları ele almayı bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="93b3f-735">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93b3f-736">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="93b3f-736">Additional resources</span></span>

* <span data-ttu-id="93b3f-737"><xref:security/blazor/server> &ndash;, kaynak tükenmesi ile Çekişmek zorunda olan Blazor sunucu uygulamaları oluşturmaya yönelik yönergeler Içerir.</span><span class="sxs-lookup"><span data-stu-id="93b3f-737"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
