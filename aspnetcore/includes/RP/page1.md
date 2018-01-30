# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET Core kurulmuş Razor sayfalarında

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici Razor önceki öğreticideki yapı iskelesi tarafından oluşturulan sayfaları inceler. 

[Görüntülemek veya karşıdan](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) örnek.

## <a name="the-create-delete-details-and-edit-pages"></a>Oluştur, Sil, Ayrıntılar ve düzenleme sayfaları.

İncelemek *Pages/Movies/Index.cshtml.cs* sayfa modeli:[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Razor sayfalarının türetilir `PageModel`. Kural tarafından `PageModel`-türetilmiş sınıf çağrılır `<PageName>Model`. Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) eklemek için `MovieContext` sayfası. Bu yol kurulmuş tüm sayfaları izler. Bkz: [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) Entity Framework zaman uyumsuz programing hakkında daha fazla bilgi için.

Sayfa için bir istek yapıldığında `OnGetAsync` yöntemi Razor sayfasına filmler listesini döndürür. `OnGetAsync`veya `OnGet` sayfası için durum başlatmak için bir Razor sayfasında olarak adlandırılır. Bu durumda, `OnGetAsync` filmler listesini alır ve bunları görüntüler. 

Zaman `OnGet` döndürür `void` veya `OnGetAsync` döndürür`Task`, hiçbir dönüş yöntemi kullanılır. Dönüş türü olduğunda `IActionResult` veya `Task<IActionResult>`, bir dönüş ifadesi sağlanmalıdır. Örneğin, *Pages/Movies/Create.cshtml.cs* `OnPostAsync` yöntemi:

<!-- TODO - replace with snippet
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
İncelemek *Pages/Movies/Index.cshtml* Razor sayfasını:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

HTML Razor C# veya Razor özgü biçimlendirme geçiş yapabilir. Zaman bir `@` simgesi tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords), Razor özgü biçimlendirme geçişleri, aksi takdirde C# diline geçiş.

`@page` Razor yönergesi yapar dosya MVC eyleme &mdash; istek işleyebileceği anlamına gelir. `@page`ilk Razor yönergesi bir sayfa üzerinde olmalıdır. `@page`Razor özgü biçimlendirme geçiş, bir örnek verilmiştir. Bkz: [Razor sözdizimi](xref:mvc/views/razor#razor-syntax) daha fazla bilgi için.

Aşağıdaki HTML Yardımcısı kullanılan lambda ifadesi inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adı belirlemek için lambda ifadesinde başvurulan özelliği. Lambda ifadesi hesaplanan Denetlenmekte yerine. Hiçbir erişim ihlali var. anlamına zaman `model`, `model.Movie`, veya `model.Movie[0]` olan `null` veya boş. Lambda ifadesi ne zaman değerlendirildiği (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri olarak değerlendirilir.

<a name="md"></a>
### <a name="the-model-directive"></a>@model Yönergesi

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Yönergesi Razor sayfasına iletilen model türünü belirtir. Önceki örnekte `@model` satır yapar `PageModel`-türetilmiş sınıf Razor sayfasına kullanılabilir. Model kullanılan `@Html.DisplayNameFor` ve `@Html.DisplayName` [HTML Yardımcıları](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData ve düzeni

Aşağıdaki kod göz önünde bulundurun:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

Önceki vurgulanmış kodu koddan C# diline Razor örneğidir. `{` Ve `}` karakterleri bir C# kod bloğunun içine alın.

`PageModel` Taban sınıfının bir `ViewData` bir görünüme iletmek istediğiniz verileri eklemek için kullanılan sözlüğü özelliği. Nesneleri eklemek `ViewData` bir anahtar/değer modeli kullanarak sözlük. Önceki örnekte, "Title" özelliği eklenen `ViewData` sözlük. "Title" özellik kullanılır *Pages/_Layout.cshtml* dosya. Aşağıdaki biçimlendirmede ilk birkaç satırlık gösterir *Pages/_Layout.cshtml* dosya.

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

Satır `@*Markup removed for brevity.*@` bir Razor açıklama. HTML açıklamaları aksine (`<!-- -->`), Razor açıklama istemciye gönderilmez.

Uygulamayı çalıştırın ve bağlantıları projesinde test (**giriş**, **hakkında**, **kişi**, **oluşturma**, **Düzenle**, ve **silmek**). Her bir sayfada bir tarayıcı sekmesinde görebilirsiniz başlık ayarlar. Bir sayfaya yer işareti başlığı için yer işareti kullanılır. *Pages/Index.cshtml* ve *Pages/Movies/Index.cshtml* şu anda aynı ada sahip, ancak farklı değerlere sahip değiştirebilirsiniz.

`Layout` Özelliği ayarlanmış *Pages/_ViewStart.cshtml* dosyası:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Düzen dosyasını önceki biçimlendirme ayarlar *Pages/_Layout.cshtml* altındaki tüm Razor dosyaları için *sayfaları* klasör. Bkz: [düzeni](xref:mvc/razor-pages/index#layout) daha fazla bilgi için.

### <a name="update-the-layout"></a>Güncelleştirme düzeni

Değişiklik `<title>` öğesinde *Pages/_Layout.cshtml* daha kısa bir dize kullanmak üzere bir dosya.

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Aşağıdaki bağlantı öğesinde Bul *Pages/_Layout.cshtml* dosya.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Önceki bağlantı öğesi bir [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro). Bu durumda, sahip [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). `asp-page="/Movies/Index"` Etiketi yardımcı öznitelik ve değeri bir bağlantı oluşturur `/Movies/Index` Razor sayfası.

Yaptığınız değişiklikleri kaydedin ve uygulamayı tıklayarak test **RpMovie** bağlantı. Bkz: [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) GitHub dosyasında.

### <a name="the-create-page-model"></a>Oluştur sayfası modeli

İncelemek *Pages/Movies/Create.cshtml.cs* sayfa modeli:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` Yöntemi sayfa için gerekli herhangi bir durum başlatır. Oluştur sayfası başlatmak için herhangi bir durum yok. `Page` Yöntemi oluşturur bir `PageResult` işleyen nesnesi *Create.cshtml* sayfası.

`Movie` Özelliğini kullanan `[BindProperty]` için katılımı için öznitelik [model bağlama](xref:mvc/models/model-binding). Form oluştur form değerleri gönderdiğinde, ASP.NET çekirdeği çalışma zamanı gönderilen değerlerden bağlar `Movie` modeli.

`OnPostAsync` Yöntemi sayfa form verileri gönderdiğinde çalıştırın:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Model hatalar varsa, formu, gönderilen tüm form verileri ile birlikte yeniden görüntülenir. Formun gönderilen önce model hataların çoğu istemci tarafında yakalanabilir. Model hatası örneği bir tarihe dönüştürülemez tarih alanı için bir değer gönderme. İstemci tarafı doğrulama ve daha sonra öğreticide model doğrulama hakkında daha fazla biz konuşun.

Model hatalar varsa, veri kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.

### <a name="the-create-razor-page"></a>Razor sayfası oluşturma

İncelemek *Pages/Movies/Create.cshtml* Razor sayfa dosyası:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
