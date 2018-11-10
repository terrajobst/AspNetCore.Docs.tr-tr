# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="8dd22-101">ASP.NET core'da iskeleli Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="8dd22-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="8dd22-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8dd22-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8dd22-103">Bu öğreticide, önceki öğreticide yapı iskelesi oluşturulmuş Razor sayfaları inceler.</span><span class="sxs-lookup"><span data-stu-id="8dd22-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="8dd22-104">[Görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) örnek.</span><span class="sxs-lookup"><span data-stu-id="8dd22-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="8dd22-105">Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları.</span><span class="sxs-lookup"><span data-stu-id="8dd22-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="8dd22-106">İnceleme *Pages/Movies/Index.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="8dd22-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="8dd22-107">Razor sayfaları türetilir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8dd22-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="8dd22-108">Kural olarak, `PageModel`-türetilmiş sınıf adlı `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="8dd22-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="8dd22-109">Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) eklemek için `MovieContext` sayfası.</span><span class="sxs-lookup"><span data-stu-id="8dd22-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="8dd22-110">Bu düzen iskele kurulmuş tüm sayfaları izleyin.</span><span class="sxs-lookup"><span data-stu-id="8dd22-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="8dd22-111">Bkz: [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) Entity Framework ile zaman uyumsuz programlamayı hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8dd22-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="8dd22-112">Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi için Razor sayfası filmler listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="8dd22-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="8dd22-113">`OnGetAsync` veya `OnGet` durumunu başlatmak için bir Razor sayfası adı verilir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="8dd22-114">Bu durumda, `OnGetAsync` filmler listesini alır ve görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8dd22-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="8dd22-115">Zaman `OnGet` döndürür `void` veya `OnGetAsync` döndürür`Task`, dönüş yöntem kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8dd22-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="8dd22-116">Dönüş türü olduğunda `IActionResult` veya `Task<IActionResult>`, bir return deyimi sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8dd22-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="8dd22-117">Örneğin, *Pages/Movies/Create.cshtml.cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8dd22-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="8dd22-118">İnceleme *Pages/Movies/Index.cshtml* Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="8dd22-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="8dd22-119">Razor HTML, C# veya Razor özgü biçimlendirme geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8dd22-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="8dd22-120">Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)Razor özgü biçimlendirme içinde geçiş, aksi takdirde, C# diline geçer.</span><span class="sxs-lookup"><span data-stu-id="8dd22-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="8dd22-121">`@page` Razor yönergesi, bir MVC eylemlere dosya yapar &mdash; istek işleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="8dd22-122">`@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8dd22-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="8dd22-123">`@page` Razor özgü biçimlendirme içinde geçiş bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="8dd22-124">Bkz: [Razor sözdizimi](xref:mvc/views/razor#razor-syntax) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8dd22-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="8dd22-125">Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="8dd22-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="8dd22-126">`DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği.</span><span class="sxs-lookup"><span data-stu-id="8dd22-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="8dd22-127">Lambda ifadesi değerlendirilir inceledi yerine.</span><span class="sxs-lookup"><span data-stu-id="8dd22-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="8dd22-128">Hiçbir erişim ihlali var. anlamına olduğunda `model`, `model.Movie`, veya `model.Movie[0]` olan `null` veya boş.</span><span class="sxs-lookup"><span data-stu-id="8dd22-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="8dd22-129">Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="8dd22-130">@model Yönergesi</span><span class="sxs-lookup"><span data-stu-id="8dd22-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="8dd22-131">`@model` Yönergesi için Razor sayfası geçirilen modelin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="8dd22-132">Önceki örnekte `@model` satır yapar `PageModel`-türetilmiş sınıf için bir Razor sayfası kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="8dd22-133">Model kullanılır `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML Yardımcıları](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında.</span><span class="sxs-lookup"><span data-stu-id="8dd22-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="8dd22-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData ve düzeni</span><span class="sxs-lookup"><span data-stu-id="8dd22-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="8dd22-135">Aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8dd22-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="8dd22-136">Önceki vurgulanmış kodu, C# koduna geçiş Razor örneğidir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="8dd22-137">`{` Ve `}` karakter C# kod bloğunun içine alın.</span><span class="sxs-lookup"><span data-stu-id="8dd22-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="8dd22-138">`PageModel` Temel sınıfa sahip değil bir `ViewData` bir görünüme iletmek istediğiniz veri eklemek için kullanılan bir sözlük özelliği.</span><span class="sxs-lookup"><span data-stu-id="8dd22-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="8dd22-139">Nesneleri eklemek `ViewData` bir anahtar/değer deseni kullanılarak sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="8dd22-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="8dd22-140">Yukarıdaki örnekte, "Title" özelliğini eklenen `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="8dd22-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8dd22-141">"Title" özelliğini kullanılan *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="8dd22-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8dd22-142">İlk birkaç satırı aşağıdaki biçimlendirme gösterir *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="8dd22-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8dd22-143">"Title" özelliğini kullanılan *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="8dd22-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8dd22-144">İlk birkaç satırı aşağıdaki biçimlendirme gösterir *_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="8dd22-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="8dd22-145">Satır `@*Markup removed for brevity.*@` bir Razor açıklama.</span><span class="sxs-lookup"><span data-stu-id="8dd22-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="8dd22-146">HTML Yorumlarını aksine (`<!-- -->`), Razor açıklama istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="8dd22-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="8dd22-147">Uygulamayı çalıştırın ve bağlantıları projedeki test (**giriş**, **hakkında**, **kişi**, **Oluştur**, **Düzenle**, ve **Sil**).</span><span class="sxs-lookup"><span data-stu-id="8dd22-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="8dd22-148">Her sayfada başlık, tarayıcı sekmesinde görebilirsiniz ayarlar. Bir sayfaya yer işareti başlık yer işareti için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8dd22-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="8dd22-149">*Pages/Index.cshtml* ve *Pages/Movies/Index.cshtml* şu anda aynı başlığa sahip, ancak bunları farklı değerlere sahip değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8dd22-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="8dd22-150">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="8dd22-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="8dd22-151">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel ayarlar için (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="8dd22-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="8dd22-152">Bu [GitHub sorunu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) ondalık virgülle ekleme hakkında yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="8dd22-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="8dd22-153">`Layout` Özelliği ayarlandığında *Pages/_ViewStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8dd22-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="8dd22-154">Düzen dosyası önceki biçimlendirme ayarlar *Pages/Shared/_Layout.cshtml* altındaki tüm Razor dosyaları için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="8dd22-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="8dd22-155">Bkz: [Düzen](xref:razor-pages/index#layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8dd22-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="8dd22-156">Düzeni güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="8dd22-156">Update the layout</span></span>

<span data-ttu-id="8dd22-157">Değişiklik `<title>` öğesinde *Pages/Shared/_Layout.cshtml* daha kısa bir dize kullanmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="8dd22-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="8dd22-158">Aşağıdaki bağlantı öğe Bul *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="8dd22-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="8dd22-159">Önceki öğeyle aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8dd22-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="8dd22-160">Önceki yer işareti öğesi bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8dd22-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="8dd22-161">Bu durumda sahip [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8dd22-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="8dd22-162">`asp-page="/Movies/Index"` Etiketi yardımcı öznitelik ve değer oluşturan bir bağlantı `/Movies/Index` Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="8dd22-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="8dd22-163">Değişikliklerinizi kaydedip tıklayarak uygulamayı test etme **RpMovie** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8dd22-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="8dd22-164">Bkz: [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) github'da dosya.</span><span class="sxs-lookup"><span data-stu-id="8dd22-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="8dd22-165">Oluşturma sayfa modeli</span><span class="sxs-lookup"><span data-stu-id="8dd22-165">The Create page model</span></span>

<span data-ttu-id="8dd22-166">İnceleme *Pages/Movies/Create.cshtml.cs* sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="8dd22-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end


<span data-ttu-id="8dd22-167">`OnGet` Yöntemi sayfa için gerekli herhangi bir durum başlatır.</span><span class="sxs-lookup"><span data-stu-id="8dd22-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="8dd22-168">Oluştur sayfasında başlatmak için bunu herhangi bir duruma sahip olmayan `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8dd22-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="8dd22-169">Daha sonra öğreticide gördüğünüz `OnGet` durumu Initialize yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8dd22-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="8dd22-170">`Page` Yöntemi oluşturur bir `PageResult` işleyen nesnesi *Create.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="8dd22-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="8dd22-171">`Movie` Özelliği kullanan `[BindProperty]` kabul etmek için için öznitelik [model bağlama](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="8dd22-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="8dd22-172">Form oluştur form değerleri gönderdiğinde, ASP.NET Core çalışma zamanı gönderilen değerine bağlar `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="8dd22-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="8dd22-173">`OnPostAsync` Sayfasının form verileri gönderildiğinde yöntemi çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="8dd22-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="8dd22-174">Model hataları varsa, form, gönderilen tüm form verileri ile birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="8dd22-175">Form gönderildiğinde önce çoğu model hataları üzerinde istemci tarafı yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="8dd22-176">Model hatası örneği bir tarihe dönüştürülür tarih alanı için bir değer gönderiyor.</span><span class="sxs-lookup"><span data-stu-id="8dd22-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="8dd22-177">İstemci tarafı doğrulama ve daha sonra öğreticide model doğrulama hakkında daha fazla konuşacağız.</span><span class="sxs-lookup"><span data-stu-id="8dd22-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="8dd22-178">Herhangi bir model hata varsa, verileri kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8dd22-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="8dd22-179">Razor sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="8dd22-179">The Create Razor Page</span></span>

<span data-ttu-id="8dd22-180">İnceleme *Pages/Movies/Create.cshtml* Razor sayfası dosyası:</span><span class="sxs-lookup"><span data-stu-id="8dd22-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
