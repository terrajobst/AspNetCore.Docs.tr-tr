---
title: ASP.NET Core kısmi görünümler
author: ardalis
description: Büyük biçimlendirme dosyalarını bölmek ve ASP.NET Core uygulamalarında Web sayfalarında ortak biçimlendirmenin çoğaltılmasını azaltmak için kısmi görünümleri nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 06/12/2019
uid: mvc/views/partial
ms.openlocfilehash: 50c4f41d5d3099184aa3992ed7e176b74c488d2a
ms.sourcegitcommit: 805f625d16d74e77f02f5f37326e5aceafcb78e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985565"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="a405d-103">ASP.NET Core kısmi görünümler</span><span class="sxs-lookup"><span data-stu-id="a405d-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="a405d-104">[Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [madan Jendoubı](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Scott Sauber](https://twitter.com/scottsauber) tarafından</span><span class="sxs-lookup"><span data-stu-id="a405d-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="a405d-105">Kısmi görünüm *, başka bir* biçimlendirme dosyasının işlenmiş ÇıKTıSıNDAKI HTML çıkışını Işleyen bir [Razor](xref:mvc/views/razor) biçimlendirme dosyasıdır ( *. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="a405d-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a405d-106">*Kısmi görünüm* terimi, biçimlendirme dosyaları *Görünümler*olarak adlandırılan bir MVC uygulaması veya biçimlendirme dosyalarının *Sayfalar*olarak adlandırıldığını Razor Pages bir uygulama geliştirirken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a405d-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="a405d-107">Bu konu, MVC görünümlerini ve Razor Pages sayfalarını *biçimlendirme dosyaları*olarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="a405d-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="a405d-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a405d-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="a405d-109">Kısmi görünümlerin ne zaman kullanılacağı</span><span class="sxs-lookup"><span data-stu-id="a405d-109">When to use partial views</span></span>

<span data-ttu-id="a405d-110">Kısmi görünümler şu şekilde etkili bir yoldur:</span><span class="sxs-lookup"><span data-stu-id="a405d-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="a405d-111">Büyük biçimlendirme dosyalarını daha küçük bileşenlere bölün.</span><span class="sxs-lookup"><span data-stu-id="a405d-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="a405d-112">Birkaç mantıksal parçadan oluşan büyük, karmaşık bir biçimlendirme dosyasında, her bir parçada kısmi bir görünümde yalıtılmış olarak çalışmanın bir avantajı vardır.</span><span class="sxs-lookup"><span data-stu-id="a405d-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="a405d-113">Biçimlendirme dosyasındaki kod, biçimlendirme yalnızca genel sayfa yapısını ve kısmi görünümlere yönelik başvuruları içerdiğinden yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="a405d-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="a405d-114">Biçimlendirme dosyaları arasında ortak biçimlendirme içeriğinin çoğaltılmasını azaltın.</span><span class="sxs-lookup"><span data-stu-id="a405d-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="a405d-115">Biçimlendirme dosyalarında aynı biçimlendirme öğeleri kullanıldığında, kısmi bir görünüm biçimlendirme içeriğinin tek bir kısmi görünüm dosyasına çoğaltılmasını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="a405d-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="a405d-116">Kısmi görünümdeki biçimlendirme değiştirildiğinde, kısmi görünümü kullanan biçimlendirme dosyalarının işlenmiş çıkışını günceller.</span><span class="sxs-lookup"><span data-stu-id="a405d-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="a405d-117">Yaygın düzen öğelerini korumak için kısmi görünümler kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a405d-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="a405d-118">[_Layout. cshtml](xref:mvc/views/layout) dosyalarında ortak düzen öğeleri belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a405d-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="a405d-119">Biçimlendirmeyi işlemek için karmaşık işleme mantığının veya kod yürütmenin gerekli olduğu kısmi bir görünüm kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a405d-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="a405d-120">Kısmi bir görünüm yerine bir [Görünüm bileşeni](xref:mvc/views/view-components)kullanın.</span><span class="sxs-lookup"><span data-stu-id="a405d-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="a405d-121">Kısmi görünümler bildirme</span><span class="sxs-lookup"><span data-stu-id="a405d-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a405d-122">Kısmi görünüm, *Görünümler* klasörü (MVC) veya *sayfalar* klasörü (Razor Pages) içinde tutulan bir *. cshtml* biçimlendirme dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="a405d-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="a405d-123">ASP.NET Core MVC 'de, denetleyici <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir görünüm veya kısmi görünüm döndürmektedir.</span><span class="sxs-lookup"><span data-stu-id="a405d-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="a405d-124">Razor Pages, bir <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> <xref:Microsoft.AspNetCore.Mvc.PartialViewResult> nesnesi olarak temsil edilen kısmi bir görünüm döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="a405d-124">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a partial view represented as a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult> object.</span></span> <span data-ttu-id="a405d-125">Kısmi görünümlere başvurmak ve işlemek [kısmi görünüm başvurusu](#reference-a-partial-view) bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a405d-125">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="a405d-126">MVC görünümü veya sayfa işleme farklı olarak, kısmi bir görünüm *_Viewstart. cshtml*çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="a405d-126">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="a405d-127">*_Viewstart. cshtml*hakkında daha fazla bilgi için bkz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="a405d-127">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="a405d-128">Kısmi görünüm dosya adları genellikle bir alt çizgi (`_`) ile başlar.</span><span class="sxs-lookup"><span data-stu-id="a405d-128">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="a405d-129">Bu adlandırma kuralı gerekli değildir, ancak görünüm ve sayfalardan kısmi görünümleri görsel açıdan ayırt etmeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a405d-129">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a405d-130">Kısmi görünüm, *Görünümler* klasörü içinde tutulan bir *. cshtml* biçimlendirme dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="a405d-130">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="a405d-131">Denetleyicinin bir görünüm <xref:Microsoft.AspNetCore.Mvc.ViewResult> veya kısmi görünüm döndürme özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="a405d-131">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="a405d-132">Kısmi görünümlere başvurmak ve işlemek [kısmi görünüm başvurusu](#reference-a-partial-view) bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a405d-132">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="a405d-133">MVC görünüm işlemenin aksine, kısmi bir görünüm *_Viewstart. cshtml*çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="a405d-133">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="a405d-134">*_Viewstart. cshtml*hakkında daha fazla bilgi için bkz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="a405d-134">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="a405d-135">Kısmi görünüm dosya adları genellikle bir alt çizgi (`_`) ile başlar.</span><span class="sxs-lookup"><span data-stu-id="a405d-135">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="a405d-136">Bu adlandırma kuralı gerekli değildir, ancak kısmen görünümlerini görünümlerde görsel açıdan ayırt etmeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a405d-136">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="a405d-137">Kısmi görünüme başvur</span><span class="sxs-lookup"><span data-stu-id="a405d-137">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.0"

### <a name="use-a-partial-view-in-a-razor-pages-pagemodel"></a><span data-ttu-id="a405d-138">Razor Pages PageModel içinde kısmi bir görünüm kullanma</span><span class="sxs-lookup"><span data-stu-id="a405d-138">Use a partial view in a Razor Pages PageModel</span></span>

<span data-ttu-id="a405d-139">ASP.NET Core 2,0 veya 2,1 ' de, aşağıdaki işleyici yöntemi  *\_authorpartialrp. cshtml* kısmi görünümünü yanıta işler:</span><span class="sxs-lookup"><span data-stu-id="a405d-139">In ASP.NET Core 2.0 or 2.1, the following handler method renders the *\_AuthorPartialRP.cshtml* partial view to the response:</span></span>

```csharp
public IActionResult OnGetPartial() =>
    new PartialViewResult
    {
        ViewName = "_AuthorPartialRP",
        ViewData = ViewData,
    };
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a405d-140">ASP.NET Core 2,2 veya sonraki sürümlerde, bir işleyici yöntemi alternatif olarak bir <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Partial*> `PartialViewResult` nesnesi oluşturmak için yöntemini çağırabilir:</span><span class="sxs-lookup"><span data-stu-id="a405d-140">In ASP.NET Core 2.2 or later, a handler method can alternatively call the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Partial*> method to produce a `PartialViewResult` object:</span></span>

[!code-csharp[](partial/sample/PartialViewsSample/Pages/DiscoveryRP.cshtml.cs?name=snippet_OnGetPartial)]

::: moniker-end

### <a name="use-a-partial-view-in-a-markup-file"></a><span data-ttu-id="a405d-141">Biçimlendirme dosyasında kısmi görünüm kullanma</span><span class="sxs-lookup"><span data-stu-id="a405d-141">Use a partial view in a markup file</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a405d-142">Bir biçimlendirme dosyasında kısmi bir görünüme başvurmak için birkaç yol vardır.</span><span class="sxs-lookup"><span data-stu-id="a405d-142">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="a405d-143">Uygulamaların aşağıdaki zaman uyumsuz işleme yaklaşımlardan birini kullanmasını öneririz:</span><span class="sxs-lookup"><span data-stu-id="a405d-143">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="a405d-144">Kısmi Etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a405d-144">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="a405d-145">Zaman uyumsuz HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a405d-145">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a405d-146">Bir biçimlendirme dosyasında, kısmi bir görünüme başvurmak için iki yol vardır:</span><span class="sxs-lookup"><span data-stu-id="a405d-146">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="a405d-147">Zaman uyumsuz HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a405d-147">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="a405d-148">Zaman uyumlu HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a405d-148">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="a405d-149">Uygulamaların [zaman uyumsuz HTML yardımcısını](#asynchronous-html-helper)kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="a405d-149">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="a405d-150">Kısmi etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a405d-150">Partial Tag Helper</span></span>

<span data-ttu-id="a405d-151">[Kısmi etiket yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) ASP.NET Core 2,1 veya sonraki bir sürümü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a405d-151">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="a405d-152">Kısmi etiket Yardımcısı içeriği zaman uyumsuz olarak işler ve HTML benzeri bir sözdizimi kullanır:</span><span class="sxs-lookup"><span data-stu-id="a405d-152">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="a405d-153">Bir dosya uzantısı mevcut olduğunda, etiket Yardımcısı kısmi görünümü çağıran biçimlendirme dosyasıyla aynı klasörde olması gereken kısmi bir görünüme başvurur:</span><span class="sxs-lookup"><span data-stu-id="a405d-153">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="a405d-154">Aşağıdaki örnek, uygulama kökünden kısmi bir görünüme başvurur.</span><span class="sxs-lookup"><span data-stu-id="a405d-154">The following example references a partial view from the app root.</span></span> <span data-ttu-id="a405d-155">Bir tilde işareti (`~/`) veya eğik çizgi (`/`) ile başlayan yollar uygulama köküne başvurur:</span><span class="sxs-lookup"><span data-stu-id="a405d-155">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="a405d-156">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a405d-156">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="a405d-157">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a405d-157">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="a405d-158">Aşağıdaki örnek, göreli bir yol ile kısmi bir görünüme başvurur:</span><span class="sxs-lookup"><span data-stu-id="a405d-158">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="a405d-159">Daha fazla bilgi için bkz. <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="a405d-159">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="a405d-160">Zaman uyumsuz HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a405d-160">Asynchronous HTML Helper</span></span>

<span data-ttu-id="a405d-161">Bir HTML Yardımcısı kullanırken en iyi yöntem <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a405d-161">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="a405d-162">`PartialAsync`içinde Sarmalanan bir <xref:Microsoft.AspNetCore.Html.IHtmlContent> tür döndürür. <xref:System.Threading.Tasks.Task%601></span><span class="sxs-lookup"><span data-stu-id="a405d-162">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task%601>.</span></span> <span data-ttu-id="a405d-163">Yöntemine, beklenen çağrının bir `@` karakterle önek olarak eklenerek başvurulur:</span><span class="sxs-lookup"><span data-stu-id="a405d-163">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="a405d-164">Dosya uzantısı varsa, HTML Yardımcısı kısmi görünümü çağıran biçimlendirme dosyasıyla aynı klasörde olması gereken kısmi bir görünüme başvurur:</span><span class="sxs-lookup"><span data-stu-id="a405d-164">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="a405d-165">Aşağıdaki örnek, uygulama kökünden kısmi bir görünüme başvurur.</span><span class="sxs-lookup"><span data-stu-id="a405d-165">The following example references a partial view from the app root.</span></span> <span data-ttu-id="a405d-166">Bir tilde işareti (`~/`) veya eğik çizgi (`/`) ile başlayan yollar uygulama köküne başvurur:</span><span class="sxs-lookup"><span data-stu-id="a405d-166">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a405d-167">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a405d-167">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="a405d-168">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a405d-168">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="a405d-169">Aşağıdaki örnek, göreli bir yol ile kısmi bir görünüme başvurur:</span><span class="sxs-lookup"><span data-stu-id="a405d-169">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="a405d-170">Alternatif olarak, ile <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>kısmi bir görünüm işleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a405d-170">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="a405d-171">Bu yöntem bir <xref:Microsoft.AspNetCore.Html.IHtmlContent>döndürmez.</span><span class="sxs-lookup"><span data-stu-id="a405d-171">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="a405d-172">İşlenmiş çıktıyı doğrudan yanıta akıp.</span><span class="sxs-lookup"><span data-stu-id="a405d-172">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="a405d-173">Yöntem bir sonuç döndürmediği için, bir Razor kod bloğu içinde çağrılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="a405d-173">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="a405d-174">`RenderPartialAsync` , İçeriği oluşturduğundan, bazı senaryolarda daha iyi performans sağlar.</span><span class="sxs-lookup"><span data-stu-id="a405d-174">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="a405d-175">Performans açısından kritik durumlarda, her iki yaklaşımı kullanarak sayfayı kıyaslar ve daha hızlı bir yanıt üreten yaklaşımı kullanır.</span><span class="sxs-lookup"><span data-stu-id="a405d-175">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="a405d-176">Zaman uyumlu HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a405d-176">Synchronous HTML Helper</span></span>

<span data-ttu-id="a405d-177"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*>ve sırasıyla zaman uyumlu `RenderPartialAsync` `PartialAsync` <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> eşdeğerlerdir.</span><span class="sxs-lookup"><span data-stu-id="a405d-177"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="a405d-178">Zaman uyumlu eşdeğerleri, kilitlendikleri senaryolar olduğu için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="a405d-178">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="a405d-179">Zaman uyumlu yöntemler gelecek sürümlerde kaldırılmak üzere hedeflenmiştir.</span><span class="sxs-lookup"><span data-stu-id="a405d-179">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a405d-180">Kodu yürütmeniz gerekiyorsa, kısmi bir görünüm yerine bir [Görünüm bileşeni](xref:mvc/views/view-components) kullanın.</span><span class="sxs-lookup"><span data-stu-id="a405d-180">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a405d-181">Bir `Partial` Visual `RenderPartial` Studio Çözümleyicisi uyarısıyla çağırma veya sonuç.</span><span class="sxs-lookup"><span data-stu-id="a405d-181">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="a405d-182">Örneğin, varlığı `Partial` aşağıdaki uyarı iletisini verir:</span><span class="sxs-lookup"><span data-stu-id="a405d-182">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="a405d-183">Ihtmlhelper. Partial kullanımı uygulama kilitlenmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a405d-183">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="a405d-184">&lt;Kısmi&gt; etiket Yardımcısı veya ıhtmlhelper. partıalasync kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="a405d-184">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="a405d-185">Çağrıları `@Html.Partial` ile veya [kısmi etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)ile `@await Html.PartialAsync` değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a405d-185">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="a405d-186">Kısmi etiket Yardımcısı geçişi hakkında daha fazla bilgi için bkz. [HTML Yardımcısı 'Ndan geçiş](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="a405d-186">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="a405d-187">Kısmi görünüm bulma</span><span class="sxs-lookup"><span data-stu-id="a405d-187">Partial view discovery</span></span>

<span data-ttu-id="a405d-188">Bir dosya uzantısı olmayan kısmi bir görünüme ad ile başvurulduğunda, aşağıdaki konumlar belirtilen sırada aranır:</span><span class="sxs-lookup"><span data-stu-id="a405d-188">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a405d-189">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a405d-189">**Razor Pages**</span></span>

1. <span data-ttu-id="a405d-190">Şu anda sayfanın klasörü yürütülüyor</span><span class="sxs-lookup"><span data-stu-id="a405d-190">Currently executing page's folder</span></span>
1. <span data-ttu-id="a405d-191">Sayfanın klasörünün üzerindeki Dizin grafiği</span><span class="sxs-lookup"><span data-stu-id="a405d-191">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="a405d-192">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a405d-192">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="a405d-193">Kısmi görünüm bulma için aşağıdaki kurallar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="a405d-193">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="a405d-194">Kısmi görünümler farklı klasörlerde olduğunda aynı dosya adına sahip farklı kısmi görünümlere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="a405d-194">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="a405d-195">Dosya uzantısı olmadan kısmi bir görünüme ada göre başvurulması ve kısmi görünümün hem arayanın klasöründe hem de *paylaşılan* klasörde mevcut olması halinde, çağıranın klasöründeki kısmi görünüm kısmi görünümü sağlar.</span><span class="sxs-lookup"><span data-stu-id="a405d-195">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="a405d-196">Kısmi görünüm çağıranın klasöründe yoksa, kısmi görünüm *paylaşılan* klasörden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a405d-196">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="a405d-197">*Paylaşılan* klasördeki kısmi görünümler, *paylaşılan kısmi görünümler* veya *varsayılan kısmi görünümler*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a405d-197">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="a405d-198">Kısmi Görünümler *zincirleme*&mdash;olabilir kısmi görünüm, çağrılar tarafından bir döngüsel başvuru oluşturulmadığı durumlarda başka bir kısmi görünümü çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="a405d-198">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="a405d-199">Göreli yollar her zaman geçerli dosyaya göredir, dosyanın köküne veya üst öğesine göre değil.</span><span class="sxs-lookup"><span data-stu-id="a405d-199">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="a405d-200">Kısmi görünümde tanımlanan bir [Razor](xref:mvc/views/razor) `section` , üst biçimlendirme dosyaları için görünmez değildir.</span><span class="sxs-lookup"><span data-stu-id="a405d-200">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="a405d-201">`section` Yalnızca tanımlandığı kısmi görünüm için görülebilir.</span><span class="sxs-lookup"><span data-stu-id="a405d-201">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="a405d-202">Kısmi görünümlerde verilere erişin</span><span class="sxs-lookup"><span data-stu-id="a405d-202">Access data from partial views</span></span>

<span data-ttu-id="a405d-203">Kısmi bir görünüm örneği oluşturulduğunda, üst öğenin `ViewData` sözlüğünün bir *kopyasını* alır.</span><span class="sxs-lookup"><span data-stu-id="a405d-203">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="a405d-204">Kısmi görünüm içindeki verilerde yapılan güncelleştirmeler üst görünümde kalıcı değildir.</span><span class="sxs-lookup"><span data-stu-id="a405d-204">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="a405d-205">`ViewData`kısmi görünüm geri döndüğünde kısmi görünümdeki değişiklikler kaybolur.</span><span class="sxs-lookup"><span data-stu-id="a405d-205">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="a405d-206">Aşağıdaki örnek, bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) örneğinin kısmi bir görünüme nasıl geçirileceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="a405d-206">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="a405d-207">Bir modeli kısmi bir görünüme geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a405d-207">You can pass a model into a partial view.</span></span> <span data-ttu-id="a405d-208">Model özel bir nesne olabilir.</span><span class="sxs-lookup"><span data-stu-id="a405d-208">The model can be a custom object.</span></span> <span data-ttu-id="a405d-209">Bir modeli ile `PartialAsync` geçirebilirsiniz (bir içerik bloğunu çağırana kaydedebilir) veya `RenderPartialAsync` (içeriği çıkışa akıp):</span><span class="sxs-lookup"><span data-stu-id="a405d-209">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a405d-210">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a405d-210">**Razor Pages**</span></span>

<span data-ttu-id="a405d-211">Örnek uygulamada aşağıdaki biçimlendirme, *Pages/ArticlesRP/ReadRP. cshtml* sayfasından yapılır.</span><span class="sxs-lookup"><span data-stu-id="a405d-211">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="a405d-212">Sayfada iki kısmi görünüm bulunur.</span><span class="sxs-lookup"><span data-stu-id="a405d-212">The page contains two partial views.</span></span> <span data-ttu-id="a405d-213">İkinci kısmi görünüm bir modelde ve `ViewData` kısmi görünüme geçer.</span><span class="sxs-lookup"><span data-stu-id="a405d-213">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="a405d-214">Oluşturucu aşırı yüklemesi, var olan `ViewData` sözlüğü korurken yeni `ViewData` bir sözlüğü geçirmek için kullanılır. `ViewDataDictionary`</span><span class="sxs-lookup"><span data-stu-id="a405d-214">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-20)]

<span data-ttu-id="a405d-215">*Pages/Shared/_AuthorPartialRP. cshtml* , *readrp. cshtml* işaretleme dosyası tarafından başvurulan ilk kısmi görünümüdür:</span><span class="sxs-lookup"><span data-stu-id="a405d-215">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="a405d-216">*Pages/ArticlesRP/_ArticleSectionRP. cshtml* , *readrp. cshtml* biçimlendirme dosyası tarafından başvurulan ikinci kısmi görünümüdür:</span><span class="sxs-lookup"><span data-stu-id="a405d-216">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="a405d-217">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a405d-217">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="a405d-218">Örnek uygulamada aşağıdaki biçimlendirme *görünümleri/makaleleri/Read. cshtml* görünümünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="a405d-218">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="a405d-219">Görünüm iki kısmi görünüm içerir.</span><span class="sxs-lookup"><span data-stu-id="a405d-219">The view contains two partial views.</span></span> <span data-ttu-id="a405d-220">İkinci kısmi görünüm bir modelde ve `ViewData` kısmi görünüme geçer.</span><span class="sxs-lookup"><span data-stu-id="a405d-220">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="a405d-221">Oluşturucu aşırı yüklemesi, var olan `ViewData` sözlüğü korurken yeni `ViewData` bir sözlüğü geçirmek için kullanılır. `ViewDataDictionary`</span><span class="sxs-lookup"><span data-stu-id="a405d-221">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-20)]

<span data-ttu-id="a405d-222">*Views/Shared/_AuthorPartial. cshtml* , *Read. cshtml* biçimlendirme dosyası tarafından başvurulan ilk kısmi görünümdür:</span><span class="sxs-lookup"><span data-stu-id="a405d-222">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="a405d-223">*Görünümler/makaleler/_ArticleSection. cshtml* , *Read. cshtml* biçimlendirme dosyası tarafından başvurulan ikinci kısmi görünümdür:</span><span class="sxs-lookup"><span data-stu-id="a405d-223">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="a405d-224">Çalışma zamanında, partiler, kendisini paylaşılan *_Layout. cshtml*içinde işlenen üst biçimlendirme dosyasının işlenmiş çıktısına işlenir.</span><span class="sxs-lookup"><span data-stu-id="a405d-224">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="a405d-225">İlk kısmi görünüm, makalenin adını ve yayımlama tarihini işler:</span><span class="sxs-lookup"><span data-stu-id="a405d-225">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="a405d-226">Abrayhelincoln</span><span class="sxs-lookup"><span data-stu-id="a405d-226">Abraham Lincoln</span></span>
>
> <span data-ttu-id="a405d-227">&lt;Paylaşılan kısmi görünüm dosyası yolundan&gt;bu kısmi görünüm.</span><span class="sxs-lookup"><span data-stu-id="a405d-227">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="a405d-228">11/19/1863 12:00:00</span><span class="sxs-lookup"><span data-stu-id="a405d-228">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="a405d-229">İkinci kısmi görünüm, makalenin bölümlerini işler:</span><span class="sxs-lookup"><span data-stu-id="a405d-229">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="a405d-230">Bölüm bir dizin: 0</span><span class="sxs-lookup"><span data-stu-id="a405d-230">Section One Index: 0</span></span>
>
> <span data-ttu-id="a405d-231">Dört puan ve yedi yıl önce...</span><span class="sxs-lookup"><span data-stu-id="a405d-231">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="a405d-232">Bölüm Iki Dizin: 1.</span><span class="sxs-lookup"><span data-stu-id="a405d-232">Section Two Index: 1</span></span>
>
> <span data-ttu-id="a405d-233">Artık harika bir hukuki War, test ediyor...</span><span class="sxs-lookup"><span data-stu-id="a405d-233">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="a405d-234">Bölüm üç Dizin: 2</span><span class="sxs-lookup"><span data-stu-id="a405d-234">Section Three Index: 2</span></span>
>
> <span data-ttu-id="a405d-235">Ancak, daha büyük bir fikir için ayıramıyoruz...</span><span class="sxs-lookup"><span data-stu-id="a405d-235">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a405d-236">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a405d-236">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
