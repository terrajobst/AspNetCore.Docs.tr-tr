---
title: ASP.NET Core Razor Pages scafkatlama
author: rick-anderson
description: Yapı iskelesi tarafından oluşturulan Razor Pages açıklar.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 594fd6186cc73aa054fc9a1478850fa01e481ef2
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034194"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="63b12-103">ASP.NET Core Razor Pages scafkatlama</span><span class="sxs-lookup"><span data-stu-id="63b12-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="63b12-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="63b12-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63b12-105">Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="63b12-106">Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları</span><span class="sxs-lookup"><span data-stu-id="63b12-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="63b12-107">*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="63b12-108">Razor Pages `PageModel` ' dan türetilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="63b12-109">Kurala göre `PageModel`türetilmiş sınıf `<PageName>Model`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="63b12-110">Oluşturucu, sayfaya `RazorPagesMovieContext` eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="63b12-111">Tüm yapı iskelesi sayfaları bu düzene uyar.</span><span class="sxs-lookup"><span data-stu-id="63b12-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="63b12-112">Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .</span><span class="sxs-lookup"><span data-stu-id="63b12-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="63b12-113">Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="63b12-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="63b12-114">`OnGetAsync` veya `OnGet`, sayfanın durumunu başlatmak için çağırılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-114">`OnGetAsync` or `OnGet` is called to initialize the state of the page.</span></span> <span data-ttu-id="63b12-115">Bu durumda, `OnGetAsync` bir film listesi alır ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="63b12-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="63b12-116">`OnGet` `void` döndürdüğünde veya `OnGetAsync``Task`döndürürse, hiçbir dönüş açıklaması kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="63b12-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return statement is used.</span></span> <span data-ttu-id="63b12-117">Dönüş türü `IActionResult` veya `Task<IActionResult>` olduğunda, bir return ifadesinin sağlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="63b12-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="63b12-118">Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="63b12-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="63b12-119">*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="63b12-120">Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="63b12-121">Bir `@` sembol sonrasında [Razor ayrılmış bir anahtar sözcük](xref:mvc/views/razor#razor-reserved-keywords)olduğunda, bu, ' a geçiş yapar C#.</span><span class="sxs-lookup"><span data-stu-id="63b12-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

### <a name="the-page-directive"></a><span data-ttu-id="63b12-122">@page yönergesi</span><span class="sxs-lookup"><span data-stu-id="63b12-122">The @page directive</span></span>

<span data-ttu-id="63b12-123">`@page` Razor yönergesi, dosyayı bir MVC eylemi yapar, bu da istekleri işleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="63b12-123">The `@page` Razor directive makes the file an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="63b12-124">`@page` bir sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="63b12-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="63b12-125">`@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="63b12-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="63b12-126">Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="63b12-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="63b12-127">Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="63b12-128">`DisplayNameFor` HTML Yardımcısı, görünen adı belirlemede lambda ifadesinde başvurulan `Title` özelliğini inceler.</span><span class="sxs-lookup"><span data-stu-id="63b12-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="63b12-129">Lambda ifadesi değerlendirilmek yerine incelenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="63b12-130">Bu, `model`, `model.Movie`veya `model.Movie[0]` `null` veya boş olduğunda herhangi bir erişim ihlali olmadığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="63b12-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` is `null` or empty.</span></span> <span data-ttu-id="63b12-131">Lambda ifadesi değerlendirildiğinde (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="63b12-132">@model yönergesi</span><span class="sxs-lookup"><span data-stu-id="63b12-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="63b12-133">`@model` yönergesi, Razor sayfasına geçirilen modelin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="63b12-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="63b12-134">Yukarıdaki örnekte `@model` satırı, `PageModel`türetilmiş sınıfı Razor sayfası için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="63b12-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="63b12-135">Model, sayfada `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) kullanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="63b12-136">Düzen sayfası</span><span class="sxs-lookup"><span data-stu-id="63b12-136">The layout page</span></span>

<span data-ttu-id="63b12-137">Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin.</span><span class="sxs-lookup"><span data-stu-id="63b12-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="63b12-138">Her sayfada aynı menü düzeni gösterilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="63b12-139">Menü düzeni *Sayfalar/Shared/_Layout. cshtml* dosyasında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="63b12-140">*Pages/Shared/_Layout. cshtml* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="63b12-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="63b12-141">[Düzen](xref:mvc/views/layout) ŞABLONLARı, HTML kapsayıcı düzeninin şu şekilde olmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="63b12-141">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="63b12-142">Tek bir yerde belirtildi.</span><span class="sxs-lookup"><span data-stu-id="63b12-142">Specified in one place.</span></span>
* <span data-ttu-id="63b12-143">Sitede birden çok sayfada uygulandı.</span><span class="sxs-lookup"><span data-stu-id="63b12-143">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="63b12-144">`@RenderBody()` satırını bulun.</span><span class="sxs-lookup"><span data-stu-id="63b12-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="63b12-145">`RenderBody`, sayfaya özgü tüm görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur.</span><span class="sxs-lookup"><span data-stu-id="63b12-145">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="63b12-146">Örneğin, **Gizlilik** bağlantısını seçin ve *Sayfalar/gizlilik. cshtml* görünümü `RenderBody` yöntemi içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-146">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="63b12-147">ViewData ve Layout</span><span class="sxs-lookup"><span data-stu-id="63b12-147">ViewData and layout</span></span>

<span data-ttu-id="63b12-148">*Pages/filmler/Index. cshtml* dosyasından aşağıdaki biçimlendirmeyi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="63b12-148">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="63b12-149">Önceki vurgulanan biçimlendirme, Razor geçişi örneği olan bir örnektir C#.</span><span class="sxs-lookup"><span data-stu-id="63b12-149">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="63b12-150">`{` ve `}` karakterler bir C# kod bloğunu kapsar.</span><span class="sxs-lookup"><span data-stu-id="63b12-150">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="63b12-151">`PageModel` temel sınıfı, verileri bir görünüme geçirmek için kullanılabilen bir `ViewData` Dictionary özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="63b12-151">The `PageModel` base class contains a `ViewData` dictionary property that can be used to pass data to a View.</span></span> <span data-ttu-id="63b12-152">Nesneler, anahtar/değer örüntüsünün kullanıldığı `ViewData` sözlüğüne eklenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-152">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="63b12-153">Yukarıdaki örnekte, `"Title"` özelliği `ViewData` sözlüğüne eklenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-153">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="63b12-154">`"Title"` özelliği *Pages/Shared/_Layout. cshtml* dosyasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-154">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="63b12-155">Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.</span><span class="sxs-lookup"><span data-stu-id="63b12-155">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="63b12-156">Satır `@*Markup removed for brevity.*@` bir Razor açıklamadır.</span><span class="sxs-lookup"><span data-stu-id="63b12-156">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="63b12-157">HTML yorumlarının (`<!-- -->`) aksine Razor açıklamaları istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="63b12-157">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="63b12-158">Düzeni güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="63b12-158">Update the layout</span></span>

<span data-ttu-id="63b12-159">*Pages/Shared/_Layout. cshtml* dosyasındaki `<title>` öğesini **RazorPagesMovie**yerine **filmi** görüntüleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="63b12-159">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="63b12-160">*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.</span><span class="sxs-lookup"><span data-stu-id="63b12-160">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="63b12-161">Önceki öğeyi aşağıdaki biçimlendirmeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="63b12-161">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="63b12-162">Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="63b12-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="63b12-163">Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="63b12-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="63b12-164">`asp-page="/Movies/Index"` Tag Helper özniteliği ve değeri, `/Movies/Index` Razor sayfasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63b12-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="63b12-165">`asp-area` öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="63b12-165">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="63b12-166">Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.</span><span class="sxs-lookup"><span data-stu-id="63b12-166">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="63b12-167">Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="63b12-167">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="63b12-168">Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="63b12-168">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="63b12-169">Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**).</span><span class="sxs-lookup"><span data-stu-id="63b12-169">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="63b12-170">Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-170">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="63b12-171">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63b12-171">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="63b12-172">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="63b12-172">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="63b12-173">Ondalık virgülden ekleme hakkında yönergeler için bkz. [GitHub sorunu 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .</span><span class="sxs-lookup"><span data-stu-id="63b12-173">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="63b12-174">`Layout` özelliği *Pages/_ViewStart. cshtml* dosyasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="63b12-174">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="63b12-175">Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63b12-175">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="63b12-176">Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="63b12-176">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="63b12-177">Sayfa oluştur modeli</span><span class="sxs-lookup"><span data-stu-id="63b12-177">The Create page model</span></span>

<span data-ttu-id="63b12-178">*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-178">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="63b12-179">`OnGet` yöntemi, sayfa için gereken tüm durumları başlatır.</span><span class="sxs-lookup"><span data-stu-id="63b12-179">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="63b12-180">Oluşturma sayfasında, başlatılacak durum yok, bu nedenle `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="63b12-180">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="63b12-181">Öğreticide daha sonra, `OnGet` başlatma durumuna bir örnek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-181">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="63b12-182">`Page` yöntemi *Create. cshtml* sayfasını işleyen bir `PageResult` nesnesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63b12-182">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="63b12-183">`Movie` özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-183">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="63b12-184">Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, postalanan değerleri `Movie` modeline bağlar.</span><span class="sxs-lookup"><span data-stu-id="63b12-184">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="63b12-185">`OnPostAsync` yöntemi, sayfa form verileri gönderdiğinde çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="63b12-185">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="63b12-186">Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-186">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="63b12-187">Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-187">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="63b12-188">Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme.</span><span class="sxs-lookup"><span data-stu-id="63b12-188">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="63b12-189">İstemci tarafı doğrulama ve model doğrulaması Öğreticinin ilerleyen kısımlarında ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="63b12-189">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="63b12-190">Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-190">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="63b12-191">Razor Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="63b12-191">The Create Razor Page</span></span>

<span data-ttu-id="63b12-192">*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-192">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63b12-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63b12-193">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63b12-194">Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle aşağıdaki etiketleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="63b12-194">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Create. cshtml sayfasının VS17 görünümü](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63b12-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63b12-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="63b12-197">Aşağıdaki etiket yardımcıları, önceki biçimlendirmede gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="63b12-197">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63b12-198">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63b12-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="63b12-199">Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle aşağıdaki etiketleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="63b12-199">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="63b12-200">`<form method="post">` öğesi bir [form etiketi yardımcıdır](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="63b12-200">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="63b12-201">Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="63b12-201">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="63b12-202">Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="63b12-202">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="63b12-203">[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="63b12-203">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="63b12-204">Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="63b12-204">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="63b12-205">[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliği için etiket başlığını ve `for` özniteliğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63b12-205">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="63b12-206">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="63b12-206">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="63b12-207">`<form method="post">`gibi etiket yardımcıları hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="63b12-207">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63b12-208">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="63b12-208">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63b12-209">[Önceki: model ekleme](xref:tutorials/razor-pages/model)
> [Sonraki: veritabanı](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="63b12-209">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="63b12-210">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="63b12-210">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63b12-211">Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-211">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="63b12-212">Örneği [görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) .</span><span class="sxs-lookup"><span data-stu-id="63b12-212">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="63b12-213">Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları</span><span class="sxs-lookup"><span data-stu-id="63b12-213">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="63b12-214">*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-214">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="63b12-215">Razor Pages `PageModel` ' dan türetilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-215">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="63b12-216">Kurala göre `PageModel`türetilmiş sınıf `<PageName>Model`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-216">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="63b12-217">Oluşturucu, sayfaya `RazorPagesMovieContext` eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-217">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="63b12-218">Tüm yapı iskelesi sayfaları bu düzene uyar.</span><span class="sxs-lookup"><span data-stu-id="63b12-218">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="63b12-219">Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .</span><span class="sxs-lookup"><span data-stu-id="63b12-219">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="63b12-220">Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="63b12-220">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="63b12-221">`OnGetAsync` veya `OnGet` bir Razor sayfasında, sayfanın durumunu başlatmak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-221">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="63b12-222">Bu durumda, `OnGetAsync` bir film listesi alır ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="63b12-222">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="63b12-223">`OnGet` `void` döndürdüğünde veya `OnGetAsync``Task`döndürürse, hiçbir dönüş yöntemi kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="63b12-223">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="63b12-224">Dönüş türü `IActionResult` veya `Task<IActionResult>` olduğunda, bir return ifadesinin sağlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="63b12-224">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="63b12-225">Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="63b12-225">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="63b12-226">*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-226">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="63b12-227">Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-227">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="63b12-228">Bir `@` sembol sonrasında [Razor ayrılmış bir anahtar sözcük](xref:mvc/views/razor#razor-reserved-keywords)olduğunda, bu, ' a geçiş yapar C#.</span><span class="sxs-lookup"><span data-stu-id="63b12-228">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="63b12-229">`@page` Razor yönergesi, dosyayı bir MVC eylemine dönüştürür, bu da istekleri işleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="63b12-229">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="63b12-230">`@page` bir sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="63b12-230">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="63b12-231">`@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="63b12-231">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="63b12-232">Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="63b12-232">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="63b12-233">Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-233">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="63b12-234">`DisplayNameFor` HTML Yardımcısı, görünen adı belirlemede lambda ifadesinde başvurulan `Title` özelliğini inceler.</span><span class="sxs-lookup"><span data-stu-id="63b12-234">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="63b12-235">Lambda ifadesi değerlendirilmek yerine incelenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-235">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="63b12-236">Bu, `model`, `model.Movie`veya `model.Movie[0]` `null` veya boş olduğunda herhangi bir erişim ihlali olmadığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="63b12-236">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="63b12-237">Lambda ifadesi değerlendirildiğinde (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-237">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="63b12-238">@model yönergesi</span><span class="sxs-lookup"><span data-stu-id="63b12-238">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="63b12-239">`@model` yönergesi, Razor sayfasına geçirilen modelin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="63b12-239">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="63b12-240">Yukarıdaki örnekte `@model` satırı, `PageModel`türetilmiş sınıfı Razor sayfası için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="63b12-240">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="63b12-241">Model, sayfada `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) kullanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-241">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="63b12-242">Düzen sayfası</span><span class="sxs-lookup"><span data-stu-id="63b12-242">The layout page</span></span>

<span data-ttu-id="63b12-243">Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin.</span><span class="sxs-lookup"><span data-stu-id="63b12-243">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="63b12-244">Her sayfada aynı menü düzeni gösterilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-244">Each page shows the same menu layout.</span></span> <span data-ttu-id="63b12-245">Menü düzeni *Sayfalar/Shared/_Layout. cshtml* dosyasında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-245">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="63b12-246">*Pages/Shared/_Layout. cshtml* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="63b12-246">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="63b12-247">[Düzen](xref:mvc/views/layout) şablonları, sitenizin HTML kapsayıcı yerleşimini tek bir yerde belirtmenize ve sonra sitenizdeki birden çok sayfaya uygulamanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-247">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="63b12-248">`@RenderBody()` satırını bulun.</span><span class="sxs-lookup"><span data-stu-id="63b12-248">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="63b12-249">`RenderBody`, oluşturduğunuz tüm sayfaya özgü görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur.</span><span class="sxs-lookup"><span data-stu-id="63b12-249">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="63b12-250">Örneğin, **Gizlilik** bağlantısını seçerseniz, **Sayfalar/gizlilik. cshtml** görünümü `RenderBody` yöntemi içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-250">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="63b12-251">ViewData ve Layout</span><span class="sxs-lookup"><span data-stu-id="63b12-251">ViewData and layout</span></span>

<span data-ttu-id="63b12-252">*Pages/filmler/Index. cshtml* dosyasından aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="63b12-252">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="63b12-253">Önceki vurgulanan kod, Razor geçişi örneği olan bir örnektir C#.</span><span class="sxs-lookup"><span data-stu-id="63b12-253">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="63b12-254">`{` ve `}` karakterler bir C# kod bloğunu kapsar.</span><span class="sxs-lookup"><span data-stu-id="63b12-254">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="63b12-255">`PageModel` temel sınıfında, bir görünüme geçirmek istediğiniz verileri eklemek için kullanılabilecek bir `ViewData` Dictionary özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="63b12-255">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="63b12-256">Bir anahtar/değer düzeniyle `ViewData` sözlüğüne nesneler eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="63b12-256">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="63b12-257">Yukarıdaki örnekte, "title" özelliği `ViewData` sözlüğüne eklenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-257">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="63b12-258">"Title" özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-258">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="63b12-259">Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.</span><span class="sxs-lookup"><span data-stu-id="63b12-259">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="63b12-260">Satır `@*Markup removed for brevity.*@`, düzen dosyanızda görünmeyen bir Razor açıklamadır.</span><span class="sxs-lookup"><span data-stu-id="63b12-260">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="63b12-261">HTML yorumlarının (`<!-- -->`) aksine Razor açıklamaları istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="63b12-261">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="63b12-262">Düzeni güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="63b12-262">Update the layout</span></span>

<span data-ttu-id="63b12-263">*Pages/Shared/_Layout. cshtml* dosyasındaki `<title>` öğesini **RazorPagesMovie**yerine **filmi** görüntüleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="63b12-263">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="63b12-264">*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.</span><span class="sxs-lookup"><span data-stu-id="63b12-264">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="63b12-265">Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="63b12-265">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="63b12-266">Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="63b12-266">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="63b12-267">Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="63b12-267">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="63b12-268">`asp-page="/Movies/Index"` Tag Helper özniteliği ve değeri, `/Movies/Index` Razor sayfasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63b12-268">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="63b12-269">`asp-area` öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="63b12-269">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="63b12-270">Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.</span><span class="sxs-lookup"><span data-stu-id="63b12-270">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="63b12-271">Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="63b12-271">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="63b12-272">Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="63b12-272">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="63b12-273">Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**).</span><span class="sxs-lookup"><span data-stu-id="63b12-273">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="63b12-274">Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63b12-274">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="63b12-275">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63b12-275">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="63b12-276">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="63b12-276">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="63b12-277">Bu GitHub, ondalık virgülden ekleme hakkında yönergeler için [4076 sorun](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .</span><span class="sxs-lookup"><span data-stu-id="63b12-277">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="63b12-278">`Layout` özelliği *Pages/_ViewStart. cshtml* dosyasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="63b12-278">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="63b12-279">Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63b12-279">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="63b12-280">Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="63b12-280">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="63b12-281">Sayfa oluştur modeli</span><span class="sxs-lookup"><span data-stu-id="63b12-281">The Create page model</span></span>

<span data-ttu-id="63b12-282">*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-282">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="63b12-283">`OnGet` yöntemi, sayfa için gereken tüm durumları başlatır.</span><span class="sxs-lookup"><span data-stu-id="63b12-283">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="63b12-284">Oluşturma sayfasında, başlatılacak durum yok, bu nedenle `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="63b12-284">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="63b12-285">Öğreticide daha sonra `OnGet` yöntemi başlatma durumunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="63b12-285">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="63b12-286">`Page` yöntemi *Create. cshtml* sayfasını işleyen bir `PageResult` nesnesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63b12-286">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="63b12-287">`Movie` özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="63b12-287">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="63b12-288">Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, postalanan değerleri `Movie` modeline bağlar.</span><span class="sxs-lookup"><span data-stu-id="63b12-288">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="63b12-289">`OnPostAsync` yöntemi, sayfa form verileri gönderdiğinde çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="63b12-289">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="63b12-290">Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="63b12-290">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="63b12-291">Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-291">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="63b12-292">Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme.</span><span class="sxs-lookup"><span data-stu-id="63b12-292">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="63b12-293">İstemci tarafı doğrulama ve model doğrulaması Öğreticinin ilerleyen kısımlarında ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="63b12-293">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="63b12-294">Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63b12-294">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="63b12-295">Razor Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="63b12-295">The Create Razor Page</span></span>

<span data-ttu-id="63b12-296">*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="63b12-296">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63b12-297">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63b12-297">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63b12-298">Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle `<form method="post">` etiketini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="63b12-298">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Create. cshtml sayfasının VS17 görünümü](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63b12-300">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63b12-300">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="63b12-301">`<form method="post">`gibi etiket yardımcıları hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="63b12-301">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63b12-302">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63b12-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="63b12-303">Mac için Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipinde `<form method="post">` etiketini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="63b12-303">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="63b12-304">`<form method="post">` öğesi bir [form etiketi yardımcıdır](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="63b12-304">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="63b12-305">Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="63b12-305">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="63b12-306">Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="63b12-306">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="63b12-307">[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="63b12-307">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="63b12-308">Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="63b12-308">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="63b12-309">[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliği için etiket başlığını ve `for` özniteliğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63b12-309">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="63b12-310">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="63b12-310">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63b12-311">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="63b12-311">Additional resources</span></span>

* [<span data-ttu-id="63b12-312">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="63b12-312">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="63b12-313">[Önceki: model ekleme](xref:tutorials/razor-pages/model)
> [Sonraki: veritabanı](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="63b12-313">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
