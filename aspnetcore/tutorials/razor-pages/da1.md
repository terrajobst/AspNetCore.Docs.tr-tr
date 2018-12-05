---
title: ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme
author: rick-anderson
description: ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.date: 12/3/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: b88dcd12ee670eb2e0919bdb07b9b7556a5b80e7
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862414"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="96678-103">ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="96678-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="96678-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="96678-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="96678-105">İskele kurulmuş film uygulaması için iyi bir başlangıç olsa da, sunu ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="96678-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="96678-106">**ReleaseDate** olmalıdır **yayın tarihi** (iki kelimeye).</span><span class="sxs-lookup"><span data-stu-id="96678-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Chrome'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="96678-108">Oluşturulan kodu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="96678-108">Update the generated code</span></span>

<span data-ttu-id="96678-109">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterilen vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="96678-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

<span data-ttu-id="96678-110">`[Column(TypeName = "decimal(18, 2)")]` Veri ek açıklama sağlayan doğru eşlemek Entity Framework Core `Price` veritabanında para birimi.</span><span class="sxs-lookup"><span data-stu-id="96678-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="96678-111">Daha fazla bilgi için [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="96678-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="96678-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="96678-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="96678-113">[Görüntüleme](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) öznitelik adı (Bu durumda "ReleaseDate" yerine "yayın tarihi") bir alan için görüntülenecek öğeleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="96678-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="96678-114">[DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) öznitelik alanında depolanan saat bilgilerini görüntülenmediğini şekilde (tarih), veri türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="96678-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="96678-115">Sayfa/film bulun ve üzerine bir **Düzenle** hedef URL'ye görmek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="96678-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Düzenleme bağlantısını ve bir bağlantı üzerinde fare ile tarayıcı penceresinde URL'sini http://localhost:1234/Movies/Edit/5 gösterilir](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="96678-117">**Düzenle**, **ayrıntıları**, ve **Sil** tarafından oluşturulan bağlantıları [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/filmler / Index.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="96678-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="96678-118">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="96678-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="96678-119">Önceki kodda, `AnchorTagHelper` dinamik olarak HTML oluşturan `href` Razor (yol göreli) sayfasından, öznitelik değeri `asp-page`ve rota kimliği (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="96678-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="96678-120">Bkz: [sayfaları için URL üretimi](xref:razor-pages/index#url-generation-for-pages) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="96678-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="96678-121">Kullanım **kaynağı görüntüle** oluşturulan biçimlendirme incelemek için sık kullanılan tarayıcınızdan.</span><span class="sxs-lookup"><span data-stu-id="96678-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="96678-122">Oluşturulan HTML değerinin bir bölümü aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="96678-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="96678-123">Dinamik olarak oluşturulmuş bağlantıları film Kimliğine sahip bir sorgu dizesi geçirin (örneğin, `?id=1` içinde `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="96678-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="96678-124">Düzenle, Ayrıntılar ve Razor Sayfaları Sil "{kimliği: int}" rota şablonu kullanmak için güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="96678-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="96678-125">Her biri bu sayfaları için sayfa yönergesi değiştirme `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="96678-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="96678-126">Uygulamayı çalıştırın ve ardından kaynağı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="96678-126">Run the app and then view source.</span></span> <span data-ttu-id="96678-127">Oluşturulan HTML kimliği URL'nin yol kısmı ekler:</span><span class="sxs-lookup"><span data-stu-id="96678-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="96678-128">"{Kimliği: int}" rota şablonuyla yapan sayfasına bir istek **değil** dahil tamsayı HTTP 404 (bulunamadı) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="96678-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="96678-129">Örneğin, `http://localhost:5000/Movies/Details` 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="96678-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="96678-130">Kimliği isteğe bağlı yapmak için URL'ye `?` rota kısıtlaması için:</span><span class="sxs-lookup"><span data-stu-id="96678-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="96678-131">Bu davranışı sınamasını veya `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="96678-131">To test the behavior or `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="96678-132">Sayfa yönergesi kümesinde *Pages/Movies/Details.cshtml* için `@page "{id:int?}"`</span><span class="sxs-lookup"><span data-stu-id="96678-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`</span></span>
* <span data-ttu-id="96678-133">Bir kesme noktası kümesinde `public async Task<IActionResult> OnGetAsync(int? id)` (içinde *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="96678-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="96678-134">Gidin  `https://localhost:5001/Movies/Details/`</span><span class="sxs-lookup"><span data-stu-id="96678-134">Navigate to  `https://localhost:5001/Movies/Details/`</span></span>

<span data-ttu-id="96678-135">İle `@page "{id:int}"` yönergesi, kesme noktası hiçbir zaman ulaşılır.</span><span class="sxs-lookup"><span data-stu-id="96678-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="96678-136">Yönlendirme altyapısını, HTTP 404 döndürür.</span><span class="sxs-lookup"><span data-stu-id="96678-136">The routing engine return HTTP 404.</span></span> <span data-ttu-id="96678-137">Kullanarak `@page "{id:int?}"`, `OnGetAsync` yöntemi döndürür `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="96678-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

<span data-ttu-id="96678-138">Önerilmese de yazabilirsiniz delete yöntemini olarak:</span><span class="sxs-lookup"><span data-stu-id="96678-138">Although not recommended, you could write the the delete method as:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

<span data-ttu-id="96678-139">Yukarıdaki kod test edin:</span><span class="sxs-lookup"><span data-stu-id="96678-139">Test the preceding code:</span></span>

* <span data-ttu-id="96678-140">Bir silme bağlantısı seçin.</span><span class="sxs-lookup"><span data-stu-id="96678-140">Select a delete link.</span></span>
* <span data-ttu-id="96678-141">URL'deki kimliği kaldırın.</span><span class="sxs-lookup"><span data-stu-id="96678-141">Remove the ID from the URL.</span></span> <span data-ttu-id="96678-142">Örneğin, değiştirme `https://localhost:5001/Movies/Delete/8` için `https://localhost:5001/Movies/Delete`</span><span class="sxs-lookup"><span data-stu-id="96678-142">For example, change `https://localhost:5001/Movies/Delete/8` to `https://localhost:5001/Movies/Delete`</span></span>
* <span data-ttu-id="96678-143">Hata ayıklayıcı kodda adım adım.</span><span class="sxs-lookup"><span data-stu-id="96678-143">Step through the code in the debugger.</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="96678-144">Eşzamanlılık özel durum işleme gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="96678-144">Review concurrency exception handling</span></span>

<span data-ttu-id="96678-145">Gözden geçirme `OnPostAsync` yönteminde *Pages/Movies/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="96678-145">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="96678-146">Önceki kod, bir istemci film siler ve diğer istemci film değişiklikleri gönderir eşzamanlılık özel durumları algılar.</span><span class="sxs-lookup"><span data-stu-id="96678-146">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="96678-147">Test etmek için `catch` engelle:</span><span class="sxs-lookup"><span data-stu-id="96678-147">To test the `catch` block:</span></span>

* <span data-ttu-id="96678-148">Bir kesme noktası ayarlayın `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="96678-148">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="96678-149">Seçin **Düzenle** bir filmi için değişiklik, ancak girmeyin **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="96678-149">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="96678-150">Başka bir tarayıcı penceresinde seçin **Sil** bağlamak için aynı film ve film silin.</span><span class="sxs-lookup"><span data-stu-id="96678-150">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="96678-151">Önceki tarayıcı penceresinde film değişiklikleri yayınlayın.</span><span class="sxs-lookup"><span data-stu-id="96678-151">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="96678-152">Eşzamanlılık çakışmalarını algılamak üzere üretim kodu isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96678-152">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="96678-153">Bkz: [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="96678-153">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="96678-154">Gönderme ve bağlama gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="96678-154">Posting and binding review</span></span>

<span data-ttu-id="96678-155">İnceleme *Pages/Movies/Edit.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="96678-155">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="96678-156">Bir HTTP GET isteği filmler/düzenleme sayfası için yapılan olduğunda (örneğin, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="96678-156">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="96678-157">`OnGetAsync` Yöntemi film veritabanından veri getirir ve döndürür `Page` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="96678-157">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="96678-158">`Page` Yöntemi işler *Pages/Movies/Edit.cshtml* Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="96678-158">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="96678-159">*Pages/Movies/Edit.cshtml* dosyasını içeren model yönergesi (`@model RazorPagesMovie.Pages.Movies.EditModel`), hangi film modeli kullanıma sunar sayfasında.</span><span class="sxs-lookup"><span data-stu-id="96678-159">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="96678-160">Düzenleme formu film değerlerle görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="96678-160">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="96678-161">Filmler/düzenleme sayfası gönderildiğinde:</span><span class="sxs-lookup"><span data-stu-id="96678-161">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="96678-162">Form değerlerini sayfasında bağlı olan `Movie` özelliği.</span><span class="sxs-lookup"><span data-stu-id="96678-162">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="96678-163">`[BindProperty]` Özniteliği etkinleştirir [Model bağlama](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="96678-163">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="96678-164">Model durumuna bir hata varsa (örneğin, `ReleaseDate` tarihe dönüştürülür), formu yeniden gönderilen değerleri ile birlikte gönderilir.</span><span class="sxs-lookup"><span data-stu-id="96678-164">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="96678-165">Film modeli hata varsa, kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="96678-165">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="96678-166">Dizin oluşturma ve silme Razor sayfaları HTTP GET yöntemleri benzer bir desen izleyin.</span><span class="sxs-lookup"><span data-stu-id="96678-166">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="96678-167">HTTP POST `OnPostAsync` Razor sayfası oluşturma yöntemi için benzer bir desen izler `OnPostAsync` Razor sayfasını Düzenle yöntemi.</span><span class="sxs-lookup"><span data-stu-id="96678-167">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="96678-168">Sonraki öğreticide arama eklenir.</span><span class="sxs-lookup"><span data-stu-id="96678-168">Search is added in the next tutorial.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="96678-169">[Önceki: bir veritabanı ile çalışmaya](xref:tutorials/razor-pages/sql)
> [sonraki: arama Ekle](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="96678-169">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
