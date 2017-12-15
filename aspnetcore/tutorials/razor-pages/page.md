---
title: "ASP.NET Core kurulmuş Razor sayfalarında"
author: rick-anderson
description: "Razor yapı iskelesi tarafından oluşturulan sayfaları açıklanmaktadır."
keywords: "ASP.NET Core, Razor sayfalarının Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: e42e7e469e411d2d4bc1bd1b3a3995a77c355ebd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="3ba16-104">ASP.NET Core kurulmuş Razor sayfalarında</span><span class="sxs-lookup"><span data-stu-id="3ba16-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3ba16-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3ba16-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3ba16-106">Bu öğretici Razor önceki öğretici konusunda yapı iskelesi tarafından oluşturulan sayfaları inceler [bir modeli ekleme](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span><span class="sxs-lookup"><span data-stu-id="3ba16-106">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="3ba16-107">[Görüntülemek veya karşıdan](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) örnek.</span><span class="sxs-lookup"><span data-stu-id="3ba16-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="3ba16-108">Oluştur, Sil, Ayrıntılar ve düzenleme sayfaları.</span><span class="sxs-lookup"><span data-stu-id="3ba16-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="3ba16-109">İncelemek *Pages/Movies/Index.cshtml.cs* arka plan kod dosyası:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="3ba16-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="3ba16-110">Razor sayfalarının türetilir `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="3ba16-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="3ba16-111">Kural tarafından `PageModel`-türetilmiş sınıf çağrılır `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="3ba16-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="3ba16-112">Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) eklemek için `MovieContext` sayfası.</span><span class="sxs-lookup"><span data-stu-id="3ba16-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="3ba16-113">Bu yol kurulmuş tüm sayfaları izler.</span><span class="sxs-lookup"><span data-stu-id="3ba16-113">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="3ba16-114">Bkz: [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) Entity Framework zaman uyumsuz programing hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3ba16-114">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="3ba16-115">Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi Razor sayfasına filmler listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="3ba16-115">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="3ba16-116">`OnGetAsync`veya `OnGet` sayfası için durum başlatmak için bir Razor sayfasında olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="3ba16-116">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="3ba16-117">Bu durumda, `OnGetAsync` görüntülenecek filmler listesini alır.</span><span class="sxs-lookup"><span data-stu-id="3ba16-117">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="3ba16-118">İncelemek *Pages/Movies/Index.cshtml* Razor sayfasını:</span><span class="sxs-lookup"><span data-stu-id="3ba16-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="3ba16-119">HTML Razor C# veya Razor özgü biçimlendirme geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="3ba16-120">Zaman bir `@` simgesi tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords), Razor özgü biçimlendirme geçişleri, aksi takdirde C# diline geçiş.</span><span class="sxs-lookup"><span data-stu-id="3ba16-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="3ba16-121">`@page` Razor yönergesi yapar dosya MVC eyleme &mdash; istek işleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="3ba16-122">`@page`ilk Razor yönergesi bir sayfa üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3ba16-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="3ba16-123">`@page`Razor özgü biçimlendirme geçiş, bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="3ba16-124">Bkz: [Razor sözdizimi](xref:mvc/views/razor#razor-syntax) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3ba16-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="3ba16-125">Aşağıdaki HTML Yardımcısı kullanılan lambda ifadesi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="3ba16-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="3ba16-126">`DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adı belirlemek için lambda ifadesinde başvurulan özelliği.</span><span class="sxs-lookup"><span data-stu-id="3ba16-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="3ba16-127">Lambda ifadesi hesaplanan Denetlenmekte yerine.</span><span class="sxs-lookup"><span data-stu-id="3ba16-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="3ba16-128">Hiçbir erişim ihlali var. anlamına zaman `model`, `model.Movie`, veya `model.Movie[0]` olan `null` veya boş.</span><span class="sxs-lookup"><span data-stu-id="3ba16-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="3ba16-129">Lambda ifadesi ne zaman değerlendirildiği (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="3ba16-130">@model Yönergesi</span><span class="sxs-lookup"><span data-stu-id="3ba16-130">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="3ba16-131">`@model` Yönergesi Razor sayfasına iletilen model türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="3ba16-132">Önceki örnekte `@model` satır yapar `PageModel`-türetilmiş sınıf Razor sayfasına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="3ba16-133">Model kullanılan `@Html.DisplayNameFor` ve `@Html.DisplayName` [HTML Yardımcıları](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında.</span><span class="sxs-lookup"><span data-stu-id="3ba16-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="3ba16-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData ve düzeni</span><span class="sxs-lookup"><span data-stu-id="3ba16-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="3ba16-135">Aşağıdaki kod göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="3ba16-135">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="3ba16-136">Önceki vurgulanmış kodu koddan C# diline Razor örneğidir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="3ba16-137">`{` Ve `}` karakterleri bir C# kod bloğunun içine alın.</span><span class="sxs-lookup"><span data-stu-id="3ba16-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="3ba16-138">`PageModel` Taban sınıfının bir `ViewData` bir görünüme iletmek istediğiniz verileri eklemek için kullanılan sözlüğü özelliği.</span><span class="sxs-lookup"><span data-stu-id="3ba16-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="3ba16-139">Nesneleri eklemek `ViewData` bir anahtar/değer modeli kullanarak sözlük.</span><span class="sxs-lookup"><span data-stu-id="3ba16-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="3ba16-140">Önceki örnekte, "Title" özelliği eklenen `ViewData` sözlük.</span><span class="sxs-lookup"><span data-stu-id="3ba16-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="3ba16-141">"Title" özellik kullanılır *Pages/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="3ba16-141">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="3ba16-142">Aşağıdaki biçimlendirmede ilk birkaç satırlık gösterir *Pages/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="3ba16-142">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="3ba16-143">Satır `@*Markup removed for brevity.*@` bir Razor açıklama.</span><span class="sxs-lookup"><span data-stu-id="3ba16-143">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="3ba16-144">HTML açıklamaları aksine (`<!-- -->`), Razor açıklama istemciye gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="3ba16-144">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="3ba16-145">Uygulamayı çalıştırın ve bağlantıları projesinde test (**giriş**, **hakkında**, **kişi**, **oluşturma**, **Düzenle**, ve **silmek**).</span><span class="sxs-lookup"><span data-stu-id="3ba16-145">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="3ba16-146">Her bir sayfada bir tarayıcı sekmesinde görebilirsiniz başlık ayarlar. Bir sayfaya yer işareti başlığı için yer işareti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3ba16-146">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="3ba16-147">*Pages/Index.cshtml* ve *Pages/Movies/Index.cshtml* şu anda aynı ada sahip, ancak farklı değerlere sahip değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ba16-147">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="3ba16-148">`Layout` Özelliği ayarlanmış *Pages/_ViewStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3ba16-148">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="3ba16-149">Düzen dosyasını önceki biçimlendirme ayarlar *Pages/_Layout.cshtml* altındaki tüm Razor dosyaları için *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="3ba16-149">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="3ba16-150">Bkz: [düzeni](xref:mvc/razor-pages/index#layout) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3ba16-150">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="3ba16-151">Güncelleştirme düzeni</span><span class="sxs-lookup"><span data-stu-id="3ba16-151">Update the layout</span></span>

<span data-ttu-id="3ba16-152">Değişiklik `<title>` öğesinde *Pages/_Layout.cshtml* daha kısa bir dize kullanmak üzere bir dosya.</span><span class="sxs-lookup"><span data-stu-id="3ba16-152">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="3ba16-153">Aşağıdaki bağlantı öğesinde Bul *Pages/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="3ba16-153">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="3ba16-154">Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3ba16-154">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="3ba16-155">Önceki bağlantı öğesi bir [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="3ba16-155">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="3ba16-156">Bu durumda, sahip [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="3ba16-156">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="3ba16-157">`asp-page="/Movies/Index"` Etiketi yardımcı öznitelik ve değeri bir bağlantı oluşturur `/Movies/Index` Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="3ba16-157">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="3ba16-158">Yaptığınız değişiklikleri kaydedin ve uygulamayı tıklayarak test **RpMovie** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="3ba16-158">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="3ba16-159">Bkz: [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) GitHub dosyasında.</span><span class="sxs-lookup"><span data-stu-id="3ba16-159">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="3ba16-160">Oluştur arka plan kod sayfası</span><span class="sxs-lookup"><span data-stu-id="3ba16-160">The Create code-behind page</span></span>

<span data-ttu-id="3ba16-161">İncelemek *Pages/Movies/Create.cshtml.cs* arka plan kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="3ba16-161">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="3ba16-162">`OnGet` Yöntemi sayfa için gerekli herhangi bir durum başlatır.</span><span class="sxs-lookup"><span data-stu-id="3ba16-162">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="3ba16-163">Oluştur sayfası başlatmak için herhangi bir durum yok.</span><span class="sxs-lookup"><span data-stu-id="3ba16-163">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="3ba16-164">`Page` Yöntemi oluşturur bir `PageResult` işleyen nesnesi *Create.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="3ba16-164">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="3ba16-165">`Movie` Özelliğini kullanan `[BindProperty]` için katılımı için öznitelik [model bağlama](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="3ba16-165">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="3ba16-166">Form oluştur form değerleri gönderdiğinde, ASP.NET çekirdeği çalışma zamanı gönderilen değerlerden bağlar `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="3ba16-166">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="3ba16-167">`OnPostAsync` Yöntemi sayfa form verileri gönderdiğinde çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3ba16-167">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="3ba16-168">Model hatalar varsa, formu, gönderilen tüm form verileri ile birlikte yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-168">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="3ba16-169">Formun gönderilen önce model hataların çoğu istemci tarafında yakalanabilir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-169">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="3ba16-170">Model hatası örneği bir tarihe dönüştürülemez tarih alanı için bir değer gönderme.</span><span class="sxs-lookup"><span data-stu-id="3ba16-170">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="3ba16-171">İstemci tarafı doğrulama ve daha sonra öğreticide model doğrulama hakkında daha fazla biz konuşun.</span><span class="sxs-lookup"><span data-stu-id="3ba16-171">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="3ba16-172">Model hatalar varsa, veri kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-172">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="3ba16-173">Razor sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="3ba16-173">The Create Razor Page</span></span>

<span data-ttu-id="3ba16-174">İncelemek *Pages/Movies/Create.cshtml* Razor sayfa dosyası:</span><span class="sxs-lookup"><span data-stu-id="3ba16-174">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="3ba16-175">Visual Studio görüntüler `<form method="post">` etiket etiket Yardımcıları için kullanılan farklı bir yazı tipi.</span><span class="sxs-lookup"><span data-stu-id="3ba16-175">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="3ba16-176">`<form method="post">` Öğesi bir [Form etiketi yardımcı](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="3ba16-176">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="3ba16-177">Form etiketi yardımcı otomatik olarak içeren bir [antiforgery belirteci](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="3ba16-177">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![Create.cshtml sayfasının VS17 görünümü](page/_static/th.png)

<span data-ttu-id="3ba16-179">Yapı iskelesi altyapısı her bir alan için Razor biçimlendirme (ID dışında) modelindeki aşağıdakine benzer oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3ba16-179">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="3ba16-180">[Doğrulama etiket Yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve ` <span asp-validation-for`) doğrulama hataları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3ba16-180">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="3ba16-181">Doğrulama bu seri ilerleyen bölümlerinde daha ayrıntılı ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="3ba16-181">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="3ba16-182">[Etiket etiket Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) etiket resim yazısı oluşturur ve `for` için öznitelik `Title` özelliği.</span><span class="sxs-lookup"><span data-stu-id="3ba16-182">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="3ba16-183">[Giriş etiketi yardımcı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) kullanan [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve istemci tarafında jQuery doğrulama için gereken HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="3ba16-183">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="3ba16-184">Sonraki öğretici, SQL Server yerel veritabanı ve veritabanı dengeli açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3ba16-184">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3ba16-185">[Önceki: bir model ekleme](xref:tutorials/razor-pages/model)
[sonraki: SQL Server yerel veritabanı](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="3ba16-185">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
