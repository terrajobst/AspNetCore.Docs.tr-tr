---
title: ASP.NET Core Razor Pages scafkatlama
author: rick-anderson
description: Yapı iskelesi tarafından oluşturulan Razor Pages açıklar.
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 741ee4291cacbb1de0f8341673c8fd6ef0c9a462
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371837"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="a5a19-103">ASP.NET Core Razor Pages scafkatlama</span><span class="sxs-lookup"><span data-stu-id="a5a19-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a5a19-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5a19-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5a19-105">Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="a5a19-106">Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları</span><span class="sxs-lookup"><span data-stu-id="a5a19-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="a5a19-107">*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="a5a19-108">Razor Pages, öğesinden `PageModel`türetilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="a5a19-109">Kural gereği, `PageModel`-türetilmiş sınıfı çağrılır `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="a5a19-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="a5a19-110">Oluşturucu, `RazorPagesMovieContext` sayfasına eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="a5a19-111">Tüm yapı iskelesi sayfaları bu düzene uyar.</span><span class="sxs-lookup"><span data-stu-id="a5a19-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="a5a19-112">Entity Framework ile zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="a5a19-113">Sayfa için bir istek yapıldığında, `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="a5a19-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="a5a19-114">`OnGetAsync`ya `OnGet` da bir Razor sayfasında, sayfanın durumunu başlatmak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-114">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="a5a19-115">Bu durumda, `OnGetAsync` filmlerin bir listesini alır ve görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a5a19-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="a5a19-116">Döndürüldüğünde `OnGet` veyadöndüğünde`Task`,returnyöntemikullanılmaz. `OnGetAsync` `void`</span><span class="sxs-lookup"><span data-stu-id="a5a19-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="a5a19-117">Dönüş türü `IActionResult` veya `Task<IActionResult>`olduğunda, bir return ifadesinin sağlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="a5a19-118">Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a5a19-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="a5a19-119">*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="a5a19-120">Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="a5a19-121">Bir `@` sembolden sonra [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)geldiğinde, Razor 'e özgü işaretlere geçiş yapar, aksi takdirde öğesine C#geçirir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="a5a19-122">`@page` Razor yönergesi, dosyayı bir MVC eylemine dönüştürür, bu da istekleri işleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-122">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="a5a19-123">`@page`sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-123">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="a5a19-124">`@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-124">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="a5a19-125">Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-125">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="a5a19-126">Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-126">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="a5a19-127">HTML Yardımcısı, görünen adı `Title` belirlemede lambda ifadesinde başvurulan özelliği inceler. `DisplayNameFor`</span><span class="sxs-lookup"><span data-stu-id="a5a19-127">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="a5a19-128">Lambda ifadesi değerlendirilmek yerine incelenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-128">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="a5a19-129">`model`Diğer bir deyişle, `model.Movie`,, veya `model.Movie[0]` `null` boş olduğunda erişim ihlali yoktur.</span><span class="sxs-lookup"><span data-stu-id="a5a19-129">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="a5a19-130">Lambda ifadesi değerlendirildiğinde (örneğin, ile `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-130">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="a5a19-131">@model Yönergesi</span><span class="sxs-lookup"><span data-stu-id="a5a19-131">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="a5a19-132">`@model` Yönerge, Razor sayfasına geçirilen modelin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-132">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="a5a19-133">Yukarıdaki örnekte, `@model` satır türetilen sınıfı Razor sayfası için `PageModel`kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-133">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="a5a19-134">Model, sayfadaki `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-134">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="a5a19-135">Düzen sayfası</span><span class="sxs-lookup"><span data-stu-id="a5a19-135">The layout page</span></span>

<span data-ttu-id="a5a19-136">Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin.</span><span class="sxs-lookup"><span data-stu-id="a5a19-136">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="a5a19-137">Her sayfada aynı menü düzeni gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-137">Each page shows the same menu layout.</span></span> <span data-ttu-id="a5a19-138">Menü düzeni *Sayfalar/Shared/_Layout. cshtml* dosyasında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-138">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="a5a19-139">*Pages/Shared/_Layout. cshtml* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="a5a19-139">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="a5a19-140">[Düzen](xref:mvc/views/layout) ŞABLONLARı, HTML kapsayıcı düzeninin şu şekilde olmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="a5a19-140">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="a5a19-141">Tek bir yerde belirtildi.</span><span class="sxs-lookup"><span data-stu-id="a5a19-141">Specified in one place.</span></span>
* <span data-ttu-id="a5a19-142">Sitede birden çok sayfada uygulandı.</span><span class="sxs-lookup"><span data-stu-id="a5a19-142">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="a5a19-143">`@RenderBody()` Satırı bulun.</span><span class="sxs-lookup"><span data-stu-id="a5a19-143">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="a5a19-144">`RenderBody`, sayfaya özgü tüm görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur.</span><span class="sxs-lookup"><span data-stu-id="a5a19-144">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="a5a19-145">Örneğin, **Gizlilik** bağlantısını seçin ve *Sayfalar/gizlilik. cshtml* görünümü `RenderBody` yöntemin içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-145">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="a5a19-146">ViewData ve Layout</span><span class="sxs-lookup"><span data-stu-id="a5a19-146">ViewData and layout</span></span>

<span data-ttu-id="a5a19-147">*Pages/filmler/Index. cshtml* dosyasından aşağıdaki biçimlendirmeyi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="a5a19-147">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="a5a19-148">Önceki vurgulanan biçimlendirme, Razor geçişi örneği olan bir örnektir C#.</span><span class="sxs-lookup"><span data-stu-id="a5a19-148">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="a5a19-149">Ve karakterleri bir C# `{` `}`</span><span class="sxs-lookup"><span data-stu-id="a5a19-149">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="a5a19-150">Temel `PageModel` sınıf, verileri eklemek `ViewData` ve bir görünüme geçirmek için kullanılabilen bir Dictionary özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-150">The `PageModel` base class contains a `ViewData` dictionary property that can be used to add data that and pass it to a View.</span></span> <span data-ttu-id="a5a19-151">Nesneler, `ViewData` anahtara/değer düzeniyle kullanılarak sözlüğe eklenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-151">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="a5a19-152">Yukarıdaki örnekte, `"Title"` özelliği `ViewData` sözlüğe eklenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-152">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="a5a19-153">Özelliği Pages */Shared/_Layout. cshtml* dosyasında kullanılır. `"Title"`</span><span class="sxs-lookup"><span data-stu-id="a5a19-153">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="a5a19-154">Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-154">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="a5a19-155">Satır `@*Markup removed for brevity.*@` bir Razor açıklamadır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-155">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="a5a19-156">HTML yorumlarının (`<!-- -->`) aksine, Razor açıklamaları istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="a5a19-156">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="a5a19-157">Düzeni güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="a5a19-157">Update the layout</span></span>

<span data-ttu-id="a5a19-158">Pages/ *Shared/_Layout. cshtml* dosyasındaki   öğesiniRazorPagesMovieyerinefilmi`<title>` görüntüleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a5a19-158">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="a5a19-159">*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.</span><span class="sxs-lookup"><span data-stu-id="a5a19-159">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="a5a19-160">Önceki öğeyi aşağıdaki biçimlendirmeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-160">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="a5a19-161">Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="a5a19-161">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="a5a19-162">Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-162">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="a5a19-163">Etiket Yardımcısı özniteliği ve değeri `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. `asp-page="/Movies/Index"`</span><span class="sxs-lookup"><span data-stu-id="a5a19-163">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="a5a19-164">`asp-area` Öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="a5a19-164">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="a5a19-165">Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.</span><span class="sxs-lookup"><span data-stu-id="a5a19-165">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="a5a19-166">Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="a5a19-166">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="a5a19-167">Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="a5a19-167">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="a5a19-168">Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**).</span><span class="sxs-lookup"><span data-stu-id="a5a19-168">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="a5a19-169">Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-169">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="a5a19-170">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="a5a19-170">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a5a19-171">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-171">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="a5a19-172">Ondalık virgülden ekleme hakkında yönergeler için bkz. [GitHub sorunu 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-172">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="a5a19-173">Özelliği Pages */_viewstart. cshtml* dosyasında ayarlanır: `Layout`</span><span class="sxs-lookup"><span data-stu-id="a5a19-173">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="a5a19-174">Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a5a19-174">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="a5a19-175">Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-175">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="a5a19-176">Sayfa oluştur modeli</span><span class="sxs-lookup"><span data-stu-id="a5a19-176">The Create page model</span></span>

<span data-ttu-id="a5a19-177">*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-177">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="a5a19-178">`OnGet` Yöntemi, sayfa için gereken tüm durumları başlatır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-178">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="a5a19-179">Oluşturma sayfasında, başlatılacak durum yoktur, bu nedenle `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a5a19-179">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="a5a19-180">Öğreticide daha sonra, `OnGet` başlatma durumuna bir örnek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-180">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="a5a19-181">Yöntemi Create *. cshtml* sayfasını işleyen bir `PageResult` nesne oluşturur. `Page`</span><span class="sxs-lookup"><span data-stu-id="a5a19-181">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="a5a19-182">Özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. `Movie`</span><span class="sxs-lookup"><span data-stu-id="a5a19-182">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="a5a19-183">Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, gönderilen değerleri `Movie` modele bağlar.</span><span class="sxs-lookup"><span data-stu-id="a5a19-183">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="a5a19-184">Bu `OnPostAsync` Yöntem, sayfa form verileri göndertiğinde çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="a5a19-184">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="a5a19-185">Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-185">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="a5a19-186">Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-186">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="a5a19-187">Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme.</span><span class="sxs-lookup"><span data-stu-id="a5a19-187">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="a5a19-188">İstemci tarafı doğrulama ve model doğrulaması Öğreticinin ilerleyen kısımlarında ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-188">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="a5a19-189">Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-189">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="a5a19-190">Razor Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="a5a19-190">The Create Razor Page</span></span>

<span data-ttu-id="a5a19-191">*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-191">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a5a19-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5a19-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a5a19-193">Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle aşağıdaki etiketleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="a5a19-193">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Create. cshtml sayfasının VS17 görünümü](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a5a19-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a5a19-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a5a19-196">Aşağıdaki etiket yardımcıları, önceki biçimlendirmede gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a5a19-196">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a5a19-197">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5a19-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a5a19-198">Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle aşağıdaki etiketleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="a5a19-198">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="a5a19-199">Öğesi bir [form etiketi yardımcıdır.](xref:mvc/views/working-with-forms#the-form-tag-helper) `<form method="post">`</span><span class="sxs-lookup"><span data-stu-id="a5a19-199">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="a5a19-200">Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-200">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="a5a19-201">Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a5a19-201">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="a5a19-202">[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a5a19-202">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="a5a19-203">Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-203">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="a5a19-204">Etiket [etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliğin etiket açıklamalı alt yazısını `for` ve özniteliğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a5a19-204">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="a5a19-205">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-205">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="a5a19-206">Gibi etiket yardımcıları `<form method="post">`hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="a5a19-206">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5a19-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a5a19-207">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a5a19-208">[Öncekini Daha sonra model](xref:tutorials/razor-pages/model)
> ekleme[: Veritabanınızı](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="a5a19-208">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a5a19-209">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5a19-209">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5a19-210">Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-210">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="a5a19-211">[Görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) örnek.</span><span class="sxs-lookup"><span data-stu-id="a5a19-211">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="a5a19-212">Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları</span><span class="sxs-lookup"><span data-stu-id="a5a19-212">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="a5a19-213">*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-213">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="a5a19-214">Razor Pages, öğesinden `PageModel`türetilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-214">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="a5a19-215">Kural gereği, `PageModel`-türetilmiş sınıfı çağrılır `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="a5a19-215">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="a5a19-216">Oluşturucu, `RazorPagesMovieContext` sayfasına eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-216">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="a5a19-217">Tüm yapı iskelesi sayfaları bu düzene uyar.</span><span class="sxs-lookup"><span data-stu-id="a5a19-217">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="a5a19-218">Entity Framework ile zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-218">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="a5a19-219">Sayfa için bir istek yapıldığında, `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="a5a19-219">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="a5a19-220">`OnGetAsync`ya `OnGet` da bir Razor sayfasında, sayfanın durumunu başlatmak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-220">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="a5a19-221">Bu durumda, `OnGetAsync` filmlerin bir listesini alır ve görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a5a19-221">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="a5a19-222">Döndürüldüğünde `OnGet` veyadöndüğünde`Task`,returnyöntemikullanılmaz. `OnGetAsync` `void`</span><span class="sxs-lookup"><span data-stu-id="a5a19-222">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="a5a19-223">Dönüş türü `IActionResult` veya `Task<IActionResult>`olduğunda, bir return ifadesinin sağlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-223">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="a5a19-224">Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a5a19-224">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="a5a19-225">*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-225">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="a5a19-226">Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-226">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="a5a19-227">Bir `@` sembolden sonra [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)geldiğinde, Razor 'e özgü işaretlere geçiş yapar, aksi takdirde öğesine C#geçirir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-227">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="a5a19-228">`@page` Razor yönergesi, dosyayı bir MVC eylemine dönüştürür, bu da istekleri işleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-228">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="a5a19-229">`@page`sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-229">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="a5a19-230">`@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-230">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="a5a19-231">Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-231">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="a5a19-232">Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-232">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="a5a19-233">HTML Yardımcısı, görünen adı `Title` belirlemede lambda ifadesinde başvurulan özelliği inceler. `DisplayNameFor`</span><span class="sxs-lookup"><span data-stu-id="a5a19-233">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="a5a19-234">Lambda ifadesi değerlendirilmek yerine incelenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-234">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="a5a19-235">`model`Diğer bir deyişle, `model.Movie`,, veya `model.Movie[0]` `null` boş olduğunda erişim ihlali yoktur.</span><span class="sxs-lookup"><span data-stu-id="a5a19-235">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="a5a19-236">Lambda ifadesi değerlendirildiğinde (örneğin, ile `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-236">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="a5a19-237">@model Yönergesi</span><span class="sxs-lookup"><span data-stu-id="a5a19-237">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="a5a19-238">`@model` Yönerge, Razor sayfasına geçirilen modelin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-238">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="a5a19-239">Yukarıdaki örnekte, `@model` satır türetilen sınıfı Razor sayfası için `PageModel`kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-239">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="a5a19-240">Model, sayfadaki `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-240">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="a5a19-241">Düzen sayfası</span><span class="sxs-lookup"><span data-stu-id="a5a19-241">The layout page</span></span>

<span data-ttu-id="a5a19-242">Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin.</span><span class="sxs-lookup"><span data-stu-id="a5a19-242">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="a5a19-243">Her sayfada aynı menü düzeni gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-243">Each page shows the same menu layout.</span></span> <span data-ttu-id="a5a19-244">Menü düzeni *Sayfalar/Shared/_Layout. cshtml* dosyasında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-244">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="a5a19-245">*Pages/Shared/_Layout. cshtml* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="a5a19-245">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="a5a19-246">[Düzen](xref:mvc/views/layout) şablonları, sitenizin HTML kapsayıcı yerleşimini tek bir yerde belirtmenize ve sonra sitenizdeki birden çok sayfaya uygulamanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-246">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="a5a19-247">`@RenderBody()` Satırı bulun.</span><span class="sxs-lookup"><span data-stu-id="a5a19-247">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="a5a19-248">`RenderBody`, oluşturduğunuz tüm sayfaya özgü görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur.</span><span class="sxs-lookup"><span data-stu-id="a5a19-248">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="a5a19-249">Örneğin, **Gizlilik** bağlantısını seçerseniz, **Sayfa/Gizlilik. cshtml** görünümü `RenderBody` yöntemin içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-249">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="a5a19-250">ViewData ve Layout</span><span class="sxs-lookup"><span data-stu-id="a5a19-250">ViewData and layout</span></span>

<span data-ttu-id="a5a19-251">*Pages/filmler/Index. cshtml* dosyasından aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="a5a19-251">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="a5a19-252">Önceki vurgulanan kod, Razor geçişi örneği olan bir örnektir C#.</span><span class="sxs-lookup"><span data-stu-id="a5a19-252">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="a5a19-253">Ve karakterleri bir C# `{` `}`</span><span class="sxs-lookup"><span data-stu-id="a5a19-253">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="a5a19-254">Temel sınıfın, bir görünüme `ViewData` geçirmek istediğiniz verileri eklemek için kullanılabilecek bir Dictionary özelliği vardır. `PageModel`</span><span class="sxs-lookup"><span data-stu-id="a5a19-254">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="a5a19-255">Bir anahtar/değer örüntüsünün kullanıldığı `ViewData` sözlüğe nesneler eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="a5a19-255">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="a5a19-256">Yukarıdaki örnekte, "title" özelliği `ViewData` sözlüğe eklenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-256">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="a5a19-257">"Title" özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-257">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="a5a19-258">Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-258">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="a5a19-259">Satır `@*Markup removed for brevity.*@` , düzen dosyanızda görünmeyen bir Razor açıklamadır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-259">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="a5a19-260">HTML yorumlarının (`<!-- -->`) aksine, Razor açıklamaları istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="a5a19-260">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="a5a19-261">Düzeni güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="a5a19-261">Update the layout</span></span>

<span data-ttu-id="a5a19-262">Pages/ *Shared/_Layout. cshtml* dosyasındaki   öğesiniRazorPagesMovieyerinefilmi`<title>` görüntüleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a5a19-262">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="a5a19-263">*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.</span><span class="sxs-lookup"><span data-stu-id="a5a19-263">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="a5a19-264">Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a5a19-264">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="a5a19-265">Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="a5a19-265">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="a5a19-266">Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-266">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="a5a19-267">Etiket Yardımcısı özniteliği ve değeri `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. `asp-page="/Movies/Index"`</span><span class="sxs-lookup"><span data-stu-id="a5a19-267">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="a5a19-268">`asp-area` Öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="a5a19-268">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="a5a19-269">Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.</span><span class="sxs-lookup"><span data-stu-id="a5a19-269">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="a5a19-270">Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="a5a19-270">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="a5a19-271">Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="a5a19-271">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="a5a19-272">Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**).</span><span class="sxs-lookup"><span data-stu-id="a5a19-272">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="a5a19-273">Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-273">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="a5a19-274">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="a5a19-274">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a5a19-275">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-275">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="a5a19-276">Bu GitHub, ondalık virgülden ekleme hakkında yönergeler için [4076 sorun](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-276">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="a5a19-277">Özelliği Pages */_viewstart. cshtml* dosyasında ayarlanır: `Layout`</span><span class="sxs-lookup"><span data-stu-id="a5a19-277">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="a5a19-278">Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a5a19-278">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="a5a19-279">Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="a5a19-279">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="a5a19-280">Sayfa oluştur modeli</span><span class="sxs-lookup"><span data-stu-id="a5a19-280">The Create page model</span></span>

<span data-ttu-id="a5a19-281">*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-281">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="a5a19-282">`OnGet` Yöntemi, sayfa için gereken tüm durumları başlatır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-282">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="a5a19-283">Oluşturma sayfasında, başlatılacak durum yoktur, bu nedenle `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a5a19-283">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="a5a19-284">Öğreticide daha sonra Yöntem başlatma `OnGet` durumunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="a5a19-284">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="a5a19-285">Yöntemi Create *. cshtml* sayfasını işleyen bir `PageResult` nesne oluşturur. `Page`</span><span class="sxs-lookup"><span data-stu-id="a5a19-285">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="a5a19-286">Özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. `Movie`</span><span class="sxs-lookup"><span data-stu-id="a5a19-286">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="a5a19-287">Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, gönderilen değerleri `Movie` modele bağlar.</span><span class="sxs-lookup"><span data-stu-id="a5a19-287">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="a5a19-288">Bu `OnPostAsync` Yöntem, sayfa form verileri göndertiğinde çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="a5a19-288">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="a5a19-289">Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-289">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="a5a19-290">Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-290">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="a5a19-291">Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme.</span><span class="sxs-lookup"><span data-stu-id="a5a19-291">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="a5a19-292">İstemci tarafı doğrulama ve model doğrulaması Öğreticinin ilerleyen kısımlarında ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-292">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="a5a19-293">Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-293">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="a5a19-294">Razor Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="a5a19-294">The Create Razor Page</span></span>

<span data-ttu-id="a5a19-295">*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a5a19-295">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a5a19-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5a19-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a5a19-297">Visual Studio etiketi, `<form method="post">` etiket yardımcıları için kullanılan farklı bir kalın yazı tipiyle görüntüler:</span><span class="sxs-lookup"><span data-stu-id="a5a19-297">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Create. cshtml sayfasının VS17 görünümü](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a5a19-299">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a5a19-299">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a5a19-300">Gibi etiket yardımcıları `<form method="post">`hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="a5a19-300">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a5a19-301">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5a19-301">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a5a19-302">Mac için Visual Studio etiket yardımcıları `<form method="post">` için kullanılan farklı kalın yazı tipinde etiketi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a5a19-302">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="a5a19-303">Öğesi bir [form etiketi yardımcıdır.](xref:mvc/views/working-with-forms#the-form-tag-helper) `<form method="post">`</span><span class="sxs-lookup"><span data-stu-id="a5a19-303">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="a5a19-304">Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-304">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="a5a19-305">Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a5a19-305">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="a5a19-306">[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a5a19-306">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="a5a19-307">Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="a5a19-307">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="a5a19-308">Etiket [etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliğin etiket açıklamalı alt yazısını `for` ve özniteliğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a5a19-308">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="a5a19-309">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="a5a19-309">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5a19-310">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a5a19-310">Additional resources</span></span>

* [<span data-ttu-id="a5a19-311">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="a5a19-311">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="a5a19-312">[Öncekini Daha sonra model](xref:tutorials/razor-pages/model)
> ekleme[: Veritabanınızı](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="a5a19-312">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end