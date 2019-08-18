# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor Pages scafkatlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, önceki öğreticide scafkatlama tarafından oluşturulan Razor Pages incelenir.

[Görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) örnek.

## <a name="the-create-delete-details-and-edit-pages"></a>Oluşturma, silme, Ayrıntılar ve düzenleme sayfaları.

*Pages/filmler/Index. cshtml. cs* sayfa modelini inceleyin:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

Razor Pages, öğesinden `PageModel`türetilir. Kural gereği, `PageModel`-türetilmiş sınıfı çağrılır `<PageName>Model`. Oluşturucu, `MovieContext` sayfasına eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır. Tüm yapı iskelesi sayfaları bu düzene uyar. Entity Framework zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz kod](xref:data/ef-rp/intro#asynchronous-code) .

Sayfa için bir istek yapıldığında, `OnGetAsync` yöntemi Razor sayfasına bir film listesi döndürür. `OnGetAsync`ya `OnGet` da bir Razor sayfasında, sayfanın durumunu başlatmak için çağrılır. Bu durumda, `OnGetAsync` filmlerin bir listesini alır ve görüntüler.

Döndürüldüğünde `OnGet` veyadöndüğünde`Task`,returnyöntemikullanılmaz. `OnGetAsync` `void` Dönüş türü `IActionResult` veya `Task<IActionResult>`olduğunda, bir return ifadesinin sağlanması gerekir. Örneğin, *Pages/filmler/Create. cshtml. cs* `OnPostAsync` yöntemi:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>*Pages/filmler/Index. cshtml* Razor sayfasını inceleyin:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor, HTML 'den C# ya da Razor 'e özgü biçimlendirmeye geçiş yapabilir. Bir `@` sembolden sonra [Razor ayrılmış anahtar sözcüğü](xref:mvc/views/razor#razor-reserved-keywords)geldiğinde, Razor 'e özgü işaretlere geçiş yapar, aksi takdirde öğesine C#geçirir.

Razor yönergesi, dosyayı, istekleri işleyebileceği anlamına gelen &mdash; bir MVC eylemine dönüştürür. `@page` `@page`sayfada ilk Razor yönergesi olmalıdır. `@page`, Razor 'e özgü biçimlendirmeye geçme örneğidir. Daha fazla bilgi için bkz. [Razor söz dizimi](xref:mvc/views/razor#razor-syntax) .

Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

HTML Yardımcısı, görünen adı `Title` belirlemede lambda ifadesinde başvurulan özelliği inceler. `DisplayNameFor` Lambda ifadesi değerlendirilmek yerine incelenir. `model`Diğer bir deyişle, `model.Movie`,, veya `model.Movie[0]` `null` boş olduğunda erişim ihlali yoktur. Lambda ifadesi değerlendirildiğinde (örneğin, ile `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

<a name="md"></a>

### <a name="the-model-directive"></a>@model Yönergesi

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Yönerge, Razor sayfasına geçirilen modelin türünü belirtir. Yukarıdaki örnekte, `@model` satır türetilen sınıfı Razor sayfası için `PageModel`kullanılabilir hale getirir. Model, sayfadaki `@Html.DisplayNameFor` ve `@Html.DisplayFor` [HTML yardımcılarını](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) sayfasında kullanılır.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData ve Layout

Aşağıdaki kodu göz önünde bulundurun:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Önceki vurgulanan kod, Razor geçişi örneği olan bir örnektir C#. Ve karakterleri bir C# `{` `}`

Temel sınıfın, bir görünüme `ViewData` geçirmek istediğiniz verileri eklemek için kullanılabilecek bir Dictionary özelliği vardır. `PageModel` Bir anahtar/değer örüntüsünün kullanıldığı `ViewData` sözlüğe nesneler eklersiniz. Yukarıdaki örnekte, "title" özelliği `ViewData` sözlüğe eklenir.

::: moniker range="= aspnetcore-2.0"

"Title" özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır. Aşağıdaki biçimlendirme *sayfa/paylaşılan/_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

"Title" özelliği *Sayfalar/Shared/_Layout. cshtml* dosyasında kullanılır. Aşağıdaki biçimlendirme, *_Layout. cshtml* dosyasının ilk birkaç satırını gösterir.

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

Satır `@*Markup removed for brevity.*@` bir Razor açıklamadır. HTML yorumlarının (`<!-- -->`) aksine, Razor açıklamaları istemciye gönderilmez.

Uygulamayı çalıştırın ve projedeki bağlantıları test edin (**ana**, **hakkında**, **iletişim**, **oluşturma**, **düzenleme**ve **silme**). Her sayfada, tarayıcı sekmesinde görebileceğiniz başlık ayarlanır. Bir sayfada yer işareti eklediğinizde başlık, yer işareti için kullanılır. *Pages/index. cshtml* ve *Pages/filmlerini/index. cshtml* Şu anda aynı başlığa sahiptir, ancak bunları farklı değerlere sahip olacak şekilde değiştirebilirsiniz.

> [!NOTE]
> Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan. Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanızı globalize için adımlar uygulamanız gerekir. Bu GitHub, ondalık virgülden ekleme hakkında yönergeler için [4076 sorun](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .

Özelliği Pages */_viewstart. cshtml* dosyasında ayarlanır: `Layout`

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Yukarıdaki biçimlendirme düzen dosyasını *Sayfalar* klasörü altındaki tüm Razor dosyaları için *Sayfalar/Shared/_Layout. cshtml* olarak ayarlar. Daha fazla bilgi için bkz. [Düzen](xref:razor-pages/index#layout) .

### <a name="update-the-layout"></a>Düzeni güncelleştirme

*Pages/Shared/_Layout. cshtml* dosyasındaki öğeyidahakısabirdizekullanacakşekildedeğiştirin.`<title>`

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

*Pages/Shared/_Layout. cshtml* dosyasında aşağıdaki tutturucu öğeyi bulun.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```

Önceki öğeyi aşağıdaki biçimlendirme ile değiştirin.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Önceki tutturucu öğesi bir [etiket yardımcıdır](xref:mvc/views/tag-helpers/intro). Bu durumda, [bağlantı etiketi yardımcısının](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)olması gerekir. Etiket Yardımcısı özniteliği ve değeri `/Movies/Index` Razor sayfasına bir bağlantı oluşturur. `asp-page="/Movies/Index"`

Değişikliklerinizi kaydedin ve **Rpmovie** bağlantısına tıklayarak uygulamayı test edin. GitHub 'daki [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) dosyasına bakın.

### <a name="the-create-page-model"></a>Sayfa oluştur modeli

*Pages/filmler/Create. cshtml. cs* sayfa modelini inceleyin:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end

`OnGet` Yöntemi, sayfa için gereken tüm durumları başlatır. Oluşturma sayfasında, başlatılacak durum yoktur, bu nedenle `Page` döndürülür. Öğreticide daha sonra Yöntem başlatma `OnGet` durumunu görürsünüz. Yöntemi Create *. cshtml* sayfasını işleyen bir `PageResult` nesne oluşturur. `Page`

Özelliği, [model bağlamayı](xref:mvc/models/model-binding)kabul etmek için `[BindProperty]` özniteliğini kullanır. `Movie` Oluşturma formu form değerlerini gönderirse, ASP.NET Core çalışma zamanı, gönderilen değerleri `Movie` modele bağlar.

Bu `OnPostAsync` Yöntem, sayfa form verileri göndertiğinde çalıştırılır:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Herhangi bir model hatası varsa, form, gönderilen tüm form verileriyle birlikte yeniden görüntülenir. Form gönderilmeden önce çoğu model hatası istemci tarafında yakalanabilir. Bir model hatasına bir örnek, Date alanı için bir tarihe dönüştürülemeyen bir değer gönderme. Öğreticide daha sonra istemci tarafı doğrulama ve model doğrulama hakkında daha fazla bilgi edineceksiniz.

Model hatası yoksa, veriler kaydedilir ve tarayıcı dizin sayfasına yönlendirilir.

### <a name="the-create-razor-page"></a>Razor Oluştur sayfası

*Pages/filmler/Create. cshtml* Razor sayfa dosyasını inceleyin:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).

![VS17 view of Create.cshtml page](page/_static/th.png)
-->
