---
title: ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme hakkında bilgi edinin.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f1f69b7facf584d46248405c808e75bdd8448d2b
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440320"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="f8089-103">ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="f8089-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="f8089-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8089-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f8089-105">Yapı iskelesi film uygulamasının iyi bir başlangıcı vardır ancak sunum ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="f8089-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="f8089-106">**ReleaseDate** **Yayın tarihi** (iki sözcük) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f8089-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Chrome 'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="f8089-108">Oluşturulan kodu Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="f8089-108">Update the generated code</span></span>

<span data-ttu-id="f8089-109">*Modeller/film. cs* dosyasını açın ve aşağıdaki kodda gösterilen vurgulanmış satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f8089-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="f8089-110">Veri `[Column(TypeName = "decimal(18, 2)")]` ek açıklaması, Entity Framework Core veritabanındaki para birimiyle `Price` doğru şekilde eşlenmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="f8089-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="f8089-111">Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="f8089-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="f8089-112">[Veri açıklamaları](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="f8089-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="f8089-113">[Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği bir alanın adı için (Bu durumda "ReleaseDate" yerine "Yayın tarihi") görüntüleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f8089-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="f8089-114">[DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih), bu nedenle alanda depolanan zaman bilgileri gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="f8089-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="f8089-115">Hedef URL 'yi görmek için sayfalara/filmlere gidin ve bir **düzenleme** bağlantısının üzerine gelin.</span><span class="sxs-lookup"><span data-stu-id="f8089-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Düzenleme bağlantısı üzerinde fare ile tarayıcı penceresi ve bağlantı URL 'si http://localhost:1234/Movies/Edit/5 gösteriliyor](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="f8089-117">**Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Sayfalar/filmler/Index. cshtml* dosyasındaki [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f8089-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="f8089-118">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="f8089-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="f8089-119">Yukarıdaki kodda `AnchorTagHelper` , Razor sayfasından (yol göreli `asp-page`olur) `href` , ve yol kimliği (`asp-route-id`) html öznitelik değerini dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f8089-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="f8089-120">Daha fazla bilgi için bkz. [Sayfalar Için URL oluşturma](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="f8089-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="f8089-121">Oluşturulan biçimlendirmeyi incelemek için sık kullandığınız tarayıcıdan **Görünüm kaynağını** kullanın.</span><span class="sxs-lookup"><span data-stu-id="f8089-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="f8089-122">Oluşturulan HTML 'nin bir bölümü aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="f8089-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="f8089-123">Dinamik olarak oluşturulan bağlantılar film kimliğini bir sorgu dizesiyle (örneğin, `?id=1` içinde `https://localhost:5001/Movies/Details?id=1`) iletir.</span><span class="sxs-lookup"><span data-stu-id="f8089-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="f8089-124">"{İd: int}" yol şablonunu kullanmak için Düzenle, Ayrıntılar ve Sil Razor Pages güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="f8089-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="f8089-125">Bu sayfaların her biri için Page yönergesini ' den `@page` ' e `@page "{id:int}"`değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f8089-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="f8089-126">Uygulamayı çalıştırın ve kaynağı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="f8089-126">Run the app and then view source.</span></span> <span data-ttu-id="f8089-127">Oluşturulan HTML, URL 'nin yol bölümüne KIMLIĞI ekler:</span><span class="sxs-lookup"><span data-stu-id="f8089-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="f8089-128">Tamsayıyı **içermeyen "** {id: int}" yol şablonuna sahip sayfaya yönelik bir Istek, HTTP 404 (bulunamadı) hatası döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="f8089-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="f8089-129">Örneğin, `http://localhost:5000/Movies/Details` bir 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="f8089-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="f8089-130">Kimliği isteğe bağlı yapmak için yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f8089-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="f8089-131">Davranışını test etmek için `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="f8089-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="f8089-132">*Pages/filmler/details. cshtml* içindeki Page yönergesini olarak `@page "{id:int?}"`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f8089-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="f8089-133">İçinde `public async Task<IActionResult> OnGetAsync(int? id)` bir kesme noktası ayarlayın ( *sayfalarda/filmlerde/details. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="f8089-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="f8089-134">          `https://localhost:5001/Movies/Details/` sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="f8089-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="f8089-135">`@page "{id:int}"` Yönergeyle, kesme noktası hiçbir şekilde vurılmaz.</span><span class="sxs-lookup"><span data-stu-id="f8089-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="f8089-136">Yönlendirme Altyapısı HTTP 404 döndürür.</span><span class="sxs-lookup"><span data-stu-id="f8089-136">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="f8089-137">Kullanarak `@page "{id:int?}"` ,`OnGetAsync` yöntemi döndürür`NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="f8089-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="f8089-138">Eşzamanlılık özel durum işlemeyi gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="f8089-138">Review concurrency exception handling</span></span>

<span data-ttu-id="f8089-139">*Pages/filmler/Edit. cshtml. cs* dosyasındaki yöntemigözdengeçirin:`OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="f8089-139">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="f8089-140">Önceki kod, bir istemci filmi sildiği ve diğer istemci filmle değişiklik yaptığı zaman eşzamanlılık özel durumlarını algılar.</span><span class="sxs-lookup"><span data-stu-id="f8089-140">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="f8089-141">`catch` Bloğu test etmek için:</span><span class="sxs-lookup"><span data-stu-id="f8089-141">To test the `catch` block:</span></span>

* <span data-ttu-id="f8089-142">Üzerinde bir kesme noktası ayarlayın`catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="f8089-142">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="f8089-143">Film için **Düzenle** ' yi seçin, değişiklikler yapın, ancak **Kaydet**' i girmeyin.</span><span class="sxs-lookup"><span data-stu-id="f8089-143">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="f8089-144">Başka bir tarayıcı penceresinde, aynı filmin **Sil** bağlantısını seçin ve ardından filmi silin.</span><span class="sxs-lookup"><span data-stu-id="f8089-144">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="f8089-145">Önceki tarayıcı penceresinde filmdeki değişiklikleri gönderin.</span><span class="sxs-lookup"><span data-stu-id="f8089-145">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="f8089-146">Üretim kodu eşzamanlılık çakışmalarını algılamak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="f8089-146">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="f8089-147">Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="f8089-147">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="f8089-148">Gönderme ve bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="f8089-148">Posting and binding review</span></span>

<span data-ttu-id="f8089-149">*Pages/filmler/Edit. cshtml. cs* dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="f8089-149">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

<span data-ttu-id="f8089-150">Filmler/düzenleme sayfasına HTTP GET isteği yapıldığında (örneğin, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="f8089-150">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="f8089-151">Yöntemi, filmi veritabanından getirir ve `Page` yöntemi döndürür. `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="f8089-151">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span>
* <span data-ttu-id="f8089-152">Yöntemi, *Pages/filmler/Edit. cshtml* Razor sayfasını işler. `Page`</span><span class="sxs-lookup"><span data-stu-id="f8089-152">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="f8089-153">*Pages/filmler/Edit. cshtml* dosyası, film modelinin sayfada kullanılabilir olmasını`@model RazorPagesMovie.Pages.Movies.EditModel`sağlayan model yönergesini () içerir.</span><span class="sxs-lookup"><span data-stu-id="f8089-153">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="f8089-154">Düzenleme formu filmdeki değerlerle birlikte görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8089-154">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="f8089-155">Filmler/Düzenle sayfası gönderildiğinde:</span><span class="sxs-lookup"><span data-stu-id="f8089-155">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="f8089-156">Sayfadaki form değerleri `Movie` özelliğine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f8089-156">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="f8089-157">Öznitelik `[BindProperty]` , [model bağlamayı](xref:mvc/models/model-binding)mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="f8089-157">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="f8089-158">Model durumunda hatalar varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), form gönderilen değerlerle yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8089-158">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is redisplayed with the submitted values.</span></span>
* <span data-ttu-id="f8089-159">Model hatası yoksa, film kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f8089-159">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="f8089-160">Razor sayfalarında Dizin, oluşturma ve silme gibi HTTP GET yöntemleri benzer bir düzende yer alır.</span><span class="sxs-lookup"><span data-stu-id="f8089-160">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="f8089-161">Razor Oluştur sayfasındaki `OnPostAsync` http post yöntemi, Razor düzenleme sayfasındaki `OnPostAsync` yönteme benzer bir düzen izler.</span><span class="sxs-lookup"><span data-stu-id="f8089-161">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8089-162">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f8089-162">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8089-163">[Öncekini Sonraki bir veritabanıyla](xref:tutorials/razor-pages/sql)
> çalışma[: Arama ekle](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="f8089-163">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f8089-164">Yapı iskelesi film uygulamasının iyi bir başlangıcı vardır ancak sunum ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="f8089-164">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="f8089-165">**ReleaseDate** **Yayın tarihi** (iki sözcük) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f8089-165">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Chrome 'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="f8089-167">Oluşturulan kodu Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="f8089-167">Update the generated code</span></span>

<span data-ttu-id="f8089-168">*Modeller/film. cs* dosyasını açın ve aşağıdaki kodda gösterilen vurgulanmış satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f8089-168">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="f8089-169">Veri `[Column(TypeName = "decimal(18, 2)")]` ek açıklaması, Entity Framework Core veritabanındaki para birimiyle `Price` doğru şekilde eşlenmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="f8089-169">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="f8089-170">Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="f8089-170">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="f8089-171">[Veri açıklamaları](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="f8089-171">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="f8089-172">[Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği bir alanın adı için (Bu durumda "ReleaseDate" yerine "Yayın tarihi") görüntüleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f8089-172">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="f8089-173">[DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih), bu nedenle alanda depolanan zaman bilgileri gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="f8089-173">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="f8089-174">Hedef URL 'yi görmek için sayfalara/filmlere gidin ve bir **düzenleme** bağlantısının üzerine gelin.</span><span class="sxs-lookup"><span data-stu-id="f8089-174">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Düzenleme bağlantısı üzerinde fare ile tarayıcı penceresi ve bağlantı URL 'si http://localhost:1234/Movies/Edit/5 gösteriliyor](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="f8089-176">**Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Sayfalar/filmler/Index. cshtml* dosyasındaki [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f8089-176">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="f8089-177">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="f8089-177">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="f8089-178">Yukarıdaki kodda `AnchorTagHelper` , Razor sayfasından (yol göreli `asp-page`olur) `href` , ve yol kimliği (`asp-route-id`) html öznitelik değerini dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f8089-178">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="f8089-179">Daha fazla bilgi için bkz. [Sayfalar Için URL oluşturma](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="f8089-179">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="f8089-180">Oluşturulan biçimlendirmeyi incelemek için sık kullandığınız tarayıcıdan **Görünüm kaynağını** kullanın.</span><span class="sxs-lookup"><span data-stu-id="f8089-180">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="f8089-181">Oluşturulan HTML 'nin bir bölümü aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="f8089-181">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="f8089-182">Dinamik olarak oluşturulan bağlantılar film kimliğini bir sorgu dizesiyle (örneğin, `?id=1` içinde `https://localhost:5001/Movies/Details?id=1`) iletir.</span><span class="sxs-lookup"><span data-stu-id="f8089-182">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="f8089-183">"{İd: int}" yol şablonunu kullanmak için Düzenle, Ayrıntılar ve Sil Razor Pages güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="f8089-183">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="f8089-184">Bu sayfaların her biri için Page yönergesini ' den `@page` ' e `@page "{id:int}"`değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f8089-184">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="f8089-185">Uygulamayı çalıştırın ve kaynağı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="f8089-185">Run the app and then view source.</span></span> <span data-ttu-id="f8089-186">Oluşturulan HTML, URL 'nin yol bölümüne KIMLIĞI ekler:</span><span class="sxs-lookup"><span data-stu-id="f8089-186">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="f8089-187">Tamsayıyı **içermeyen "** {id: int}" yol şablonuna sahip sayfaya yönelik bir Istek, HTTP 404 (bulunamadı) hatası döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="f8089-187">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="f8089-188">Örneğin, `http://localhost:5000/Movies/Details` bir 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="f8089-188">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="f8089-189">Kimliği isteğe bağlı yapmak için yol kısıtlamasına `?` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f8089-189">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="f8089-190">Davranışını test etmek için `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="f8089-190">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="f8089-191">*Pages/filmler/details. cshtml* içindeki Page yönergesini olarak `@page "{id:int?}"`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f8089-191">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="f8089-192">İçinde `public async Task<IActionResult> OnGetAsync(int? id)` bir kesme noktası ayarlayın ( *sayfalarda/filmlerde/details. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="f8089-192">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="f8089-193">          `https://localhost:5001/Movies/Details/` sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="f8089-193">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="f8089-194">`@page "{id:int}"` Yönergeyle, kesme noktası hiçbir şekilde vurılmaz.</span><span class="sxs-lookup"><span data-stu-id="f8089-194">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="f8089-195">Yönlendirme Altyapısı HTTP 404 döndürür.</span><span class="sxs-lookup"><span data-stu-id="f8089-195">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="f8089-196">Kullanarak `@page "{id:int?}"` ,`OnGetAsync` yöntemi döndürür`NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="f8089-196">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="f8089-197">Eşzamanlılık özel durum işlemeyi gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="f8089-197">Review concurrency exception handling</span></span>

<span data-ttu-id="f8089-198">*Pages/filmler/Edit. cshtml. cs* dosyasındaki yöntemigözdengeçirin:`OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="f8089-198">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="f8089-199">Önceki kod, bir istemci filmi sildiği ve diğer istemci filmle değişiklik yaptığı zaman eşzamanlılık özel durumlarını algılar.</span><span class="sxs-lookup"><span data-stu-id="f8089-199">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="f8089-200">`catch` Bloğu test etmek için:</span><span class="sxs-lookup"><span data-stu-id="f8089-200">To test the `catch` block:</span></span>

* <span data-ttu-id="f8089-201">Üzerinde bir kesme noktası ayarlayın`catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="f8089-201">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="f8089-202">Film için **Düzenle** ' yi seçin, değişiklikler yapın, ancak **Kaydet**' i girmeyin.</span><span class="sxs-lookup"><span data-stu-id="f8089-202">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="f8089-203">Başka bir tarayıcı penceresinde, aynı filmin **Sil** bağlantısını seçin ve ardından filmi silin.</span><span class="sxs-lookup"><span data-stu-id="f8089-203">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="f8089-204">Önceki tarayıcı penceresinde filmdeki değişiklikleri gönderin.</span><span class="sxs-lookup"><span data-stu-id="f8089-204">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="f8089-205">Üretim kodu eşzamanlılık çakışmalarını algılamak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="f8089-205">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="f8089-206">Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="f8089-206">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="f8089-207">Gönderme ve bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="f8089-207">Posting and binding review</span></span>

<span data-ttu-id="f8089-208">*Pages/filmler/Edit. cshtml. cs* dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="f8089-208">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="f8089-209">Filmler/düzenleme sayfasına HTTP GET isteği yapıldığında (örneğin, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="f8089-209">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="f8089-210">Yöntemi, filmi veritabanından getirir ve `Page` yöntemi döndürür. `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="f8089-210">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="f8089-211">Yöntemi, *Pages/filmler/Edit. cshtml* Razor sayfasını işler. `Page`</span><span class="sxs-lookup"><span data-stu-id="f8089-211">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="f8089-212">*Pages/filmler/Edit. cshtml* dosyası, film modelinin sayfada kullanılabilir olmasını`@model RazorPagesMovie.Pages.Movies.EditModel`sağlayan model yönergesini () içerir.</span><span class="sxs-lookup"><span data-stu-id="f8089-212">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="f8089-213">Düzenleme formu filmdeki değerlerle birlikte görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8089-213">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="f8089-214">Filmler/Düzenle sayfası gönderildiğinde:</span><span class="sxs-lookup"><span data-stu-id="f8089-214">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="f8089-215">Sayfadaki form değerleri `Movie` özelliğine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f8089-215">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="f8089-216">Öznitelik `[BindProperty]` , [model bağlamayı](xref:mvc/models/model-binding)mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="f8089-216">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="f8089-217">Model durumunda hatalar varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), form gönderilen değerlerle birlikte görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8089-217">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="f8089-218">Model hatası yoksa, film kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f8089-218">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="f8089-219">Razor sayfalarında Dizin, oluşturma ve silme gibi HTTP GET yöntemleri benzer bir düzende yer alır.</span><span class="sxs-lookup"><span data-stu-id="f8089-219">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="f8089-220">Razor Oluştur sayfasındaki `OnPostAsync` http post yöntemi, Razor düzenleme sayfasındaki `OnPostAsync` yönteme benzer bir düzen izler.</span><span class="sxs-lookup"><span data-stu-id="f8089-220">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="f8089-221">Arama sonraki öğreticiye eklenir.</span><span class="sxs-lookup"><span data-stu-id="f8089-221">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8089-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f8089-222">Additional resources</span></span>

* [<span data-ttu-id="f8089-223">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="f8089-223">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="f8089-224">[Öncekini Sonraki bir veritabanıyla](xref:tutorials/razor-pages/sql)
> çalışma[: Arama ekle](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="f8089-224">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end
