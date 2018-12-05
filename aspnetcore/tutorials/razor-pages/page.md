---
title: ASP.NET core'da iskeleli Razor sayfaları
author: rick-anderson
description: Yapı iskelesi tarafından oluşturulan Razor sayfaları açıklar.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: acfc446732803c67714943fe3e5b7a31055ebcd7
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862011"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET core'da iskeleli Razor sayfaları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, önceki öğreticide yapı iskelesi oluşturulmuş Razor sayfaları inceler.

[Görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) örnek.

## <a name="the-create-delete-details-and-edit-pages"></a>Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları

İnceleme *Pages/Movies/Index.cshtml.cs* sayfa modeli:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Razor sayfaları türetilir `PageModel`. Kural olarak, `PageModel`-türetilmiş sınıf adlı `<PageName>Model`. Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) eklemek için `RazorPagesMovieContext` sayfası. Bu düzen iskele kurulmuş tüm sayfaları izleyin. Bkz: [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) Entity Framework ile zaman uyumsuz programlamayı hakkında daha fazla bilgi için.

Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi için Razor sayfası filmler listesini döndürür. `OnGetAsync` veya `OnGet` durumunu başlatmak için bir Razor sayfası adı verilir. Bu durumda, `OnGetAsync` filmler listesini alır ve görüntüler.

Zaman `OnGet` döndürür `void` veya `OnGetAsync` döndürür`Task`, dönüş yöntem kullanılır. Dönüş türü olduğunda `IActionResult` veya `Task<IActionResult>`, bir return deyimi sağlanmalıdır. Örneğin, *Pages/Movies/Create.cshtml.cs* `OnPostAsync` yöntemi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> İnceleme *Pages/Movies/Index.cshtml* Razor sayfası:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor HTML, C# veya Razor özgü biçimlendirme geçiş yapabilirsiniz. Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)Razor özgü biçimlendirme içinde geçiş, aksi takdirde, C# diline geçer.

`@page` Razor yönergesi, istek işleyebileceği anlamına gelir. bir MVC eyleme dosya yapar. `@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır. `@page` Razor özgü biçimlendirme içinde geçiş bir örnektir. Bkz: [Razor sözdizimi](xref:mvc/views/razor#razor-syntax) daha fazla bilgi için.

Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği. Lambda ifadesi değerlendirilir inceledi yerine. Hiçbir erişim ihlali var. anlamına olduğunda `model`, `model.Movie`, veya `model.Movie[0]` olan `null` veya boş. Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

<a name="md"></a>
### <a name="the-model-directive"></a>@model Yönergesi

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Yönergesi için Razor sayfası geçirilen modelin türünü belirtir. Önceki örnekte `@model` satır yapar `PageModel`-türetilmiş sınıf için bir Razor sayfası kullanılabilir. Model kullanılır `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML Yardımcıları](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında.

<a name="vd"></a>
### <a name="viewdata-and-layout"></a>ViewData ve düzeni

Aşağıdaki kodu düşünün *Pages/Movies/Index.cshtml* dosyası:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Önceki vurgulanmış kodu, C# koduna geçiş Razor örneğidir. `{` Ve `}` karakter C# kod bloğunun içine alın.

`PageModel` Temel sınıfa sahip değil bir `ViewData` bir görünüme iletmek istediğiniz veri eklemek için kullanılan bir sözlük özelliği. Nesneleri eklemek `ViewData` bir anahtar/değer deseni kullanılarak sözlüğü. Yukarıdaki örnekte, "Title" özelliğini eklenen `ViewData` sözlüğü. 

"Title" özelliğini kullanılan *Pages/Shared/_Layout.cshtml* dosya. İlk birkaç satırı aşağıdaki biçimlendirme gösterir *_Layout.cshtml* dosya.

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

Satır `@*Markup removed for brevity.*@` Düzen dosyanızda görünmez bir Razor açıklama. HTML Yorumlarını aksine (`<!-- -->`), Razor açıklama istemciye gönderilmez.

### <a name="update-the-layout"></a>Düzeni güncelleştirme

Değişiklik `<title>` öğesinde *Pages/Shared/_Layout.cshtml* dosya görünen **film** yerine **RazorPagesMovie**.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


Aşağıdaki bağlantı öğe Bul *Pages/Shared/_Layout.cshtml* dosya.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Önceki öğeyle aşağıdaki biçimlendirme ile değiştirin.

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Önceki yer işareti öğesi bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro). Bu durumda sahip [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). `asp-page="/Movies/Index"` Etiketi yardımcı öznitelik ve değer oluşturan bir bağlantı `/Movies/Index` Razor sayfası. `asp-area` Öznitelik değeri olduğundan boş alanı bağlantıdaki kullanılmaz. Bkz: [alanları](xref:mvc/controllers/areas) daha fazla bilgi için.

Değişikliklerinizi kaydedip tıklayarak uygulamayı test etme **RpMovie** bağlantı. Bkz: [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) herhangi bir sorun varsa GitHub dosya.

Test diğer bağlantılardan (**giriş**, **RpMovie**, **Oluştur**, **Düzenle**, ve **Sil**). Her sayfada başlık, tarayıcı sekmesinde görebilirsiniz ayarlar. Bir sayfaya yer işareti başlık yer işareti için kullanılır. *Pages/Index.cshtml* ve *Pages/Movies/Index.cshtml* şu anda aynı başlığa sahip, ancak bunları farklı değerlere sahip değiştirebilirsiniz.

> [!NOTE]
> Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel ayarlar için (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz. Bu [GitHub sorunu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) ondalık virgülle ekleme hakkında yönergeler için.

`Layout` Özelliği ayarlandığında *Pages/_ViewStart.cshtml* dosyası:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

Düzen dosyası önceki biçimlendirme ayarlar *Pages/Shared/_Layout.cshtml* altındaki tüm Razor dosyaları için *sayfaları* klasör. Bkz: [Düzen](xref:razor-pages/index#layout) daha fazla bilgi için.

### <a name="the-create-page-model"></a>Oluşturma sayfa modeli

İnceleme *Pages/Movies/Create.cshtml.cs* sayfa modeli:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` Yöntemi sayfa için gerekli herhangi bir durum başlatır. Oluştur sayfasında başlatmak için bunu herhangi bir duruma sahip olmayan `Page` döndürülür. Daha sonra öğreticide gördüğünüz `OnGet` durumu Initialize yöntemi. `Page` Yöntemi oluşturur bir `PageResult` işleyen nesnesi *Create.cshtml* sayfası.

`Movie` Özelliği kullanan `[BindProperty]` kabul etmek için için öznitelik [model bağlama](xref:mvc/models/model-binding). Form oluştur form değerleri gönderdiğinde, ASP.NET Core çalışma zamanı gönderilen değerine bağlar `Movie` modeli.

`OnPostAsync` Sayfasının form verileri gönderildiğinde yöntemi çalıştırılır:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Model hataları varsa, form, gönderilen tüm form verileri ile birlikte yeniden görüntülenir. Form gönderildiğinde önce çoğu model hataları üzerinde istemci tarafı yakalanabilir. Model hatası örneği bir tarihe dönüştürülür tarih alanı için bir değer gönderiyor. İstemci tarafı doğrulama ve model doğrulama öğreticinin ilerleyen bölümlerinde ele alınmıştır.

Herhangi bir model hata varsa, verileri kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.

### <a name="the-create-razor-page"></a>Razor sayfası oluşturma

İnceleme *Pages/Movies/Create.cshtml* Razor sayfası dosyası:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio görüntüler `<form method="post">` etiket Yardımcıları için kullanılan ayırıcı kalın yazı tipinde etiketi:

![Create.cshtml sayfasının görünümünü VS17](page/_static/th.png)
<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Etiket yardımcıları gibi daha fazla bilgi için `<form method="post">`, bkz: [etiket Yardımcıları ASP.NET core'da](xref:mvc/views/tag-helpers/intro).

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Visual Studio Mac görüntüler için `<form method="post">` etiket Yardımcıları için kullanılan ayırıcı kalın yazı tipinde etiketi.

---  
<!-- End of VS tabs -->

`<form method="post">` Öğesi bir [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi Yardımcısı otomatik olarak içeren bir [antiforgery belirteci](xref:security/anti-request-forgery).

Yapı iskelesi altyapısı Razor işaretlemesi için her bir alan (ID dışında) modelinde aşağıdakine benzer oluşturur:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiket Yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve ` <span asp-validation-for`) doğrulama hataları görüntüleyin. Doğrulama Bu seriyi ilerleyen bölümlerinde daha ayrıntılı ele alınmıştır.

[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) etiketi başlığını oluşturur ve `for` özniteliğini `Title` özelliği.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur.

> [!div class="step-by-step"]
> [Önceki: model ekleme](xref:tutorials/razor-pages/model)
> [sonraki: veri tabanı](xref:tutorials/razor-pages/sql)
