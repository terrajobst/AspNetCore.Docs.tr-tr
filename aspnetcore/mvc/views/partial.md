---
title: ASP.NET Core, kısmi görünümleri
author: ardalis
description: Kısmi görünümler büyük işaretleme dosyaları bölün ve ASP.NET Core uygulamaları, web sayfaları arasında ortak biçimlendirme çoğaltma azaltmak için nasıl kullanılacağını keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/partial
ms.openlocfilehash: 9564639dcff0cff7f21b123cec39f0c96ebda208
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824851"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="a34e0-103">ASP.NET Core, kısmi görünümleri</span><span class="sxs-lookup"><span data-stu-id="a34e0-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="a34e0-104">Tarafından [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="a34e0-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="a34e0-105">Kısmi bir görünümü bir [Razor](xref:mvc/views/razor) işaretleme dosyasının ( *.cshtml*) HTML çıkışı işleyen *içinde* başka bir işaretleme dosyasının çıkış işlenen.</span><span class="sxs-lookup"><span data-stu-id="a34e0-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a34e0-106">Terim *kısmi Görünüm* burada biçimlendirme dosyaları olarak da adlandırılır ya da bir MVC uygulaması geliştirilirken kullanılır *görünümleri*, veya biçimlendirme dosyalarında biçimlendirmeyi çağrılır burada bir Razor sayfaları uygulama *sayfaları*.</span><span class="sxs-lookup"><span data-stu-id="a34e0-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="a34e0-107">Bu konuda genel MVC görünümleri ve Razor sayfaları sayfaları olarak başvurduğu *biçimlendirme dosyalarında biçimlendirmeyi*.</span><span class="sxs-lookup"><span data-stu-id="a34e0-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="a34e0-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a34e0-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="a34e0-109">Kısmi görünümler kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="a34e0-109">When to use partial views</span></span>

<span data-ttu-id="a34e0-110">Kısmi görünümler için etkili bir yoludur:</span><span class="sxs-lookup"><span data-stu-id="a34e0-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="a34e0-111">Daha küçük bileşenlere büyük işaretleme dosyaları bölün.</span><span class="sxs-lookup"><span data-stu-id="a34e0-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="a34e0-112">Büyük, karmaşık işaretleme dosyasının mantıksal parçalarını oluşur, her parça bir kısmi görünüme yalıtılmış çalışma bir avantaj sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="a34e0-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="a34e0-113">Kodu biçimlendirme dosyasında yönetilebilir çünkü biçimlendirme yalnızca genel sayfa yapısı ve kısmi görünümler için başvurular içerir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="a34e0-114">Ortak biçimlendirme içeriğin biçimlendirme dosyalardaki azaltmak.</span><span class="sxs-lookup"><span data-stu-id="a34e0-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="a34e0-115">Biçimlendirme dosyalardaki aynı biçimlendirme öğeleri kullanıldığında biçimlendirme içerik çoğaltılması bir kısmi görünüm dosyasına kısmi görünüm kaldırır.</span><span class="sxs-lookup"><span data-stu-id="a34e0-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="a34e0-116">Kısmi görünüm biçimlendirme değiştirildiğinde, kısmi görünümü kullanmak biçimlendirme dosyaların işlenen çıkışı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="a34e0-117">Kısmi görünümler, ortak yerleşim öğelerinin korumak için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a34e0-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="a34e0-118">Ortak yerleşim öğeleri tanımlanmamalıdır [_Layout.cshtml](xref:mvc/views/layout) dosyaları.</span><span class="sxs-lookup"><span data-stu-id="a34e0-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="a34e0-119">Kısmi görünüm karmaşık işleme mantığı ya da kod yürütme biçimlendirmesi oluşturmak için gerekli olduğu kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a34e0-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="a34e0-120">Kısmi bir görünümü yerine kullanmak bir [görünümü bileşen](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="a34e0-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="a34e0-121">Kısmi görünümler bildirme</span><span class="sxs-lookup"><span data-stu-id="a34e0-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a34e0-122">Kısmi bir görünümü bir *.cshtml* işaretleme dosyasının tutulan içinde *görünümleri* klasörü (MVC) veya *sayfaları* klasörü (Razor sayfaları).</span><span class="sxs-lookup"><span data-stu-id="a34e0-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="a34e0-123">ASP.NET Core MVC, denetleyici 's, <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir görünüm veya kısmi görünüm döndürme özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="a34e0-124">Razor sayfaları içinde bir <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> döndürebilir bir <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-124">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="a34e0-125">Başvuru ve kısmi görünümleri işlemeye açıklanan [kısmi görünüm başvuru](#reference-a-partial-view) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a34e0-125">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="a34e0-126">MVC görünümü veya sayfa işleme aksine, kısmi görünüm çalıştırmaz *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a34e0-126">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="a34e0-127">Daha fazla bilgi için *_ViewStart.cshtml*, bkz: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-127">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="a34e0-128">Kısmi görünüm dosya adları, genellikle bir alt çizgiyle başlayan (`_`).</span><span class="sxs-lookup"><span data-stu-id="a34e0-128">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="a34e0-129">Bu adlandırma kuralı gerekmez, ancak kısmi görünümler görünümlere ve sayfalara görsel olarak ayırt etmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a34e0-129">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a34e0-130">Kısmi bir görünümü bir *.cshtml* işaretleme dosyasının tutulan içinde *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a34e0-130">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="a34e0-131">Bir denetleyicinin <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir görünüm veya kısmi görünüm döndürme özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-131">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="a34e0-132">MVC görünümü işleme aksine, kısmi görünüm çalıştırmaz *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a34e0-132">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="a34e0-133">Daha fazla bilgi için *_ViewStart.cshtml*, bkz: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-133">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="a34e0-134">Kısmi görünüm dosya adları, genellikle bir alt çizgiyle başlayan (`_`).</span><span class="sxs-lookup"><span data-stu-id="a34e0-134">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="a34e0-135">Bu adlandırma kuralı gerekmez, ancak kısmi görünümler görünümleri görsel olarak ayırt etmesine yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a34e0-135">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="a34e0-136">Kısmi görünüm başvurusu</span><span class="sxs-lookup"><span data-stu-id="a34e0-136">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a34e0-137">Bir işaretleme dosyasının içinde kısmi görünüm başvurmak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="a34e0-137">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="a34e0-138">Uygulamaları aşağıdaki zaman uyumsuz işleme yaklaşımlardan birini kullanmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="a34e0-138">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="a34e0-139">Kısmi Etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a34e0-139">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="a34e0-140">Zaman uyumsuz HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a34e0-140">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a34e0-141">Bir işaretleme dosyasının içinde kısmi görünüm başvurmak için iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="a34e0-141">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="a34e0-142">Zaman uyumsuz HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a34e0-142">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="a34e0-143">Zaman uyumlu HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a34e0-143">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="a34e0-144">Uygulamaları kullanmanızı öneririz [zaman uyumsuz HTML Yardımcısı](#asynchronous-html-helper).</span><span class="sxs-lookup"><span data-stu-id="a34e0-144">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="a34e0-145">Kısmi etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a34e0-145">Partial Tag Helper</span></span>

<span data-ttu-id="a34e0-146">[Kısmi etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) ASP.NET Core 2.1 veya üzerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-146">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="a34e0-147">Kısmi etiket Yardımcısı içeriği zaman uyumsuz olarak işler ve HTML benzeri bir sözdizimi kullanır:</span><span class="sxs-lookup"><span data-stu-id="a34e0-147">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="a34e0-148">Bir dosya uzantısı varsa, etiket Yardımcısı kısmi görünüm çağırma biçimlendirme dosyasıyla aynı klasörde olması gereken bir kısmi görünüm başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="a34e0-148">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="a34e0-149">Aşağıdaki örnek, uygulama kök kısmi görünüm başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="a34e0-149">The following example references a partial view from the app root.</span></span> <span data-ttu-id="a34e0-150">Bir tilde-eğik çizgi ile başlayan yollar (`~/`) veya eğik çizgi (`/`) uygulama kök dizinine bakın:</span><span class="sxs-lookup"><span data-stu-id="a34e0-150">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="a34e0-151">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a34e0-151">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="a34e0-152">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a34e0-152">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="a34e0-153">Aşağıdaki örnek, göreli bir yol ile kısmi görünüm başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="a34e0-153">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="a34e0-154">Daha fazla bilgi için bkz. <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-154">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="a34e0-155">Zaman uyumsuz HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a34e0-155">Asynchronous HTML Helper</span></span>

<span data-ttu-id="a34e0-156">Bir HTML Yardımcısı kullanırken en iyi kullanmaktır <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-156">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="a34e0-157">`PartialAsync` döndürür bir <xref:Microsoft.AspNetCore.Html.IHtmlContent> türü içinde kaydırılır bir <xref:System.Threading.Tasks.Task%601>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-157">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task%601>.</span></span> <span data-ttu-id="a34e0-158">Yöntem ile bekletilen çağrısı koyarak başvurulan bir `@` karakter:</span><span class="sxs-lookup"><span data-stu-id="a34e0-158">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="a34e0-159">Dosya uzantısı varsa, kısmi görünüm çağırma biçimlendirme dosyasıyla aynı klasörde olmalıdır kısmi bir görünümü HTML Yardımcısı başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="a34e0-159">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="a34e0-160">Aşağıdaki örnek, uygulama kök kısmi görünüm başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="a34e0-160">The following example references a partial view from the app root.</span></span> <span data-ttu-id="a34e0-161">Bir tilde-eğik çizgi ile başlayan yollar (`~/`) veya eğik çizgi (`/`) uygulama kök dizinine bakın:</span><span class="sxs-lookup"><span data-stu-id="a34e0-161">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a34e0-162">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a34e0-162">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="a34e0-163">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a34e0-163">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="a34e0-164">Aşağıdaki örnek, göreli bir yol ile kısmi görünüm başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="a34e0-164">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="a34e0-165">Alternatif olarak, kısmi bir görünümü ile oluşturulabilen <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-165">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="a34e0-166">Bu yöntem döndürmüyor bir <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="a34e0-166">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="a34e0-167">Bu yanıt doğrudan işlenmiş çıktı akışları.</span><span class="sxs-lookup"><span data-stu-id="a34e0-167">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="a34e0-168">Yöntem bir sonuç döndürmediğinden Razor kodu bloğu çağrılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="a34e0-168">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="a34e0-169">Bu yana `RenderPartialAsync` akışları işlenmiş içeriği, bazı senaryolarda daha iyi performans sağlar.</span><span class="sxs-lookup"><span data-stu-id="a34e0-169">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="a34e0-170">Performans açısından kritik durumlarda, her iki yaklaşım kullanarak sayfayı Kıyaslama ve daha hızlı bir yanıt oluşturan bir yaklaşım kullanın.</span><span class="sxs-lookup"><span data-stu-id="a34e0-170">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="a34e0-171">Zaman uyumlu HTML Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a34e0-171">Synchronous HTML Helper</span></span>

<span data-ttu-id="a34e0-172"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> ve <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> zaman uyumlu eşdeğerleri olan `PartialAsync` ve `RenderPartialAsync`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="a34e0-172"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="a34e0-173">Hangi kilitlenme senaryolar olduğundan, zaman uyumlu eşdeğerleri önerilmez.</span><span class="sxs-lookup"><span data-stu-id="a34e0-173">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="a34e0-174">Gelecek sürümlerde kaldırılması için zaman uyumlu metotları hedefler.</span><span class="sxs-lookup"><span data-stu-id="a34e0-174">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a34e0-175">Kodu çalıştırmak ihtiyacınız varsa, bir [görünümü bileşen](xref:mvc/views/view-components) yerine kısmi görünüm.</span><span class="sxs-lookup"><span data-stu-id="a34e0-175">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a34e0-176">Çağırma `Partial` veya `RenderPartial` sonuçları bir Visual Studio analyzer uyarı.</span><span class="sxs-lookup"><span data-stu-id="a34e0-176">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="a34e0-177">Örneğin, varlığını `Partial` aşağıdaki uyarı iletisini verir:</span><span class="sxs-lookup"><span data-stu-id="a34e0-177">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="a34e0-178">Uygulama kilitlenmeleri IHtmlHelper.Partial kullanımına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-178">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="a34e0-179">Kullanmayı &lt;kısmi&gt; etiketi Yardımcısı veya IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="a34e0-179">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="a34e0-180">Çağrıları değiştirin `@Html.Partial` ile `@await Html.PartialAsync` veya [kısmi etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a34e0-180">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="a34e0-181">Kısmi etiket Yardımcısı geçiş hakkında daha fazla bilgi için bkz. [HTML Yardımcısı'ten geçiş](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="a34e0-181">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="a34e0-182">Kısmi görünüm bulma</span><span class="sxs-lookup"><span data-stu-id="a34e0-182">Partial view discovery</span></span>

<span data-ttu-id="a34e0-183">Kısmi Görünüm adı olmayan bir dosya uzantısı tarafından başvurulduğunda belirtilen sırayla aşağıdaki konumlardan aranır:</span><span class="sxs-lookup"><span data-stu-id="a34e0-183">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a34e0-184">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a34e0-184">**Razor Pages**</span></span>

1. <span data-ttu-id="a34e0-185">Sayfanın klasörü şu anda yürütülüyor</span><span class="sxs-lookup"><span data-stu-id="a34e0-185">Currently executing page's folder</span></span>
1. <span data-ttu-id="a34e0-186">Dizin graph sayfanın klasörün üstünde</span><span class="sxs-lookup"><span data-stu-id="a34e0-186">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="a34e0-187">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a34e0-187">**MVC**</span></span>

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

<span data-ttu-id="a34e0-188">Kısmi görünüm bulma için aşağıdaki kurallar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="a34e0-188">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="a34e0-189">Aynı dosya adına sahip farklı kısmi görünümler, kısmi görünümleri farklı klasörlerde bulunan olduğunda izin verilir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-189">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="a34e0-190">Kısmi Görünüm adı olmayan bir dosya uzantısı ile kısmi görünüm tarafından başvuru olduğunda hem arayanın klasörde mevcut ve *paylaşılan* kısmi görünüm klasörü, arayanın klasöründeki kısmi görünüm sağlar.</span><span class="sxs-lookup"><span data-stu-id="a34e0-190">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="a34e0-191">Kısmi görünüm arayanın klasörde mevcut değilse, kısmi görünüm gelen sağlanır *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="a34e0-191">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="a34e0-192">Kısmi görünümler içinde *paylaşılan* klasör çağrılır *paylaşılan kısmi görünümler* veya *varsayılan kısmi görünümler*.</span><span class="sxs-lookup"><span data-stu-id="a34e0-192">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="a34e0-193">Kısmi görünümler olabilir *zincirleme*&mdash;döngüsel bir başvuru çağrıları'na göre biçimlendirilmiş değil, kısmi görünüm başka bir kısmi görünüm çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a34e0-193">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="a34e0-194">Göreli yolları her zaman kök veya dosyanın üst geçerli dosyanın göreli olur.</span><span class="sxs-lookup"><span data-stu-id="a34e0-194">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="a34e0-195">A [Razor](xref:mvc/views/razor) `section` tanımlanan bir kısmi görünüm üst işaretleme dosyaları için görünmez.</span><span class="sxs-lookup"><span data-stu-id="a34e0-195">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="a34e0-196">`section` Yalnızca tanımlanmış kısmi görünüm için görünür durumdadır.</span><span class="sxs-lookup"><span data-stu-id="a34e0-196">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="a34e0-197">Kısmi görünümler verilere erişmek</span><span class="sxs-lookup"><span data-stu-id="a34e0-197">Access data from partial views</span></span>

<span data-ttu-id="a34e0-198">Kısmi görünümün örneği oluşturulduğunda aldığı bir *kopyalama* üst öğenin'ın `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="a34e0-198">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="a34e0-199">Kısmi görünüm içindeki verilerde yapılan güncelleştirmeler üst görünümde kalıcı değildir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-199">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="a34e0-200">`ViewData` Kısmi görünüm döndürdüğünde kısmi görünüm değişiklikler kaybolur.</span><span class="sxs-lookup"><span data-stu-id="a34e0-200">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="a34e0-201">Aşağıdaki örnek bir örneğini geçirin gösterilmektedir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) kısmi görünüm için:</span><span class="sxs-lookup"><span data-stu-id="a34e0-201">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="a34e0-202">Kısmi görünüme, bir model geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a34e0-202">You can pass a model into a partial view.</span></span> <span data-ttu-id="a34e0-203">Modeli, özel bir nesne olabilir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-203">The model can be a custom object.</span></span> <span data-ttu-id="a34e0-204">Bir model ile geçirdiğiniz `PartialAsync` (içerik bloğu çağırana işler) veya `RenderPartialAsync` (çıkış içeriği akışları):</span><span class="sxs-lookup"><span data-stu-id="a34e0-204">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a34e0-205">**Razor Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="a34e0-205">**Razor Pages**</span></span>

<span data-ttu-id="a34e0-206">Örnek uygulama aşağıdaki biçimlendirmede dandır *Pages/ArticlesRP/ReadRP.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="a34e0-206">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="a34e0-207">İki kısmi görünüm sayfası içerir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-207">The page contains two partial views.</span></span> <span data-ttu-id="a34e0-208">Bir modeldeki ikinci kısmi görünüm geçirir ve `ViewData` kısmi görünüm için.</span><span class="sxs-lookup"><span data-stu-id="a34e0-208">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="a34e0-209">`ViewDataDictionary` Oluşturucu aşırı yüklemesi, yeni bir geçirmek için kullanılır `ViewData` varolan korurken sözlük `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="a34e0-209">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-20)]

<span data-ttu-id="a34e0-210">*Pages/Shared/_AuthorPartialRP.cshtml* tarafından başvurulan ilk kısmi Görünüm *ReadRP.cshtml* işaretleme dosyasının:</span><span class="sxs-lookup"><span data-stu-id="a34e0-210">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="a34e0-211">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* tarafından başvurulan ikinci kısmi Görünüm *ReadRP.cshtml* işaretleme dosyasının:</span><span class="sxs-lookup"><span data-stu-id="a34e0-211">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="a34e0-212">**MVC**</span><span class="sxs-lookup"><span data-stu-id="a34e0-212">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="a34e0-213">Aşağıdaki örnek uygulamanın gösterildiği biçimlendirmede *Views/Articles/Read.cshtml* görünümü.</span><span class="sxs-lookup"><span data-stu-id="a34e0-213">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="a34e0-214">İki kısmi görünümler görünümün içerir.</span><span class="sxs-lookup"><span data-stu-id="a34e0-214">The view contains two partial views.</span></span> <span data-ttu-id="a34e0-215">Bir modeldeki ikinci kısmi görünüm geçirir ve `ViewData` kısmi görünüm için.</span><span class="sxs-lookup"><span data-stu-id="a34e0-215">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="a34e0-216">`ViewDataDictionary` Oluşturucu aşırı yüklemesi, yeni bir geçirmek için kullanılır `ViewData` varolan korurken sözlük `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="a34e0-216">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-20)]

<span data-ttu-id="a34e0-217">*Views/Shared/_AuthorPartial.cshtml* tarafından başvurulan ilk kısmi Görünüm *ReadRP.cshtml* işaretleme dosyasının:</span><span class="sxs-lookup"><span data-stu-id="a34e0-217">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="a34e0-218">*Views/Articles/_ArticleSection.cshtml* tarafından başvurulan ikinci kısmi Görünüm *Read.cshtml* işaretleme dosyasının:</span><span class="sxs-lookup"><span data-stu-id="a34e0-218">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="a34e0-219">Kısmi çalışma zamanında işlendiğini işlenmiş çıkış üst işaretleme dosyasının kendisi işlenen paylaşılan içinde *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a34e0-219">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="a34e0-220">Bir makalede yazarın adı ve yayın tarihi ilk kısmi görünümü işler:</span><span class="sxs-lookup"><span data-stu-id="a34e0-220">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="a34e0-221">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="a34e0-221">Abraham Lincoln</span></span>
>
> <span data-ttu-id="a34e0-222">Kısmi bu görünümden &lt;paylaşılan kısmi görünüm dosya yolu&gt;.</span><span class="sxs-lookup"><span data-stu-id="a34e0-222">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="a34e0-223">19/11/1863 12:00:00: 00</span><span class="sxs-lookup"><span data-stu-id="a34e0-223">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="a34e0-224">Makalenin bölümleri ikinci kısmi görünümü işler:</span><span class="sxs-lookup"><span data-stu-id="a34e0-224">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="a34e0-225">Bir dizin bölümünde: 0</span><span class="sxs-lookup"><span data-stu-id="a34e0-225">Section One Index: 0</span></span>
>
> <span data-ttu-id="a34e0-226">Dört puanı ve yedi yıl önce...</span><span class="sxs-lookup"><span data-stu-id="a34e0-226">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="a34e0-227">İki bölüm dizini: 1.</span><span class="sxs-lookup"><span data-stu-id="a34e0-227">Section Two Index: 1</span></span>
>
> <span data-ttu-id="a34e0-228">Biz de harika bir inşaat war katılan artık test...</span><span class="sxs-lookup"><span data-stu-id="a34e0-228">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="a34e0-229">Üç bölüm dizini: 2</span><span class="sxs-lookup"><span data-stu-id="a34e0-229">Section Three Index: 2</span></span>
>
> <span data-ttu-id="a34e0-230">Ancak, daha büyük bir anlamda, biz değil ayırabilirsiniz...</span><span class="sxs-lookup"><span data-stu-id="a34e0-230">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a34e0-231">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a34e0-231">Additional resources</span></span>

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
