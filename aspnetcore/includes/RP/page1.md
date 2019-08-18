# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="1551b-101">ASP.NET Core Razor Pages scafkatlama</span><span class="sxs-lookup"><span data-stu-id="1551b-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="1551b-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1551b-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1551b-103">Bu öğreticide, önceki öğreticide scafkatlama tarafından oluşturulan Razor Pages incelenir.</span><span class="sxs-lookup"><span data-stu-id="1551b-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="1551b-104">[Görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) örnek.</span><span class="sxs-lookup"><span data-stu-id="1551b-104">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="1551b-105">Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları.</span><span class="sxs-lookup"><span data-stu-id="1551b-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="1551b-106">*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="1551b-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="1551b-107">Razor Pages, öğesinden `PageModel`türetilir.</span><span class="sxs-lookup"><span data-stu-id="1551b-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="1551b-108">Kural gereği, `PageModel`-türetilmiş sınıfı çağrılır `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="1551b-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="1551b-109">Oluşturucu, `MovieContext` sayfasına eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="1551b-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="1551b-110">Tüm yapı iskelesi sayfaları bu düzene uyar.</span><span class="sxs-lookup"><span data-stu-id="1551b-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="1551b-111">Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .</span><span class="sxs-lookup"><span data-stu-id="1551b-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="1551b-112">Sayfa için bir istek yapıldığında, `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="1551b-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="1551b-113">`OnGetAsync`ya `OnGet` da bir Razor sayfasında, sayfanın durumunu başlatmak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1551b-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="1551b-114">Bu durumda, `OnGetAsync` filmlerin bir listesini alır ve görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1551b-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="1551b-115">Döndürüldüğünde `OnGet` veyadöndüğünde`Task`,returnyöntemikullanılmaz. `OnGetAsync` `void`</span><span class="sxs-lookup"><span data-stu-id="1551b-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="1551b-116">Dönüş türü `IActionResult` veya `Task<IActionResult>`olduğunda, bir return ifadesinin sağlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1551b-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="1551b-117">Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1551b-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="1551b-118">*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="1551b-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="1551b-119">Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="1551b-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="1551b-120">Bir `@` sembolden sonra [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)geldiğinde, Razor 'e özgü işaretlere geçiş yapar, aksi takdirde öğesine C#geçirir.</span><span class="sxs-lookup"><span data-stu-id="1551b-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="1551b-121">Razor yönergesi, dosyayı, istekleri işleyebileceği anlamına gelen &mdash; bir MVC eylemine dönüştürür. `@page`</span><span class="sxs-lookup"><span data-stu-id="1551b-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="1551b-122">`@page`sayfada ilk Razor yönergesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1551b-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="1551b-123">`@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="1551b-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="1551b-124">Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="1551b-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="1551b-125">Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="1551b-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="1551b-126">HTML Yardımcısı, görünen adı `Title` belirlemede lambda ifadesinde başvurulan özelliği inceler. `DisplayNameFor`</span><span class="sxs-lookup"><span data-stu-id="1551b-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="1551b-127">Lambda ifadesi değerlendirilmek yerine incelenir.</span><span class="sxs-lookup"><span data-stu-id="1551b-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="1551b-128">`model`Diğer bir deyişle, `model.Movie`,, veya `model.Movie[0]` `null` boş olduğunda erişim ihlali yoktur.</span><span class="sxs-lookup"><span data-stu-id="1551b-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="1551b-129">Lambda ifadesi değerlendirildiğinde (örneğin, ile `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1551b-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="1551b-130">@model Yönergesi</span><span class="sxs-lookup"><span data-stu-id="1551b-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="1551b-131">`@model` Yönerge, Razor sayfasına geçirilen modelin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="1551b-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="1551b-132">Yukarıdaki örnekte, `@model` satır türetilen sınıfı Razor sayfası için `PageModel`kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="1551b-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="1551b-133">Model, sayfadaki `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1551b-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="1551b-134">ViewData ve Layout</span><span class="sxs-lookup"><span data-stu-id="1551b-134">ViewData and layout</span></span>

<span data-ttu-id="1551b-135">Aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1551b-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="1551b-136">Önceki vurgulanan kod, Razor geçişi örneği olan bir örnektir C#.</span><span class="sxs-lookup"><span data-stu-id="1551b-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="1551b-137">Ve karakterleri bir C# `{` `}`</span><span class="sxs-lookup"><span data-stu-id="1551b-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="1551b-138">Temel sınıfın, bir görünüme `ViewData` geçirmek istediğiniz verileri eklemek için kullanılabilecek bir Dictionary özelliği vardır. `PageModel`</span><span class="sxs-lookup"><span data-stu-id="1551b-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="1551b-139">Bir anahtar/değer örüntüsünün kullanıldığı `ViewData` sözlüğe nesneler eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="1551b-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="1551b-140">Yukarıdaki örnekte, "title" özelliği `ViewData` sözlüğe eklenir.</span><span class="sxs-lookup"><span data-stu-id="1551b-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1551b-141">"Title" özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1551b-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="1551b-142">Aşağıdaki biçimlendirme *sayfa/paylaşılan/_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.</span><span class="sxs-lookup"><span data-stu-id="1551b-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1551b-143">"Title" özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1551b-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="1551b-144">Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.</span><span class="sxs-lookup"><span data-stu-id="1551b-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="1551b-145">Satır `@*Markup removed for brevity.*@` bir Razor açıklamadır.</span><span class="sxs-lookup"><span data-stu-id="1551b-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="1551b-146">HTML yorumlarının (`<!-- -->`) aksine, Razor açıklamaları istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="1551b-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="1551b-147">Uygulamayı çalıştırın ve projedeki bağlantıları test edin (**ana**, **hakkında**, **iletişim**, **oluşturma**, **düzenleme**ve **silme**).</span><span class="sxs-lookup"><span data-stu-id="1551b-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="1551b-148">Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1551b-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="1551b-149">*Pages/index. cshtml* ve *Pages/filmlerini/index. cshtml* Şu anda aynı başlığa sahiptir, ancak bunları farklı değerlere sahip olacak şekilde değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1551b-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="1551b-150">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="1551b-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="1551b-151">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1551b-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="1551b-152">Bu GitHub, ondalık virgülden ekleme hakkında yönergeler için [4076 sorun](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .</span><span class="sxs-lookup"><span data-stu-id="1551b-152">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="1551b-153">Özelliği Pages */_viewstart. cshtml* dosyasında ayarlanır: `Layout`</span><span class="sxs-lookup"><span data-stu-id="1551b-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="1551b-154">Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1551b-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="1551b-155">Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="1551b-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="1551b-156">Düzeni güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="1551b-156">Update the layout</span></span>

<span data-ttu-id="1551b-157">*Pages/Shared/_Layout. cshtml* dosyasındaki öğeyidahakısabirdizekullanacakşekildedeğiştirin.`<title>`</span><span class="sxs-lookup"><span data-stu-id="1551b-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="1551b-158">*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.</span><span class="sxs-lookup"><span data-stu-id="1551b-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```

<span data-ttu-id="1551b-159">Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1551b-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="1551b-160">Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="1551b-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="1551b-161">Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1551b-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="1551b-162">Etiket Yardımcısı özniteliği ve değeri `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. `asp-page="/Movies/Index"`</span><span class="sxs-lookup"><span data-stu-id="1551b-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="1551b-163">Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="1551b-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="1551b-164">GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="1551b-164">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="1551b-165">Sayfa oluştur modeli</span><span class="sxs-lookup"><span data-stu-id="1551b-165">The Create page model</span></span>

<span data-ttu-id="1551b-166">*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="1551b-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end

<span data-ttu-id="1551b-167">`OnGet` Yöntemi, sayfa için gereken tüm durumları başlatır.</span><span class="sxs-lookup"><span data-stu-id="1551b-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="1551b-168">Oluşturma sayfasında, başlatılacak durum yoktur, bu nedenle `Page` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1551b-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="1551b-169">Öğreticide daha sonra Yöntem başlatma `OnGet` durumunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="1551b-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="1551b-170">Yöntemi Create *. cshtml* sayfasını işleyen bir `PageResult` nesne oluşturur. `Page`</span><span class="sxs-lookup"><span data-stu-id="1551b-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="1551b-171">Özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. `Movie`</span><span class="sxs-lookup"><span data-stu-id="1551b-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="1551b-172">Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, gönderilen değerleri `Movie` modele bağlar.</span><span class="sxs-lookup"><span data-stu-id="1551b-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="1551b-173">Bu `OnPostAsync` Yöntem, sayfa form verileri göndertiğinde çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="1551b-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="1551b-174">Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1551b-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="1551b-175">Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="1551b-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="1551b-176">Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme.</span><span class="sxs-lookup"><span data-stu-id="1551b-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="1551b-177">Öğreticide daha sonra istemci tarafı doğrulama ve model doğrulama hakkında daha fazla bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1551b-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="1551b-178">Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1551b-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="1551b-179">Razor Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="1551b-179">The Create Razor Page</span></span>

<span data-ttu-id="1551b-180">*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="1551b-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).

![VS17 view of Create.cshtml page](page/_static/th.png)
-->
