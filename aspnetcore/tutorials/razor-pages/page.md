---
title: ASP.NET Core Razor Pages scafkatlama
author: rick-anderson
description: Yapı iskelesi tarafından oluşturulan Razor Pages açıklar.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 939ed5c3cdf33d8d99712e3166d8d07d3bac719f
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334091"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor Pages scafkatlama

::: moniker range=">= aspnetcore-3.0"

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a>Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları

*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

Razor Pages `PageModel` ' dan türetilir. Kurala göre, @no__t -0-Derived sınıfı `<PageName>Model` olarak adlandırılır. Oluşturucu, sayfaya `RazorPagesMovieContext` eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır. Tüm yapı iskelesi sayfaları bu düzene uyar. Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .

Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür. `OnGetAsync` veya `OnGet`, sayfanın durumunu başlatmak için çağırılır. Bu durumda, `OnGetAsync` bir film listesi alır ve bunları görüntüler.

@No__t-0 `void` döndürürse veya `OnGetAsync`, @ no__t-3 döndürür, Return deyimleri kullanılmaz. Dönüş türü `IActionResult` veya `Task<IActionResult>` olduğunda, bir return ifadesinin sağlanması gerekir. Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir. @No__t-0 sembolünden sonra [Razor ayrılmış bir anahtar sözcük](xref:mvc/views/razor#razor-reserved-keywords)olduğunda, bu, ' a geçiş yapar C#.

### <a name="the-page-directive"></a>@No__t-0 yönergesi

@No__t-0 Razor yönergesi, dosyayı bir MVC eylemi yapar, bu da istekleri işleyebileceği anlamına gelir. `@page` bir sayfada ilk Razor yönergesi olmalıdır. `@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir. Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .

Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

@No__t-0 HTML Yardımcısı, görünen adı belirlemede lambda ifadesinde başvurulan `Title` özelliğini inceler. Lambda ifadesi değerlendirilmek yerine incelenir. Bu, `model`, `model.Movie` veya `model.Movie[0]` `null` veya boş olduğunda bir erişim ihlali olmadığı anlamına gelir. Lambda ifadesi değerlendirildiğinde (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

<a name="md"></a>

### <a name="the-model-directive"></a>@No__t-0 yönergesi

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

@No__t-0 yönergesi, Razor sayfasına geçirilen modelin türünü belirtir. Yukarıdaki örnekte `@model` satırı, @no__t -1-Derived sınıfını Razor sayfası için kullanılabilir hale getirir. Model, sayfada `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) kullanır.

### <a name="the-layout-page"></a>Düzen sayfası

Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin. Her sayfada aynı menü düzeni gösterilir. Menü düzeni *Sayfalar/Shared/_Layout. cshtml* dosyasında uygulanır. *Pages/Shared/_Layout. cshtml* dosyasını açın.

[Düzen](xref:mvc/views/layout) ŞABLONLARı, HTML kapsayıcı düzeninin şu şekilde olmasını sağlar:

* Tek bir yerde belirtildi.
* Sitede birden çok sayfada uygulandı.

@No__t-0 satırını bulun. `RenderBody`, sayfaya özgü tüm görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur. Örneğin, **Gizlilik** bağlantısını seçin ve *Sayfalar/gizlilik. cshtml* görünümü `RenderBody` yöntemi içinde işlenir.

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData ve Layout

*Pages/filmler/Index. cshtml* dosyasından aşağıdaki biçimlendirmeyi göz önünde bulundurun:

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Önceki vurgulanan biçimlendirme, Razor geçişi örneği olan bir örnektir C#. @No__t-0 ve `}` karakterleri bir C# kod bloğunu kapsar.

@No__t-0 taban sınıfı, verileri bir görünüme geçirmek için kullanılabilen bir `ViewData` sözlük özelliği içerir. Nesneler, anahtar/değer örüntüsünün kullanıldığı `ViewData` sözlüğüne eklenir. Yukarıdaki örnekte, `"Title"` özelliği `ViewData` sözlüğüne eklenir.

@No__t-0 özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır. Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

@No__t-0 ' ın bir Razor yorumu vardır. HTML yorumlarının (`<!-- -->`) aksine Razor açıklamaları istemciye gönderilmez.

### <a name="update-the-layout"></a>Düzeni güncelleştirme

*Pages/Shared/_Layout. cshtml* dosyasındaki `<title>` öğesini **RazorPagesMovie**yerine **filmi** görüntüleyecek şekilde değiştirin.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Önceki öğeyi aşağıdaki biçimlendirmeyle değiştirin:

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro). Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir. @No__t-0 etiket Yardımcısı özniteliği ve değeri, `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. @No__t-0 öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz. Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.

Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin. Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) dosyasına bakın.

Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**). Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.

> [!NOTE]
> @No__t-0 alanına ondalık virgüller giremeyebilirsiniz. Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir. Ondalık virgülden ekleme hakkında yönergeler için bkz. [GitHub sorunu 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .

@No__t-0 özelliği *Sayfalar/_ViewStart. cshtml* dosyasında ayarlanır:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar. Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .

### <a name="the-create-page-model"></a>Sayfa oluştur modeli

*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

@No__t-0 yöntemi, sayfa için gereken tüm durumları başlatır. Oluşturma sayfasında, başlatılacak durum yok, bu nedenle `Page` döndürülür. Öğreticide daha sonra, `OnGet` başlatma durumuna bir örnek gösterilir. @No__t-0 yöntemi *Create. cshtml* sayfasını işleyen bir `PageResult` nesnesi oluşturur.

@No__t-0 özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, postalanan değerleri `Movie` modeline bağlar.

@No__t-0 yöntemi, sayfa form verileri gönderdiğinde çalıştırılır:

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir. Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir. Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme. İstemci tarafı doğrulama ve model doğrulaması Öğreticinin ilerleyen kısımlarında ele alınmıştır.

Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.

### <a name="the-create-razor-page"></a>Razor Oluştur sayfası

*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle aşağıdaki etiketleri görüntüler:

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Create. cshtml sayfasının VS17 görünümü](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki etiket yardımcıları, önceki biçimlendirmede gösterilmektedir:

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle aşağıdaki etiketleri görüntüler:

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

@No__t-0 öğesi bir [form etiketi yardımcıdır](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.

Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler. Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.

[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliği için etiket başlığını ve `for` özniteliğini oluşturur.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.

@No__t-0 gibi etiket yardımcıları hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).

## <a name="additional-resources"></a>Ek kaynaklar

> [!div class="step-by-step"]
> [Önceki: model ekleme](xref:tutorials/razor-pages/model)
> [Sonraki: veritabanı](xref:tutorials/razor-pages/sql)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.

Örneği [görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) .

## <a name="the-create-delete-details-and-edit-pages"></a>Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları

*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Razor Pages `PageModel` ' dan türetilir. Kurala göre, @no__t -0-Derived sınıfı `<PageName>Model` olarak adlandırılır. Oluşturucu, sayfaya `RazorPagesMovieContext` eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır. Tüm yapı iskelesi sayfaları bu düzene uyar. Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .

Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür. `OnGetAsync` veya `OnGet` bir Razor sayfasında, sayfanın durumunu başlatmak için çağrılır. Bu durumda, `OnGetAsync` bir film listesi alır ve bunları görüntüler.

@No__t-0 `void` döndürürse veya `OnGetAsync`, @ no__t-3 döndürür, return yöntemi kullanılmaz. Dönüş türü `IActionResult` veya `Task<IActionResult>` olduğunda, bir return ifadesinin sağlanması gerekir. Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir. @No__t-0 sembolünden sonra [Razor ayrılmış bir anahtar sözcük](xref:mvc/views/razor#razor-reserved-keywords)olduğunda, bu, ' a geçiş yapar C#.

@No__t-0 Razor yönergesi, dosyayı bir MVC eylemine dönüştürür, bu da istekleri işleyebileceği anlamına gelir. `@page` bir sayfada ilk Razor yönergesi olmalıdır. `@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir. Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .

Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

@No__t-0 HTML Yardımcısı, görünen adı belirlemede lambda ifadesinde başvurulan `Title` özelliğini inceler. Lambda ifadesi değerlendirilmek yerine incelenir. Bu, `model`, `model.Movie` veya `model.Movie[0]` `null` ya da boş olduğunda bir erişim ihlali olmadığı anlamına gelir. Lambda ifadesi değerlendirildiğinde (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

<a name="md"></a>

### <a name="the-model-directive"></a>@No__t-0 yönergesi

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

@No__t-0 yönergesi, Razor sayfasına geçirilen modelin türünü belirtir. Yukarıdaki örnekte `@model` satırı, @no__t -1-Derived sınıfını Razor sayfası için kullanılabilir hale getirir. Model, sayfada `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) kullanır.

### <a name="the-layout-page"></a>Düzen sayfası

Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin. Her sayfada aynı menü düzeni gösterilir. Menü düzeni *Sayfalar/Shared/_Layout. cshtml* dosyasında uygulanır. *Pages/Shared/_Layout. cshtml* dosyasını açın.

[Düzen](xref:mvc/views/layout) şablonları, sitenizin HTML kapsayıcı yerleşimini tek bir yerde belirtmenize ve sonra sitenizdeki birden çok sayfaya uygulamanıza olanak tanır. @No__t-0 satırını bulun. `RenderBody`, oluşturduğunuz tüm sayfaya özgü görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur. Örneğin, **Gizlilik** bağlantısını seçerseniz, **Sayfalar/gizlilik. cshtml** görünümü `RenderBody` yöntemi içinde işlenir.

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData ve Layout

*Pages/filmler/Index. cshtml* dosyasından aşağıdaki kodu göz önünde bulundurun:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Önceki vurgulanan kod, Razor geçişi örneği olan bir örnektir C#. @No__t-0 ve `}` karakterleri bir C# kod bloğunu kapsar.

@No__t-0 taban sınıfı, bir görünüme geçirmek istediğiniz verileri eklemek için kullanılabilecek bir `ViewData` sözlük özelliğine sahiptir. Bir anahtar/değer düzeniyle `ViewData` sözlüğüne nesneler eklersiniz. Yukarıdaki örnekte, "title" özelliği `ViewData` sözlüğüne eklenir.

"Title" özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır. Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

@No__t-0 satırı, düzen dosyanızda görünmeyen bir Razor açıklamadır. HTML yorumlarının (`<!-- -->`) aksine Razor açıklamaları istemciye gönderilmez.

### <a name="update-the-layout"></a>Düzeni güncelleştirme

*Pages/Shared/_Layout. cshtml* dosyasındaki `<title>` öğesini **RazorPagesMovie**yerine **filmi** görüntüleyecek şekilde değiştirin.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro). Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir. @No__t-0 etiket Yardımcısı özniteliği ve değeri, `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. @No__t-0 öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz. Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.

Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin. Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) dosyasına bakın.

Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**). Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.

> [!NOTE]
> @No__t-0 alanına ondalık virgüller giremeyebilirsiniz. Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir. Bu GitHub, ondalık virgülden ekleme hakkında yönergeler için [4076 sorun](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .

@No__t-0 özelliği *Sayfalar/_ViewStart. cshtml* dosyasında ayarlanır:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar. Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .

### <a name="the-create-page-model"></a>Sayfa oluştur modeli

*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

@No__t-0 yöntemi, sayfa için gereken tüm durumları başlatır. Oluşturma sayfasında, başlatılacak durum yok, bu nedenle `Page` döndürülür. Öğreticide daha sonra `OnGet` yöntemi başlatma durumunu görürsünüz. @No__t-0 yöntemi *Create. cshtml* sayfasını işleyen bir `PageResult` nesnesi oluşturur.

@No__t-0 özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, postalanan değerleri `Movie` modeline bağlar.

@No__t-0 yöntemi, sayfa form verileri gönderdiğinde çalıştırılır:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir. Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir. Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme. İstemci tarafı doğrulama ve model doğrulaması Öğreticinin ilerleyen kısımlarında ele alınmıştır.

Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.

### <a name="the-create-razor-page"></a>Razor Oluştur sayfası

*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipiyle `<form method="post">` etiketini görüntüler:

![Create. cshtml sayfasının VS17 görünümü](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

@No__t-0 gibi etiket yardımcıları hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Mac için Visual Studio, etiket yardımcıları için kullanılan farklı kalın yazı tipinde `<form method="post">` etiketini görüntüler.

---

@No__t-0 öğesi bir [form etiketi yardımcıdır](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.

Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler. Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.

[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliği için etiket başlığını ve `for` özniteliğini oluşturur.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> [Önceki: model ekleme](xref:tutorials/razor-pages/model)
> [Sonraki: veritabanı](xref:tutorials/razor-pages/sql)

::: moniker-end
