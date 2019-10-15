---
title: ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme hakkında bilgi edinin.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0f6535462fe2d308825bf7289c10d2b0690cebd4
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334108"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="fefa7-103">ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="fefa7-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="fefa7-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="fefa7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fefa7-105">Yapı iskelesi film uygulamasının iyi bir başlangıcı vardır ancak sunum ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="fefa7-106">**ReleaseDate** **Yayın tarihi** (iki sözcük) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Chrome 'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="fefa7-108">Oluşturulan kodu Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="fefa7-108">Update the generated code</span></span>

<span data-ttu-id="fefa7-109">*Modeller/film. cs* dosyasını açın ve aşağıdaki kodda gösterilen vurgulanmış satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="fefa7-110">@No__t-0 veri ek açıklaması, Entity Framework Core `Price` ' i veritabanında para birimine doğru şekilde eşlemesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="fefa7-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="fefa7-111">Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="fefa7-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="fefa7-112">[Veri açıklamaları](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="fefa7-113">[Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği bir alanın adı için (Bu durumda "ReleaseDate" yerine "Yayın tarihi") görüntüleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="fefa7-114">[DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih), bu nedenle alanda depolanan zaman bilgileri gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="fefa7-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="fefa7-115">Hedef URL 'yi görmek için sayfalara/filmlere gidin ve bir **düzenleme** bağlantısının üzerine gelin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Düzenleme bağlantısı üzerinde fare ile tarayıcı penceresi ve http://localhost:1234/Movies/Edit/5 bağlantı URL 'Si gösteriliyor](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="fefa7-117">**Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Sayfalar/filmler/Index. cshtml* dosyasındaki [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fefa7-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="fefa7-118">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="fefa7-119">Yukarıdaki kodda `AnchorTagHelper`, Razor sayfasından (yol göreli), `asp-page` ve yol kimliği (`asp-route-id`) HTML `href` öznitelik değerini dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fefa7-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="fefa7-120">Daha fazla bilgi için bkz. [Sayfalar Için URL oluşturma](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="fefa7-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="fefa7-121">Oluşturulan biçimlendirmeyi incelemek için sık kullandığınız tarayıcıdan **Görünüm kaynağını** kullanın.</span><span class="sxs-lookup"><span data-stu-id="fefa7-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="fefa7-122">Oluşturulan HTML 'nin bir bölümü aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fefa7-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="fefa7-123">Dinamik olarak oluşturulan bağlantılar film KIMLIĞINI bir sorgu dizesiyle (örneğin, `https://localhost:5001/Movies/Details?id=1` ' de `?id=1`) iletir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

### <a name="add-route-template"></a><span data-ttu-id="fefa7-124">Rota şablonu Ekle</span><span class="sxs-lookup"><span data-stu-id="fefa7-124">Add route template</span></span>

<span data-ttu-id="fefa7-125">"{İd: int}" yol şablonunu kullanmak için Düzenle, Ayrıntılar ve Sil Razor Pages güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-125">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="fefa7-126">Bu sayfaların her biri için Page yönergesini `@page` ' dan `@page "{id:int}"` ' e değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-126">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="fefa7-127">Uygulamayı çalıştırın ve kaynağı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-127">Run the app and then view source.</span></span> <span data-ttu-id="fefa7-128">Oluşturulan HTML, URL 'nin yol bölümüne KIMLIĞI ekler:</span><span class="sxs-lookup"><span data-stu-id="fefa7-128">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="fefa7-129">Tamsayıyı **içermeyen "** {id: int}" yol şablonuna sahip sayfaya yönelik bir Istek, HTTP 404 (bulunamadı) hatası döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="fefa7-129">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="fefa7-130">Örneğin, `http://localhost:5000/Movies/Details` bir 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-130">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="fefa7-131">KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-131">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="fefa7-132">@No__t davranışını test etmek için-0:</span><span class="sxs-lookup"><span data-stu-id="fefa7-132">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="fefa7-133">*Pages/filmler/details. cshtml* içindeki page yönergesini `@page "{id:int?}"` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fefa7-133">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="fefa7-134">@No__t-0 ' da ( *sayfalarda/filmlerde/details. cshtml. cs*) bir kesme noktası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fefa7-134">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="fefa7-135">@No__t-0 ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-135">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="fefa7-136">@No__t-0 yönergesi ile, kesme noktası hiçbir şekilde vurılmaz.</span><span class="sxs-lookup"><span data-stu-id="fefa7-136">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="fefa7-137">Yönlendirme Altyapısı HTTP 404 döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-137">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="fefa7-138">@No__t-0 ' ı kullanarak `OnGetAsync` yöntemi `NotFound` (HTTP 404) döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-138">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="fefa7-139">Eşzamanlılık özel durum işlemeyi gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="fefa7-139">Review concurrency exception handling</span></span>

<span data-ttu-id="fefa7-140">*Pages/filmler/Edit. cshtml. cs* dosyasında `OnPostAsync` yöntemini gözden geçirin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-140">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="fefa7-141">Önceki kod, bir istemci filmi sildiği ve diğer istemci filmle değişiklik yaptığı zaman eşzamanlılık özel durumlarını algılar.</span><span class="sxs-lookup"><span data-stu-id="fefa7-141">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="fefa7-142">@No__t-0 bloğunu test etmek için:</span><span class="sxs-lookup"><span data-stu-id="fefa7-142">To test the `catch` block:</span></span>

* <span data-ttu-id="fefa7-143">@No__t kesme noktası ayarla-0</span><span class="sxs-lookup"><span data-stu-id="fefa7-143">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="fefa7-144">Film için **Düzenle** ' yi seçin, değişiklikler yapın, ancak **Kaydet**' i girmeyin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-144">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="fefa7-145">Başka bir tarayıcı penceresinde, aynı filmin **Sil** bağlantısını seçin ve ardından filmi silin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-145">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="fefa7-146">Önceki tarayıcı penceresinde filmdeki değişiklikleri gönderin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-146">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="fefa7-147">Üretim kodu eşzamanlılık çakışmalarını algılamak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-147">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="fefa7-148">Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="fefa7-148">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="fefa7-149">Gönderme ve bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="fefa7-149">Posting and binding review</span></span>

<span data-ttu-id="fefa7-150">*Pages/filmler/Edit. cshtml. cs* dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-150">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

<span data-ttu-id="fefa7-151">Filmler/düzenleme sayfasına HTTP GET isteği yapıldığında (örneğin, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="fefa7-151">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="fefa7-152">@No__t-0 yöntemi, filmi veritabanından getirir ve `Page` yöntemini döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-152">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span>
* <span data-ttu-id="fefa7-153">@No__t-0 yöntemi *Sayfalar/filmler/Edit. cshtml* Razor sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="fefa7-153">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="fefa7-154">*Pages/filmler/Edit. cshtml* dosyası, film modelinin sayfada kullanılabilir olmasını sağlayan model yönergesini (`@model RazorPagesMovie.Pages.Movies.EditModel`) içerir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-154">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="fefa7-155">Düzenleme formu filmdeki değerlerle birlikte görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-155">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="fefa7-156">Filmler/Düzenle sayfası gönderildiğinde:</span><span class="sxs-lookup"><span data-stu-id="fefa7-156">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="fefa7-157">Sayfadaki form değerleri `Movie` özelliğine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-157">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="fefa7-158">@No__t-0 özniteliği [model bağlamayı](xref:mvc/models/model-binding)mümkün.</span><span class="sxs-lookup"><span data-stu-id="fefa7-158">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="fefa7-159">Model durumunda hatalar varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), form gönderilen değerlerle yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-159">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is redisplayed with the submitted values.</span></span>
* <span data-ttu-id="fefa7-160">Model hatası yoksa, film kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-160">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="fefa7-161">Razor sayfalarında Dizin, oluşturma ve silme gibi HTTP GET yöntemleri benzer bir düzende yer alır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-161">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="fefa7-162">Razor Oluştur sayfasındaki HTTP POST `OnPostAsync` yöntemi, Razor düzenleme sayfasındaki `OnPostAsync` yöntemine benzer bir düzen izler.</span><span class="sxs-lookup"><span data-stu-id="fefa7-162">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fefa7-163">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fefa7-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fefa7-164">[Önceki: bir veritabanıyla çalışma](xref:tutorials/razor-pages/sql)
> [Sonraki: Arama ekle](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="fefa7-164">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fefa7-165">Yapı iskelesi film uygulamasının iyi bir başlangıcı vardır ancak sunum ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-165">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="fefa7-166">**ReleaseDate** **Yayın tarihi** (iki sözcük) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-166">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Chrome 'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="fefa7-168">Oluşturulan kodu Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="fefa7-168">Update the generated code</span></span>

<span data-ttu-id="fefa7-169">*Modeller/film. cs* dosyasını açın ve aşağıdaki kodda gösterilen vurgulanmış satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-169">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="fefa7-170">@No__t-0 veri ek açıklaması, Entity Framework Core `Price` ' i veritabanında para birimine doğru şekilde eşlemesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="fefa7-170">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="fefa7-171">Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="fefa7-171">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="fefa7-172">[Veri açıklamaları](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-172">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="fefa7-173">[Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği bir alanın adı için (Bu durumda "ReleaseDate" yerine "Yayın tarihi") görüntüleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-173">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="fefa7-174">[DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih), bu nedenle alanda depolanan zaman bilgileri gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="fefa7-174">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="fefa7-175">Hedef URL 'yi görmek için sayfalara/filmlere gidin ve bir **düzenleme** bağlantısının üzerine gelin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-175">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Düzenleme bağlantısı üzerinde fare ile tarayıcı penceresi ve http://localhost:1234/Movies/Edit/5 bağlantı URL 'Si gösteriliyor](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="fefa7-177">**Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Sayfalar/filmler/Index. cshtml* dosyasındaki [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fefa7-177">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="fefa7-178">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-178">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="fefa7-179">Yukarıdaki kodda `AnchorTagHelper`, Razor sayfasından (yol göreli), `asp-page` ve yol kimliği (`asp-route-id`) HTML `href` öznitelik değerini dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fefa7-179">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="fefa7-180">Daha fazla bilgi için bkz. [Sayfalar Için URL oluşturma](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="fefa7-180">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="fefa7-181">Oluşturulan biçimlendirmeyi incelemek için sık kullandığınız tarayıcıdan **Görünüm kaynağını** kullanın.</span><span class="sxs-lookup"><span data-stu-id="fefa7-181">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="fefa7-182">Oluşturulan HTML 'nin bir bölümü aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fefa7-182">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="fefa7-183">Dinamik olarak oluşturulan bağlantılar film KIMLIĞINI bir sorgu dizesiyle (örneğin, `https://localhost:5001/Movies/Details?id=1` ' de `?id=1`) iletir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-183">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="fefa7-184">"{İd: int}" yol şablonunu kullanmak için Düzenle, Ayrıntılar ve Sil Razor Pages güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-184">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="fefa7-185">Bu sayfaların her biri için Page yönergesini `@page` ' dan `@page "{id:int}"` ' e değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-185">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="fefa7-186">Uygulamayı çalıştırın ve kaynağı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-186">Run the app and then view source.</span></span> <span data-ttu-id="fefa7-187">Oluşturulan HTML, URL 'nin yol bölümüne KIMLIĞI ekler:</span><span class="sxs-lookup"><span data-stu-id="fefa7-187">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="fefa7-188">Tamsayıyı **içermeyen "** {id: int}" yol şablonuna sahip sayfaya yönelik bir Istek, HTTP 404 (bulunamadı) hatası döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="fefa7-188">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="fefa7-189">Örneğin, `http://localhost:5000/Movies/Details` bir 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-189">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="fefa7-190">KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-190">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="fefa7-191">@No__t davranışını test etmek için-0:</span><span class="sxs-lookup"><span data-stu-id="fefa7-191">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="fefa7-192">*Pages/filmler/details. cshtml* içindeki page yönergesini `@page "{id:int?}"` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fefa7-192">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="fefa7-193">@No__t-0 ' da ( *sayfalarda/filmlerde/details. cshtml. cs*) bir kesme noktası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fefa7-193">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="fefa7-194">@No__t-0 ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-194">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="fefa7-195">@No__t-0 yönergesi ile, kesme noktası hiçbir şekilde vurılmaz.</span><span class="sxs-lookup"><span data-stu-id="fefa7-195">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="fefa7-196">Yönlendirme Altyapısı HTTP 404 döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-196">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="fefa7-197">@No__t-0 ' ı kullanarak `OnGetAsync` yöntemi `NotFound` (HTTP 404) döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-197">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="fefa7-198">Eşzamanlılık özel durum işlemeyi gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="fefa7-198">Review concurrency exception handling</span></span>

<span data-ttu-id="fefa7-199">*Pages/filmler/Edit. cshtml. cs* dosyasında `OnPostAsync` yöntemini gözden geçirin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-199">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="fefa7-200">Önceki kod, bir istemci filmi sildiği ve diğer istemci filmle değişiklik yaptığı zaman eşzamanlılık özel durumlarını algılar.</span><span class="sxs-lookup"><span data-stu-id="fefa7-200">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="fefa7-201">@No__t-0 bloğunu test etmek için:</span><span class="sxs-lookup"><span data-stu-id="fefa7-201">To test the `catch` block:</span></span>

* <span data-ttu-id="fefa7-202">@No__t kesme noktası ayarla-0</span><span class="sxs-lookup"><span data-stu-id="fefa7-202">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="fefa7-203">Film için **Düzenle** ' yi seçin, değişiklikler yapın, ancak **Kaydet**' i girmeyin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-203">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="fefa7-204">Başka bir tarayıcı penceresinde, aynı filmin **Sil** bağlantısını seçin ve ardından filmi silin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-204">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="fefa7-205">Önceki tarayıcı penceresinde filmdeki değişiklikleri gönderin.</span><span class="sxs-lookup"><span data-stu-id="fefa7-205">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="fefa7-206">Üretim kodu eşzamanlılık çakışmalarını algılamak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-206">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="fefa7-207">Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="fefa7-207">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="fefa7-208">Gönderme ve bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="fefa7-208">Posting and binding review</span></span>

<span data-ttu-id="fefa7-209">*Pages/filmler/Edit. cshtml. cs* dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="fefa7-209">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="fefa7-210">Filmler/düzenleme sayfasına HTTP GET isteği yapıldığında (örneğin, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="fefa7-210">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="fefa7-211">@No__t-0 yöntemi, filmi veritabanından getirir ve `Page` yöntemini döndürür.</span><span class="sxs-lookup"><span data-stu-id="fefa7-211">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="fefa7-212">@No__t-0 yöntemi *Sayfalar/filmler/Edit. cshtml* Razor sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="fefa7-212">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="fefa7-213">*Pages/filmler/Edit. cshtml* dosyası, film modelinin sayfada kullanılabilir olmasını sağlayan model yönergesini (`@model RazorPagesMovie.Pages.Movies.EditModel`) içerir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-213">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="fefa7-214">Düzenleme formu filmdeki değerlerle birlikte görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-214">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="fefa7-215">Filmler/Düzenle sayfası gönderildiğinde:</span><span class="sxs-lookup"><span data-stu-id="fefa7-215">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="fefa7-216">Sayfadaki form değerleri `Movie` özelliğine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-216">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="fefa7-217">@No__t-0 özniteliği [model bağlamayı](xref:mvc/models/model-binding)mümkün.</span><span class="sxs-lookup"><span data-stu-id="fefa7-217">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="fefa7-218">Model durumunda hatalar varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), form gönderilen değerlerle birlikte görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-218">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="fefa7-219">Model hatası yoksa, film kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-219">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="fefa7-220">Razor sayfalarında Dizin, oluşturma ve silme gibi HTTP GET yöntemleri benzer bir düzende yer alır.</span><span class="sxs-lookup"><span data-stu-id="fefa7-220">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="fefa7-221">Razor Oluştur sayfasındaki HTTP POST `OnPostAsync` yöntemi, Razor düzenleme sayfasındaki `OnPostAsync` yöntemine benzer bir düzen izler.</span><span class="sxs-lookup"><span data-stu-id="fefa7-221">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="fefa7-222">Arama sonraki öğreticiye eklenir.</span><span class="sxs-lookup"><span data-stu-id="fefa7-222">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fefa7-223">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fefa7-223">Additional resources</span></span>

* [<span data-ttu-id="fefa7-224">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="fefa7-224">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="fefa7-225">[Önceki: bir veritabanıyla çalışma](xref:tutorials/razor-pages/sql)
> [Sonraki: Arama ekle](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="fefa7-225">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end
