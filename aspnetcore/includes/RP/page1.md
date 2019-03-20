---
ms.openlocfilehash: 927f9528eb6ca4fac10ccee47aa255d848b7daaa
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265003"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET core'da iskeleli Razor sayfaları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, önceki öğreticide yapı iskelesi oluşturulmuş Razor sayfaları inceler.

[Görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) örnek.

## <a name="the-create-delete-details-and-edit-pages"></a>Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları.

İnceleme *Pages/Movies/Index.cshtml.cs* sayfa modeli:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

Razor sayfaları türetilir `PageModel`. Kural olarak, `PageModel`-türetilmiş sınıf adlı `<PageName>Model`. Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) eklemek için `MovieContext` sayfası. Bu düzen iskele kurulmuş tüm sayfaları izleyin. Bkz: [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) Entity Framework ile zaman uyumsuz programlamayı hakkında daha fazla bilgi için.

Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi için Razor sayfası filmler listesini döndürür. `OnGetAsync` veya `OnGet` durumunu başlatmak için bir Razor sayfası adı verilir. Bu durumda, `OnGetAsync` filmler listesini alır ve görüntüler.

Zaman `OnGet` döndürür `void` veya `OnGetAsync` döndürür`Task`, dönüş yöntem kullanılır. Dönüş türü olduğunda `IActionResult` veya `Task<IActionResult>`, bir return deyimi sağlanmalıdır. Örneğin, *Pages/Movies/Create.cshtml.cs* `OnPostAsync` yöntemi:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> İnceleme *Pages/Movies/Index.cshtml* Razor sayfası:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor HTML, C# veya Razor özgü biçimlendirme geçiş yapabilirsiniz. Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)Razor özgü biçimlendirme içinde geçiş, aksi takdirde, C# diline geçer.

`@page` Razor yönergesi, bir MVC eylemlere dosya yapar &mdash; istek işleyebileceği anlamına gelir. `@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır. `@page` Razor özgü biçimlendirme içinde geçiş bir örnektir. Bkz: [Razor sözdizimi](xref:mvc/views/razor#razor-syntax) daha fazla bilgi için.

Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği. Lambda ifadesi değerlendirilir inceledi yerine. Hiçbir erişim ihlali var. anlamına olduğunda `model`, `model.Movie`, veya `model.Movie[0]` olan `null` veya boş. Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

<a name="md"></a>

### <a name="the-model-directive"></a>@model Yönergesi

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Yönergesi için Razor sayfası geçirilen modelin türünü belirtir. Önceki örnekte `@model` satır yapar `PageModel`-türetilmiş sınıf için bir Razor sayfası kullanılabilir. Model kullanılır `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML Yardımcıları](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData ve düzeni

Aşağıdaki kodu göz önünde bulundurun:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Önceki vurgulanmış kodu, C# koduna geçiş Razor örneğidir. `{` Ve `}` karakter C# kod bloğunun içine alın.

`PageModel` Temel sınıfa sahip değil bir `ViewData` bir görünüme iletmek istediğiniz veri eklemek için kullanılan bir sözlük özelliği. Nesneleri eklemek `ViewData` bir anahtar/değer deseni kullanılarak sözlüğü. Yukarıdaki örnekte, "Title" özelliğini eklenen `ViewData` sözlüğü.

::: moniker range="= aspnetcore-2.0"

"Title" özelliğini kullanılan *Pages/Shared/_Layout.cshtml* dosya. İlk birkaç satırı aşağıdaki biçimlendirme gösterir *Pages/Shared/_Layout.cshtml* dosya.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

"Title" özelliğini kullanılan *Pages/Shared/_Layout.cshtml* dosya. İlk birkaç satırı aşağıdaki biçimlendirme gösterir *_Layout.cshtml* dosya.

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

Satır `@*Markup removed for brevity.*@` bir Razor açıklama. HTML Yorumlarını aksine (`<!-- -->`), Razor açıklama istemciye gönderilmez.

Uygulamayı çalıştırın ve bağlantıları projedeki test (**giriş**, **hakkında**, **kişi**, **Oluştur**, **Düzenle**, ve **Sil**). Her sayfada başlık, tarayıcı sekmesinde görebilirsiniz ayarlar. Bir sayfaya yer işareti başlık yer işareti için kullanılır. *Pages/Index.cshtml* ve *Pages/Movies/Index.cshtml* şu anda aynı başlığa sahip, ancak bunları farklı değerlere sahip değiştirebilirsiniz.

> [!NOTE]
> Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel ayarlar için (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz. Bu [GitHub sorunu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) ondalık virgülle ekleme hakkında yönergeler için.

`Layout` Özelliği ayarlandığında *Pages/_ViewStart.cshtml* dosyası:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Düzen dosyası önceki biçimlendirme ayarlar *Pages/Shared/_Layout.cshtml* altındaki tüm Razor dosyaları için *sayfaları* klasör. Bkz: [Düzen](xref:razor-pages/index#layout) daha fazla bilgi için.

### <a name="update-the-layout"></a>Düzeni güncelleştirme

Değişiklik `<title>` öğesinde *Pages/Shared/_Layout.cshtml* daha kısa bir dize kullanmak için dosya.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Aşağıdaki bağlantı öğe Bul *Pages/Shared/_Layout.cshtml* dosya.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```

Önceki öğeyle aşağıdaki biçimlendirme ile değiştirin.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Önceki yer işareti öğesi bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro). Bu durumda sahip [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). `asp-page="/Movies/Index"` Etiketi yardımcı öznitelik ve değer oluşturan bir bağlantı `/Movies/Index` Razor sayfası.

Değişikliklerinizi kaydedip tıklayarak uygulamayı test etme **RpMovie** bağlantı. Bkz: [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) github'da dosya.

### <a name="the-create-page-model"></a>Oluşturma sayfa modeli

İnceleme *Pages/Movies/Create.cshtml.cs* sayfa modeli:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end

`OnGet` Yöntemi sayfa için gerekli herhangi bir durum başlatır. Oluştur sayfasında başlatmak için bunu herhangi bir duruma sahip olmayan `Page` döndürülür. Daha sonra öğreticide gördüğünüz `OnGet` durumu Initialize yöntemi. `Page` Yöntemi oluşturur bir `PageResult` işleyen nesnesi *Create.cshtml* sayfası.

`Movie` Özelliği kullanan `[BindProperty]` kabul etmek için için öznitelik [model bağlama](xref:mvc/models/model-binding). Form oluştur form değerleri gönderdiğinde, ASP.NET Core çalışma zamanı gönderilen değerine bağlar `Movie` modeli.

`OnPostAsync` Sayfasının form verileri gönderildiğinde yöntemi çalıştırılır:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Model hataları varsa, form, gönderilen tüm form verileri ile birlikte yeniden görüntülenir. Form gönderildiğinde önce çoğu model hataları üzerinde istemci tarafı yakalanabilir. Model hatası örneği bir tarihe dönüştürülür tarih alanı için bir değer gönderiyor. İstemci tarafı doğrulama ve daha sonra öğreticide model doğrulama hakkında daha fazla konuşacağız.

Herhangi bir model hata varsa, verileri kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.

### <a name="the-create-razor-page"></a>Razor sayfası oluşturma

İnceleme *Pages/Movies/Create.cshtml* Razor sayfası dosyası:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).

![VS17 view of Create.cshtml page](page/_static/th.png)
-->
