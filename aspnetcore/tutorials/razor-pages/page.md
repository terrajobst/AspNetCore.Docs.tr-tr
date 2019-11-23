---
title: ASP.NET Core Razor Pages scafkatlama
author: rick-anderson
description: Yapı iskelesi tarafından oluşturulan Razor Pages açıklar.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 594fd6186cc73aa054fc9a1478850fa01e481ef2
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034194"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor Pages scafkatlama

::: moniker range=">= aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a>Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları

*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

Razor Pages `PageModel`türetilir. Kurala göre `PageModel`türetilmiş sınıf `<PageName>Model`olarak adlandırılır. Oluşturucu, `RazorPagesMovieContext` sayfaya eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır. Tüm yapı iskelesi sayfaları bu düzene uyar. Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .

Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi, Razor sayfasına bir film listesi döndürür. `OnGetAsync` veya `OnGet`, sayfanın durumunu başlatmak için çağırılır. Bu durumda, `OnGetAsync` film listesini alır ve görüntüler.

`OnGet` `void` döndürdüğünde veya `OnGetAsync``Task`döndürürse, hiçbir dönüş açıklaması kullanılmaz. Dönüş türü `IActionResult` veya `Task<IActionResult>`olduğunda, return ifadesinin sağlanması gerekir. Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir. Bir `@` sembol sonrasında [Razor ayrılmış bir anahtar sözcük](xref:mvc/views/razor#razor-reserved-keywords)olduğunda, bu, ' a geçiş yapar C#.

### <a name="the-page-directive"></a>@page yönergesi

`@page` Razor yönergesi, dosyayı bir MVC eylemi yapar, bu da istekleri işleyebileceği anlamına gelir. `@page` sayfadaki ilk Razor yönergesi olmalıdır. `@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir. Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .

Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

`DisplayNameFor` HTML Yardımcısı, görünen adı belirlemede lambda ifadesinde başvurulan `Title` özelliğini inceler. Lambda ifadesi değerlendirilmek yerine incelenir. Bu, `model`, `model.Movie`veya `model.Movie[0]` `null` veya boş olduğunda herhangi bir erişim ihlali olmadığı anlamına gelir. Lambda ifadesi değerlendirildiğinde (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

<a name="md"></a>

### <a name="the-model-directive"></a>@model yönergesi

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` yönergesi, Razor sayfasına geçirilen modelin türünü belirtir. Yukarıdaki örnekte `@model` satırı, `PageModel`türetilmiş sınıfı Razor sayfası için kullanılabilir hale getirir. Model, `@Html.DisplayNameFor` ve sayfadaki [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayFor` kullanılır.

### <a name="the-layout-page"></a>Düzen sayfası

Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin. Her sayfada aynı menü düzeni gösterilir. Menü düzeni *sayfa/paylaşılan/_Layout. cshtml* dosyasında uygulanır. *Pages/Shared/_Layout. cshtml* dosyasını açın.

[Düzen](xref:mvc/views/layout) ŞABLONLARı, HTML kapsayıcı düzeninin şu şekilde olmasını sağlar:

* Tek bir yerde belirtildi.
* Sitede birden çok sayfada uygulandı.

`@RenderBody()` satırını bulun. `RenderBody`, tüm sayfaya özgü görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur. Örneğin, **Gizlilik** bağlantısını seçin ve *Sayfalar/gizlilik. cshtml* görünümü `RenderBody` yöntemi içinde işlenir.

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData ve Layout

*Pages/filmler/Index. cshtml* dosyasından aşağıdaki biçimlendirmeyi göz önünde bulundurun:

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Önceki vurgulanan biçimlendirme, Razor geçişi örneği olan bir örnektir C#. `{` ve `}` karakterler bir C# kod bloğunu kapsar.

`PageModel` temel sınıfı, verileri bir görünüme geçirmek için kullanılabilen bir `ViewData` Dictionary özelliği içerir. Nesneler, anahtar/değer düzeniyle `ViewData` sözlüğüne eklenir. Yukarıdaki örnekte, `"Title"` özelliği `ViewData` sözlüğüne eklenir.

`"Title"` özelliği *sayfa/paylaşılan/_Layout. cshtml* dosyasında kullanılır. Aşağıdaki biçimlendirme *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

Satır `@*Markup removed for brevity.*@` bir Razor açıklamadır. HTML yorumlarının (`<!-- -->`) aksine, Razor açıklamaları istemciye gönderilmez.

### <a name="update-the-layout"></a>Düzeni güncelleştirme

*Pages/Shared/_Layout. cshtml* dosyasındaki `<title>` öğesini **RazorPagesMovie**yerine **filmi** görüntüleyecek şekilde değiştirin.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

*Sayfa/paylaşılan/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Önceki öğeyi aşağıdaki biçimlendirmeyle değiştirin:

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro). Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir. `asp-page="/Movies/Index"` Tag Helper özniteliği ve değeri, `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. `asp-area` öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz. Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.

Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin. Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) dosyasına bakın.

Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**). Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.

> [!NOTE]
> Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir. Ondalık virgülden ekleme hakkında yönergeler için bkz. [GitHub sorunu 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .

`Layout` özelliği *Pages/_ViewStart. cshtml* dosyasında ayarlanır:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/paylaşılan/_Layout. cshtml* olarak ayarlar. Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .

### <a name="the-create-page-model"></a>Sayfa oluştur modeli

*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` yöntemi, sayfa için gereken tüm durumları başlatır. Oluşturma sayfasında başlatılacak durum yok, bu nedenle `Page` döndürülür. Öğreticide daha sonra, `OnGet` başlatma durumuna bir örnek gösterilir. `Page` yöntemi *Create. cshtml* sayfasını işleyen bir `PageResult` nesnesi oluşturur.

`Movie` özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, postalanan değerleri `Movie` modeline bağlar.

`OnPostAsync` yöntemi, sayfa form verileri gönderdiğinde çalıştırılır:

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

`<form method="post">` öğesi bir [form etiketi yardımcıdır](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.

Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler. Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.

[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliği için etiket başlığını ve `for` özniteliğini oluşturur.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.

`<form method="post">`gibi etiket yardımcıları hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).

## <a name="additional-resources"></a>Ek kaynaklar

> [!div class="step-by-step"]
> [Önceki: bir model ekleme](xref:tutorials/razor-pages/model) [sonraki
> : veritabanı](xref:tutorials/razor-pages/sql)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, [önceki öğreticide](xref:tutorials/razor-pages/model)scafkatlama tarafından oluşturulan Razor Pages incelenir.

[Görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) örnek.

## <a name="the-create-delete-details-and-edit-pages"></a>Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları

*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Razor Pages `PageModel`türetilir. Kurala göre `PageModel`türetilmiş sınıf `<PageName>Model`olarak adlandırılır. Oluşturucu, `RazorPagesMovieContext` sayfaya eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır. Tüm yapı iskelesi sayfaları bu düzene uyar. Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .

Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi, Razor sayfasına bir film listesi döndürür. `OnGetAsync` veya `OnGet` bir Razor sayfasında, sayfanın durumunu başlatmak için çağrılır. Bu durumda, `OnGetAsync` film listesini alır ve görüntüler.

`OnGet` `void` döndürdüğünde veya `OnGetAsync``Task`döndürürse, hiçbir dönüş yöntemi kullanılmaz. Dönüş türü `IActionResult` veya `Task<IActionResult>`olduğunda, return ifadesinin sağlanması gerekir. Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir. Bir `@` sembol sonrasında [Razor ayrılmış bir anahtar sözcük](xref:mvc/views/razor#razor-reserved-keywords)olduğunda, bu, ' a geçiş yapar C#.

`@page` Razor yönergesi, dosyayı bir MVC eylemine dönüştürür, bu da istekleri işleyebileceği anlamına gelir. `@page` sayfadaki ilk Razor yönergesi olmalıdır. `@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir. Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .

Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

`DisplayNameFor` HTML Yardımcısı, görünen adı belirlemede lambda ifadesinde başvurulan `Title` özelliğini inceler. Lambda ifadesi değerlendirilmek yerine incelenir. Bu, `model`, `model.Movie`veya `model.Movie[0]` `null` veya boş olduğunda herhangi bir erişim ihlali olmadığı anlamına gelir. Lambda ifadesi değerlendirildiğinde (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

<a name="md"></a>

### <a name="the-model-directive"></a>@model yönergesi

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` yönergesi, Razor sayfasına geçirilen modelin türünü belirtir. Yukarıdaki örnekte `@model` satırı, `PageModel`türetilmiş sınıfı Razor sayfası için kullanılabilir hale getirir. Model, `@Html.DisplayNameFor` ve sayfadaki [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayFor` kullanılır.

### <a name="the-layout-page"></a>Düzen sayfası

Menü bağlantılarını (**RazorPagesMovie**, **Home**ve **Gizlilik**) seçin. Her sayfada aynı menü düzeni gösterilir. Menü düzeni *sayfa/paylaşılan/_Layout. cshtml* dosyasında uygulanır. *Pages/Shared/_Layout. cshtml* dosyasını açın.

[Düzen](xref:mvc/views/layout) şablonları, sitenizin HTML kapsayıcı yerleşimini tek bir yerde belirtmenize ve sonra sitenizdeki birden çok sayfaya uygulamanıza olanak tanır. `@RenderBody()` satırını bulun. `RenderBody`, oluşturduğunuz tüm sayfaya özgü görünümlerin, Düzen sayfasında *kaydırılan* bir yer tutucudur. Örneğin, **Gizlilik** bağlantısını seçerseniz, **Sayfa/Gizlilik. cshtml** görünümü `RenderBody` yöntemi içinde işlenir.

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData ve Layout

*Pages/filmler/Index. cshtml* dosyasından aşağıdaki kodu göz önünde bulundurun:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Önceki vurgulanan kod, Razor geçişi örneği olan bir örnektir C#. `{` ve `}` karakterler bir C# kod bloğunu kapsar.

`PageModel` temel sınıfında, bir görünüme geçirmek istediğiniz verileri eklemek için kullanılabilecek bir `ViewData` Dictionary özelliği vardır. Bir anahtar/değer düzeniyle `ViewData` sözlüğüne nesne eklersiniz. Yukarıdaki örnekte, "title" özelliği `ViewData` sözlüğüne eklenir.

"Title" özelliği *sayfa/paylaşılan/_Layout. cshtml* dosyasında kullanılır. Aşağıdaki biçimlendirme *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

Satır `@*Markup removed for brevity.*@`, düzen dosyanızda görünmeyen bir Razor açıklamadır. HTML yorumlarının (`<!-- -->`) aksine, Razor açıklamaları istemciye gönderilmez.

### <a name="update-the-layout"></a>Düzeni güncelleştirme

*Pages/Shared/_Layout. cshtml* dosyasındaki `<title>` öğesini **RazorPagesMovie**yerine **filmi** görüntüleyecek şekilde değiştirin.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

*Sayfa/paylaşılan/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro). Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir. `asp-page="/Movies/Index"` Tag Helper özniteliği ve değeri, `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. `asp-area` öznitelik değeri boş olduğundan, alan bağlantıda kullanılmaz. Daha fazla bilgi için bkz. [alanlara](xref:mvc/controllers/areas) bakın.

Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin. Herhangi bir sorununuz varsa GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) dosyasına bakın.

Diğer bağlantıları test edin (**giriş**, **rpmovie**, **oluşturma**, **düzenleme**ve **silme**). Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır.

> [!NOTE]
> Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir. Bu GitHub, ondalık virgülden ekleme hakkında yönergeler için [4076 sorun](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .

`Layout` özelliği *Pages/_ViewStart. cshtml* dosyasında ayarlanır:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/paylaşılan/_Layout. cshtml* olarak ayarlar. Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .

### <a name="the-create-page-model"></a>Sayfa oluştur modeli

*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` yöntemi, sayfa için gereken tüm durumları başlatır. Oluşturma sayfasında başlatılacak durum yok, bu nedenle `Page` döndürülür. Öğreticide daha sonra `OnGet` yöntemi başlatma durumunu görürsünüz. `Page` yöntemi *Create. cshtml* sayfasını işleyen bir `PageResult` nesnesi oluşturur.

`Movie` özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, postalanan değerleri `Movie` modeline bağlar.

`OnPostAsync` yöntemi, sayfa form verileri gönderdiğinde çalıştırılır:

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

`<form method="post">`gibi etiket yardımcıları hakkında daha fazla bilgi için bkz. [ASP.NET Core etiket yardımcıları](xref:mvc/views/tag-helpers/intro).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Mac için Visual Studio etiket yardımcıları için kullanılan farklı kalın yazı tipinde `<form method="post">` etiketini görüntüler.

---

`<form method="post">` öğesi bir [form etiketi yardımcıdır](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi Yardımcısı, bir [antiforgery belirtecini](xref:security/anti-request-forgery)otomatik olarak içerir.

Yapı iskelesi altyapısı, modeldeki her alan için (KIMLIK hariç), aşağıdakine benzer Razor biçimlendirmesi oluşturur:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiketi yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hatalarını görüntüler. Doğrulama, bu serinin ilerleyen kısımlarında daha ayrıntılı bir şekilde ele alınmıştır.

[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`), `Title` özelliği için etiket başlığını ve `for` özniteliğini oluşturur.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`), [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> [Önceki: bir model ekleme](xref:tutorials/razor-pages/model) [sonraki
> : veritabanı](xref:tutorials/razor-pages/sql)

::: moniker-end
