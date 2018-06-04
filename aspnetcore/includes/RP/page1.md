# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="c9a88-101">ASP.NET Core kurulmuş Razor sayfalarında</span><span class="sxs-lookup"><span data-stu-id="c9a88-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c9a88-102">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c9a88-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c9a88-103">Bu öğretici Razor önceki öğreticideki yapı iskelesi tarafından oluşturulan sayfaları inceler.</span><span class="sxs-lookup"><span data-stu-id="c9a88-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="c9a88-104">[Görüntülemek veya karşıdan](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) örnek.</span><span class="sxs-lookup"><span data-stu-id="c9a88-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="c9a88-105">Oluştur, Sil, Ayrıntılar ve düzenleme sayfaları.</span><span class="sxs-lookup"><span data-stu-id="c9a88-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="c9a88-106">İncelemek *Pages/Movies/Index.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="c9a88-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="c9a88-107">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="c9a88-107">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="c9a88-108">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="c9a88-108">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]</span></span>

::: moniker-end

<span data-ttu-id="c9a88-109">Razor sayfalarının türetilir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="c9a88-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="c9a88-110">Kural tarafından `PageModel`-türetilmiş sınıf çağrılır `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="c9a88-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="c9a88-111">Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) eklemek için `MovieContext` sayfası.</span><span class="sxs-lookup"><span data-stu-id="c9a88-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="c9a88-112">Bu yol kurulmuş tüm sayfaları izler.</span><span class="sxs-lookup"><span data-stu-id="c9a88-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="c9a88-113">Bkz: [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) Entity Framework zaman uyumsuz programing hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c9a88-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="c9a88-114">Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi Razor sayfasına filmler listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="c9a88-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="c9a88-115">`OnGetAsync` veya `OnGet` sayfası için durum başlatmak için bir Razor sayfasında olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="c9a88-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="c9a88-116">Bu durumda, `OnGetAsync` filmler listesini alır ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="c9a88-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="c9a88-117">Zaman `OnGet` döndürür `void` veya `OnGetAsync` döndürür`Task`, hiçbir dönüş yöntemi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9a88-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="c9a88-118">Dönüş türü olduğunda `IActionResult` veya `Task<IActionResult>`, bir dönüş ifadesi sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c9a88-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="c9a88-119">Örneğin, *Pages/Movies/Create.cshtml.cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c9a88-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="c9a88-120">İncelemek *Pages/Movies/Index.cshtml* Razor sayfasını:</span><span class="sxs-lookup"><span data-stu-id="c9a88-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="c9a88-121">HTML Razor C# veya Razor özgü biçimlendirme geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="c9a88-122">Zaman bir `@` simgesi tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords), Razor özgü biçimlendirme geçişleri, aksi takdirde C# diline geçiş.</span><span class="sxs-lookup"><span data-stu-id="c9a88-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="c9a88-123">`@page` Razor yönergesi yapar dosya MVC eyleme &mdash; istek işleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-123">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="c9a88-124">`@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c9a88-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="c9a88-125">`@page` Razor özgü biçimlendirme geçiş, bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="c9a88-126">Bkz: [Razor sözdizimi](xref:mvc/views/razor#razor-syntax) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c9a88-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="c9a88-127">Aşağıdaki HTML Yardımcısı kullanılan lambda ifadesi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="c9a88-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="c9a88-128">`DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adı belirlemek için lambda ifadesinde başvurulan özelliği.</span><span class="sxs-lookup"><span data-stu-id="c9a88-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="c9a88-129">Lambda ifadesi hesaplanan Denetlenmekte yerine.</span><span class="sxs-lookup"><span data-stu-id="c9a88-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="c9a88-130">Hiçbir erişim ihlali var. anlamına zaman `model`, `model.Movie`, veya `model.Movie[0]` olan `null` veya boş.</span><span class="sxs-lookup"><span data-stu-id="c9a88-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="c9a88-131">Lambda ifadesi ne zaman değerlendirildiği (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="c9a88-132">@model Yönergesi</span><span class="sxs-lookup"><span data-stu-id="c9a88-132">The @model directive</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="c9a88-133">`@model` Yönergesi Razor sayfasına iletilen model türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="c9a88-134">Önceki örnekte `@model` satır yapar `PageModel`-türetilmiş sınıf Razor sayfasına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="c9a88-135">Model kullanılan `@Html.DisplayNameFor` ve `@Html.DisplayName` [HTML Yardımcıları](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında.</span><span class="sxs-lookup"><span data-stu-id="c9a88-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="c9a88-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData ve düzeni</span><span class="sxs-lookup"><span data-stu-id="c9a88-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="c9a88-137">Aşağıdaki kod göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c9a88-137">Consider the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="c9a88-138">Önceki vurgulanmış kodu koddan C# diline Razor örneğidir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="c9a88-139">`{` Ve `}` karakterleri bir C# kod bloğunun içine alın.</span><span class="sxs-lookup"><span data-stu-id="c9a88-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="c9a88-140">`PageModel` Taban sınıfının bir `ViewData` bir görünüme iletmek istediğiniz verileri eklemek için kullanılan sözlüğü özelliği.</span><span class="sxs-lookup"><span data-stu-id="c9a88-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="c9a88-141">Nesneleri eklemek `ViewData` bir anahtar/değer modeli kullanarak sözlük.</span><span class="sxs-lookup"><span data-stu-id="c9a88-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="c9a88-142">Önceki örnekte, "Title" özelliği eklenen `ViewData` sözlük.</span><span class="sxs-lookup"><span data-stu-id="c9a88-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c9a88-143">"Title" özellik kullanılır *Pages/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="c9a88-143">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="c9a88-144">Aşağıdaki biçimlendirmede ilk birkaç satırlık gösterir *Pages/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="c9a88-144">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c9a88-145">"Title" özellik kullanılır *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="c9a88-145">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="c9a88-146">Aşağıdaki biçimlendirmede ilk birkaç satırlık gösterir *_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="c9a88-146">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="c9a88-147">Satır `@*Markup removed for brevity.*@` bir Razor açıklama.</span><span class="sxs-lookup"><span data-stu-id="c9a88-147">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="c9a88-148">HTML açıklamaları aksine (`<!-- -->`), Razor açıklama istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="c9a88-148">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="c9a88-149">Uygulamayı çalıştırın ve bağlantıları projesinde test (**giriş**, **hakkında**, **kişi**, **oluşturma**, **Düzenle**, ve **silmek**).</span><span class="sxs-lookup"><span data-stu-id="c9a88-149">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="c9a88-150">Her bir sayfada bir tarayıcı sekmesinde görebilirsiniz başlık ayarlar. Bir sayfaya yer işareti başlığı için yer işareti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9a88-150">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="c9a88-151">*Pages/Index.cshtml* ve *Pages/Movies/Index.cshtml* şu anda aynı ada sahip, ancak farklı değerlere sahip değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9a88-151">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="c9a88-152">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="c9a88-152">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="c9a88-153">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce dışındaki yerel ayarlar için (",") ondalık ve ABD İngilizcesi dışındaki tarih biçimleri, uygulamanızı globalize için adımlar atmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-153">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="c9a88-154">Bu [GitHub sorunu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) ondalık virgülle ekleme hakkında yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="c9a88-154">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="c9a88-155">`Layout` Özelliği ayarlanmış *Pages/_ViewStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c9a88-155">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="c9a88-156">Düzen dosyasını önceki biçimlendirme ayarlar *Pages/_Layout.cshtml* altındaki tüm Razor dosyaları için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="c9a88-156">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="c9a88-157">Bkz: [düzeni](xref:mvc/razor-pages/index#layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c9a88-157">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="c9a88-158">Güncelleştirme düzeni</span><span class="sxs-lookup"><span data-stu-id="c9a88-158">Update the layout</span></span>

<span data-ttu-id="c9a88-159">Değişiklik `<title>` öğesinde *Pages/_Layout.cshtml* daha kısa bir dize kullanmak üzere bir dosya.</span><span class="sxs-lookup"><span data-stu-id="c9a88-159">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="c9a88-160">Aşağıdaki bağlantı öğesinde Bul *Pages/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="c9a88-160">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="c9a88-161">Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c9a88-161">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="c9a88-162">Önceki bağlantı öğesi bir [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c9a88-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c9a88-163">Bu durumda, sahip [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="c9a88-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="c9a88-164">`asp-page="/Movies/Index"` Etiketi yardımcı öznitelik ve değeri bir bağlantı oluşturur `/Movies/Index` Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="c9a88-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="c9a88-165">Yaptığınız değişiklikleri kaydedin ve uygulamayı tıklayarak test **RpMovie** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="c9a88-165">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="c9a88-166">Bkz: [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) GitHub dosyasında.</span><span class="sxs-lookup"><span data-stu-id="c9a88-166">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-modelthe-create-page-model"></a><span data-ttu-id="c9a88-167">Oluştur sayfası ModeliADO.NET Oluştur sayfası modeli</span><span class="sxs-lookup"><span data-stu-id="c9a88-167">The Create page modelThe Create page model</span></span>

<span data-ttu-id="c9a88-168">İncelemek *Pages/Movies/Create.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="c9a88-168">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="c9a88-169">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="c9a88-169">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="c9a88-170">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="c9a88-170">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]</span></span>
::: moniker-end


<span data-ttu-id="c9a88-171">`OnGet` Yöntemi sayfa için gerekli herhangi bir durum başlatır.</span><span class="sxs-lookup"><span data-stu-id="c9a88-171">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="c9a88-172">Oluştur sayfası başlatmak için bunu herhangi bir durum yok `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="c9a88-172">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="c9a88-173">Daha sonra öğreticide gördüğünüz `OnGet` durumu Initialize yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c9a88-173">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="c9a88-174">`Page` Yöntemi oluşturur bir `PageResult` işleyen nesnesi *Create.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="c9a88-174">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="c9a88-175">`Movie` Özelliğini kullanan `[BindProperty]` için katılımı için öznitelik [model bağlama](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="c9a88-175">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="c9a88-176">Form oluştur form değerleri gönderdiğinde, ASP.NET çekirdeği çalışma zamanı gönderilen değerlerden bağlar `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="c9a88-176">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="c9a88-177">`OnPostAsync` Yöntemi sayfa form verileri gönderdiğinde çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c9a88-177">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="c9a88-178">Model hatalar varsa, formu, gönderilen tüm form verileri ile birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-178">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="c9a88-179">Formun gönderilen önce model hataların çoğu istemci tarafında yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-179">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="c9a88-180">Model hatası örneği bir tarihe dönüştürülemez tarih alanı için bir değer gönderme.</span><span class="sxs-lookup"><span data-stu-id="c9a88-180">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="c9a88-181">İstemci tarafı doğrulama ve daha sonra öğreticide model doğrulama hakkında daha fazla biz konuşun.</span><span class="sxs-lookup"><span data-stu-id="c9a88-181">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="c9a88-182">Model hatalar varsa, veri kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c9a88-182">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="c9a88-183">Razor sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="c9a88-183">The Create Razor Page</span></span>

<span data-ttu-id="c9a88-184">İncelemek *Pages/Movies/Create.cshtml* Razor sayfa dosyası:</span><span class="sxs-lookup"><span data-stu-id="c9a88-184">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
