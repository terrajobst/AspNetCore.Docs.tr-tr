---
title: "Oluşturulan sayfalarını güncelleştirme"
author: rick-anderson
description: "Oluşturulan sayfalar daha iyi ekranıyla güncelleştiriliyor."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 201e8d9c77d8e022bc56ffcf46456fada6fcfe25
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="4b2e1-103">Oluşturulan sayfalarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4b2e1-103">Updating the generated pages</span></span>

<span data-ttu-id="4b2e1-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4b2e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4b2e1-105">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="4b2e1-106">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).</span><span class="sxs-lookup"><span data-stu-id="4b2e1-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="4b2e1-108">Oluşturulan kod güncelleştir</span><span class="sxs-lookup"><span data-stu-id="4b2e1-108">Update the generated code</span></span>

<span data-ttu-id="4b2e1-109">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4b2e1-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="4b2e1-110">Bir kırmızı dalgalı satıra sağ tıklayın > ** Hızlı Eylemler ve yapan yeniden düzenlemeler **.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](da1/qa.png)

<span data-ttu-id="4b2e1-112">Seçin`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="4b2e1-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  <span data-ttu-id="4b2e1-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="4b2e1-115">Şu konulara değineceğiz [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-115">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="4b2e1-116">[Görüntülemek](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği ne bir alanın adını (Bu durumda "ReleaseDate" yerine "yayın tarihi") için görüntülenecek belirtir.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-116">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="4b2e1-117">[DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) öznitelik alanında depolanan saat bilgisi görüntülenmez şekilde (tarih), veri türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-117">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="4b2e1-118">Sayfa/filmlere göz atın ve üzerine gelerek bir **Düzenle** hedef URL görmek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-118">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Tarayıcı penceresini düzenleme bağlantısını ve bir bağlantı üzerinden fareyle http://localhost:1234/filmler/düzenleme/5 URL'sini gösterilir](da1/edit7.png)

<span data-ttu-id="4b2e1-120">**Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar tarafından üretilen [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/filmler / Index.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-120">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="4b2e1-121">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-121">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="4b2e1-122">Önceki kod `AnchorTagHelper` dinamik olarak HTML oluşturan `href` öznitelik değeri Razor (rota göreli) sayfasından `asp-page`ve rota kimliği (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="4b2e1-122">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="4b2e1-123">Bkz: [sayfaları için URL oluşturma](xref:mvc/razor-pages/index#url-generation-for-pages) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-123">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="4b2e1-124">Kullanım **kaynağı görüntüle** oluşturulan biçimlendirme incelemek için sık kullanılan tarayıcınızdan.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-124">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="4b2e1-125">Oluşturulan HTML bir bölümü aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4b2e1-125">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="4b2e1-126">Bir sorgu dizesi film Kimliğiyle dinamik olarak üretilen bağlantılar geçirin (örneğin, `http://localhost:5000/Movies/Details?id=2` ).</span><span class="sxs-lookup"><span data-stu-id="4b2e1-126">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="4b2e1-127">Düzenleme, Ayrıntılar ve Razor Sayfaları Sil "{kimliği: int}" rota şablonu kullanmak için güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-127">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="4b2e1-128">Sayfa yönergesi her bu sayfalarından değiştirme `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-128">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="4b2e1-129">Uygulamayı çalıştırın ve ardından kaynak görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-129">Run the app and then view source.</span></span> <span data-ttu-id="4b2e1-130">Oluşturulan HTML kimliği URL yolu bölümüne ekler:</span><span class="sxs-lookup"><span data-stu-id="4b2e1-130">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="4b2e1-131">İsteği yapan "{kimliği: int}" rota şablonuyla sayfasına **değil** dahil tamsayı (bulunamadı) HTTP 404 hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-131">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="4b2e1-132">Örneğin, `http://localhost:5000/Movies/Details` 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-132">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="4b2e1-133">Kimliği isteğe bağlı yapmak için ekleme `?` rota kısıtlaması için:</span><span class="sxs-lookup"><span data-stu-id="4b2e1-133">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="4b2e1-134">Güncelleştirme eşzamanlılık özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="4b2e1-134">Update concurrency exception handling</span></span>

<span data-ttu-id="4b2e1-135">Güncelleştirme `OnPostAsync` yönteminde *Pages/Movies/Edit.cshtml.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-135">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="4b2e1-136">Aşağıdaki vurgulanmış kodu değişiklikleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="4b2e1-136">The following highlighted code shows the changes:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

<span data-ttu-id="4b2e1-137">Önceki kod, yalnızca ilk eşzamanlı istemci film siler ve ikinci eşzamanlı istemci film değişiklikler yazılarını eşzamanlılık algılar.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-137">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="4b2e1-138">Test etmek için `catch` engelle:</span><span class="sxs-lookup"><span data-stu-id="4b2e1-138">To test the `catch` block:</span></span>

* <span data-ttu-id="4b2e1-139">Bir kesme noktası ayarlayın`catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="4b2e1-139">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="4b2e1-140">Bir filmi düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-140">Edit a movie.</span></span>
* <span data-ttu-id="4b2e1-141">Başka bir tarayıcı penceresinde seçin **silmek** bağlamak için aynı film ve film silin.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-141">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="4b2e1-142">Önceki tarayıcı penceresinde film değişiklikler gönderin.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-142">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="4b2e1-143">Üretim kodu genellikle eşzamanlılık çakışması iki algılamaz veya daha fazla istemciler eşzamanlı olarak güncelleştirilen bir kaydı.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-143">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="4b2e1-144">Bkz: [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-144">See [Handling concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="4b2e1-145">Gönderme ve bağlama gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="4b2e1-145">Posting and binding review</span></span>

<span data-ttu-id="4b2e1-146">İncelemek *Pages/Movies/Edit.cshtml.cs* dosyası:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="4b2e1-146">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="4b2e1-147">Ne zaman bir HTTP GET isteği yapıldığında filmler/Düzenle sayfasına (örneğin, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="4b2e1-147">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="4b2e1-148">`OnGetAsync` Yöntemi veritabanından film getirir ve döndürür `Page` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-148">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="4b2e1-149">`Page` Yöntemi işler *Pages/Movies/Edit.cshtml* Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-149">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="4b2e1-150">*Pages/Movies/Edit.cshtml* dosyasını içeren model yönergesi (`@model RazorPagesMovie.Pages.Movies.EditModel`), hangi kullanılabilir hale getirir film modeli sayfasında.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-150">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="4b2e1-151">Düzenleme formu film değerlerle görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-151">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="4b2e1-152">Ne zaman filmler/düzenleme sayfasını nakledilir:</span><span class="sxs-lookup"><span data-stu-id="4b2e1-152">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="4b2e1-153">Form değerleri sayfasında bağlı olan `Movie` özelliği.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-153">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="4b2e1-154">`[BindProperty]` Özniteliği etkinleştirir [bağlama Model](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="4b2e1-154">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="4b2e1-155">Model durumuna bir hata varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), formu yeniden gönderilen değerlerle nakledilir.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-155">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="4b2e1-156">Model hatalar varsa, film kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-156">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="4b2e1-157">Dizin oluşturma ve silme Razor sayfalarının HTTP GET yöntemlere benzer bir desen izleyin.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-157">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="4b2e1-158">HTTP POST `OnPostAsync` Razor Sayfa Oluştur yönteminde benzer bir desen izler `OnPostAsync` Razor sayfasını Düzenle yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-158">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="4b2e1-159">Arama sonraki öğreticide eklenir.</span><span class="sxs-lookup"><span data-stu-id="4b2e1-159">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4b2e1-160">[Önceki: SQL Server yerel veritabanı ile çalışma](xref:tutorials/razor-pages/sql)
[arama ekleme](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="4b2e1-160">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>