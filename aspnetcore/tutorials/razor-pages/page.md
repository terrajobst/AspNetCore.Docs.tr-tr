---
title: ASP.NET core'da iskeleli Razor sayfaları
author: rick-anderson
description: Yapı iskelesi tarafından oluşturulan Razor sayfaları açıklar.
ms.author: riande
ms.date: 04/06/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 2b8b9cde5a37a0754ca177cfc80163e2ffd2925b
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468795"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="39952-103">ASP.NET core'da iskeleli Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="39952-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="39952-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="39952-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="39952-105">Bu öğreticide, önceki öğreticide yapı iskelesi oluşturulmuş Razor sayfaları inceler.</span><span class="sxs-lookup"><span data-stu-id="39952-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="39952-106">[Görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) örnek.</span><span class="sxs-lookup"><span data-stu-id="39952-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="39952-107">Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları</span><span class="sxs-lookup"><span data-stu-id="39952-107">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="39952-108">İnceleme *Pages/Movies/Index.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="39952-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="39952-109">Razor sayfaları türetilir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="39952-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="39952-110">Kural olarak, `PageModel`-türetilmiş sınıf adlı `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="39952-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="39952-111">Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) eklemek için `RazorPagesMovieContext` sayfası.</span><span class="sxs-lookup"><span data-stu-id="39952-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="39952-112">Bu düzen iskele kurulmuş tüm sayfaları izleyin.</span><span class="sxs-lookup"><span data-stu-id="39952-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="39952-113">Bkz: [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) Entity Framework ile zaman uyumsuz programlamayı hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="39952-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="39952-114">Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi için Razor sayfası filmler listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="39952-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> `OnGetAsync` <span data-ttu-id="39952-115">veya `OnGet` durumunu başlatmak için bir Razor sayfası adı verilir.</span><span class="sxs-lookup"><span data-stu-id="39952-115">or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="39952-116">Bu durumda, `OnGetAsync` filmler listesini alır ve görüntüler.</span><span class="sxs-lookup"><span data-stu-id="39952-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="39952-117">Zaman `OnGet` döndürür `void` veya `OnGetAsync` döndürür`Task`, dönüş yöntem kullanılır.</span><span class="sxs-lookup"><span data-stu-id="39952-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="39952-118">Dönüş türü olduğunda `IActionResult` veya `Task<IActionResult>`, bir return deyimi sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="39952-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="39952-119">Örneğin, *Pages/Movies/Create.cshtml.cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="39952-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="39952-120">İnceleme *Pages/Movies/Index.cshtml* Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="39952-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="39952-121">Razor HTML, C# veya Razor özgü biçimlendirme geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="39952-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="39952-122">Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)Razor özgü biçimlendirme içinde geçiş, aksi takdirde, C# diline geçer.</span><span class="sxs-lookup"><span data-stu-id="39952-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="39952-123">`@page` Razor yönergesi, istek işleyebileceği anlamına gelir. bir MVC eyleme dosya yapar.</span><span class="sxs-lookup"><span data-stu-id="39952-123">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> `@page` <span data-ttu-id="39952-124">ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="39952-124">must be the first Razor directive on a page.</span></span> `@page` <span data-ttu-id="39952-125">Razor özgü biçimlendirme içinde geçiş bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="39952-125">is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="39952-126">Bkz: [Razor sözdizimi](xref:mvc/views/razor#razor-syntax) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="39952-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="39952-127">Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="39952-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="39952-128">`DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği.</span><span class="sxs-lookup"><span data-stu-id="39952-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="39952-129">Lambda ifadesi değerlendirilir inceledi yerine.</span><span class="sxs-lookup"><span data-stu-id="39952-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="39952-130">Hiçbir erişim ihlali var. anlamına olduğunda `model`, `model.Movie`, veya `model.Movie[0]` olan `null` veya boş.</span><span class="sxs-lookup"><span data-stu-id="39952-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="39952-131">Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="39952-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="39952-132">@model Yönergesi</span><span class="sxs-lookup"><span data-stu-id="39952-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="39952-133">`@model` Yönergesi için Razor sayfası geçirilen modelin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="39952-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="39952-134">Önceki örnekte `@model` satır yapar `PageModel`-türetilmiş sınıf için bir Razor sayfası kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="39952-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="39952-135">Model kullanılır `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML Yardımcıları](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında.</span><span class="sxs-lookup"><span data-stu-id="39952-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="39952-136">Düzen Sayfası</span><span class="sxs-lookup"><span data-stu-id="39952-136">The layout page</span></span>

<span data-ttu-id="39952-137">Menü bağlantıyı seçin (**RazorPagesMovie**, **giriş**, ve **gizlilik**).</span><span class="sxs-lookup"><span data-stu-id="39952-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="39952-138">Her sayfada aynı menüsü düzeni gösterilir.</span><span class="sxs-lookup"><span data-stu-id="39952-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="39952-139">Menüsü düzeni uygulanan *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="39952-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="39952-140">Açık *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="39952-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="39952-141">[Düzen](xref:mvc/views/layout) , tek bir yerde sitenizin HTML kapsayıcı düzenini belirtin ve ardından sitenizdeki birden çok sayfada uygulamak şablonlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="39952-141">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="39952-142">Bulma `@RenderBody()` satır.</span><span class="sxs-lookup"><span data-stu-id="39952-142">Find the `@RenderBody()` line.</span></span> `RenderBody` <span data-ttu-id="39952-143">olan bir yer tutucu burada tüm sayfaya özgü görünümler oluşturma Göster, *sarmalanmış* Düzen sayfasında.</span><span class="sxs-lookup"><span data-stu-id="39952-143">is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="39952-144">Örneğin, **gizlilik** bağlantı **Pages/Privacy.cshtml** görünümü içinde işlenir `RenderBody` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="39952-144">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="39952-145">ViewData ve düzeni</span><span class="sxs-lookup"><span data-stu-id="39952-145">ViewData and layout</span></span>

<span data-ttu-id="39952-146">Aşağıdaki kodu düşünün *Pages/Movies/Index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="39952-146">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="39952-147">Önceki vurgulanmış kodu, C# koduna geçiş Razor örneğidir.</span><span class="sxs-lookup"><span data-stu-id="39952-147">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="39952-148">`{` Ve `}` karakter C# kod bloğunun içine alın.</span><span class="sxs-lookup"><span data-stu-id="39952-148">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="39952-149">`PageModel` Temel sınıfa sahip değil bir `ViewData` bir görünüme iletmek istediğiniz veri eklemek için kullanılan bir sözlük özelliği.</span><span class="sxs-lookup"><span data-stu-id="39952-149">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="39952-150">Nesneleri eklemek `ViewData` bir anahtar/değer deseni kullanılarak sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="39952-150">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="39952-151">Yukarıdaki örnekte, "Title" özelliğini eklenen `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="39952-151">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="39952-152">"Title" özelliğini kullanılan *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="39952-152">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="39952-153">İlk birkaç satırı aşağıdaki biçimlendirme gösterir *_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="39952-153">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="39952-154">Satır `@*Markup removed for brevity.*@` Düzen dosyanızda görünmez bir Razor açıklama.</span><span class="sxs-lookup"><span data-stu-id="39952-154">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="39952-155">HTML Yorumlarını aksine (`<!-- -->`), Razor açıklama istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="39952-155">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="39952-156">Düzeni güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="39952-156">Update the layout</span></span>

<span data-ttu-id="39952-157">Değişiklik `<title>` öğesinde *Pages/Shared/_Layout.cshtml* dosyayı görüntülemek için **film** yerine **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="39952-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="39952-158">Aşağıdaki bağlantı öğe Bul *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="39952-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="39952-159">Önceki öğeyle aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="39952-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="39952-160">Önceki yer işareti öğesi bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="39952-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="39952-161">Bu durumda sahip [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="39952-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="39952-162">`asp-page="/Movies/Index"` Etiketi yardımcı öznitelik ve değer oluşturan bir bağlantı `/Movies/Index` Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="39952-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="39952-163">`asp-area` Öznitelik değeri olduğundan boş alanı bağlantıdaki kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="39952-163">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="39952-164">Bkz: [alanları](xref:mvc/controllers/areas) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="39952-164">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="39952-165">Değişikliklerinizi kaydedip tıklayarak uygulamayı test etme **RpMovie** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="39952-165">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="39952-166">Bkz: [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) herhangi bir sorun varsa GitHub dosya.</span><span class="sxs-lookup"><span data-stu-id="39952-166">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="39952-167">Test diğer bağlantılardan (**giriş**, **RpMovie**, **Oluştur**, **Düzenle**, ve **Sil**).</span><span class="sxs-lookup"><span data-stu-id="39952-167">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="39952-168">Her sayfada başlık, tarayıcı sekmesinde görebilirsiniz ayarlar. Bir sayfaya yer işareti başlık yer işareti için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="39952-168">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="39952-169">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="39952-169">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="39952-170">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel ayarlar için (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="39952-170">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="39952-171">Bu [GitHub sorunu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) ondalık virgülle ekleme hakkında yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="39952-171">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="39952-172">`Layout` Özelliği ayarlandığında *Pages/_ViewStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="39952-172">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="39952-173">Düzen dosyası önceki biçimlendirme ayarlar *Pages/Shared/_Layout.cshtml* altındaki tüm Razor dosyaları için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="39952-173">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="39952-174">Bkz: [Düzen](xref:razor-pages/index#layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="39952-174">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="39952-175">Oluşturma sayfa modeli</span><span class="sxs-lookup"><span data-stu-id="39952-175">The Create page model</span></span>

<span data-ttu-id="39952-176">İnceleme *Pages/Movies/Create.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="39952-176">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="39952-177">`OnGet` Yöntemi sayfa için gerekli herhangi bir durum başlatır.</span><span class="sxs-lookup"><span data-stu-id="39952-177">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="39952-178">Oluştur sayfasında başlatmak için bunu herhangi bir duruma sahip olmayan `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="39952-178">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="39952-179">Daha sonra öğreticide gördüğünüz `OnGet` durumu Initialize yöntemi.</span><span class="sxs-lookup"><span data-stu-id="39952-179">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="39952-180">`Page` Yöntemi oluşturur bir `PageResult` işleyen nesnesi *Create.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="39952-180">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="39952-181">`Movie` Özelliği kullanan `[BindProperty]` kabul etmek için için öznitelik [model bağlama](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="39952-181">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="39952-182">Form oluştur form değerleri gönderdiğinde, ASP.NET Core çalışma zamanı gönderilen değerine bağlar `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="39952-182">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="39952-183">`OnPostAsync` Sayfasının form verileri gönderildiğinde yöntemi çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="39952-183">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="39952-184">Model hataları varsa, form, gönderilen tüm form verileri ile birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="39952-184">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="39952-185">Form gönderildiğinde önce çoğu model hataları üzerinde istemci tarafı yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="39952-185">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="39952-186">Model hatası örneği bir tarihe dönüştürülür tarih alanı için bir değer gönderiyor.</span><span class="sxs-lookup"><span data-stu-id="39952-186">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="39952-187">İstemci tarafı doğrulama ve model doğrulama öğreticinin ilerleyen bölümlerinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="39952-187">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="39952-188">Herhangi bir model hata varsa, verileri kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="39952-188">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="39952-189">Razor sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="39952-189">The Create Razor Page</span></span>

<span data-ttu-id="39952-190">İnceleme *Pages/Movies/Create.cshtml* Razor sayfası dosyası:</span><span class="sxs-lookup"><span data-stu-id="39952-190">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# [<a name="visual-studio"></a><span data-ttu-id="39952-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39952-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39952-192">Visual Studio görüntüler `<form method="post">` etiket Yardımcıları için kullanılan ayırıcı kalın yazı tipinde etiketi:</span><span class="sxs-lookup"><span data-stu-id="39952-192">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Create.cshtml sayfasının görünümünü VS17](page/_static/th.png)

# [<a name="visual-studio-code"></a><span data-ttu-id="39952-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39952-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="39952-195">Etiket yardımcıları gibi daha fazla bilgi için `<form method="post">`, bkz: [etiket Yardımcıları ASP.NET core'da](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="39952-195">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# [<a name="visual-studio-for-mac"></a><span data-ttu-id="39952-196">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39952-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="39952-197">Visual Studio Mac görüntüler için `<form method="post">` etiket Yardımcıları için kullanılan ayırıcı kalın yazı tipinde etiketi.</span><span class="sxs-lookup"><span data-stu-id="39952-197">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="39952-198">`<form method="post">` Öğesi bir [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="39952-198">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="39952-199">Form etiketi Yardımcısı otomatik olarak içeren bir [antiforgery belirteci](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="39952-199">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="39952-200">Yapı iskelesi altyapısı Razor işaretlemesi için her bir alan (ID dışında) modelinde aşağıdakine benzer oluşturur:</span><span class="sxs-lookup"><span data-stu-id="39952-200">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="39952-201">[Doğrulama etiket Yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hataları görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="39952-201">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="39952-202">Doğrulama Bu seriyi ilerleyen bölümlerinde daha ayrıntılı ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="39952-202">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="39952-203">[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) etiketi başlığını oluşturur ve `for` özniteliğini `Title` özelliği.</span><span class="sxs-lookup"><span data-stu-id="39952-203">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="39952-204">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="39952-204">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39952-205">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="39952-205">Additional resources</span></span>

* [<span data-ttu-id="39952-206">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="39952-206">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="39952-207">[Önceki: Model ekleme](xref:tutorials/razor-pages/model)
> [sonraki: Veritabanı](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="39952-207">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>
