Şu konulara değineceğiz [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide. [Görüntülemek](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği ne bir alanın adını (Bu durumda "ReleaseDate" yerine "yayın tarihi") için görüntülenecek belirtir. [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) öznitelik alanında depolanan saat bilgisi görüntülenmiyor şekilde (tarih), veri türünü belirtir.

Sayfa/filmlere göz atın ve üzerine gelerek bir **Düzenle** hedef URL görmek için bağlantı.

![Tarayıcı penceresini düzenleme bağlantısını ve bir bağlantı üzerinden fareyle http://localhost:1234/filmler/düzenleme/5 URL'sini gösterilir](../../tutorials/razor-pages/da1/edit7.png)

**Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar tarafından üretilen [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/filmler / Index.cshtml* dosya.

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Önceki kod `AnchorTagHelper` dinamik olarak HTML oluşturan `href` öznitelik değeri Razor (rota göreli) sayfasından `asp-page`ve rota kimliği (`asp-route-id`). Bkz: [sayfaları için URL oluşturma](xref:mvc/razor-pages/index#url-generation-for-pages) daha fazla bilgi için.

Kullanım **kaynağı görüntüle** oluşturulan biçimlendirme incelemek için sık kullanılan tarayıcınızdan. Oluşturulan HTML bir bölümü aşağıda verilmiştir:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Bir sorgu dizesi film Kimliğiyle dinamik olarak üretilen bağlantılar geçirin (örneğin, `http://localhost:5000/Movies/Details?id=2` ). 

Düzenleme, Ayrıntılar ve Razor Sayfaları Sil "{kimliği: int}" rota şablonu kullanmak için güncelleştirin. Sayfa yönergesi her bu sayfalarından değiştirme `@page` için `@page "{id:int}"`. Uygulamayı çalıştırın ve ardından kaynak görüntüleyin. Oluşturulan HTML kimliği URL yolu bölümüne ekler:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

İsteği yapan "{kimliği: int}" rota şablonuyla sayfasına **değil** dahil tamsayı (bulunamadı) HTTP 404 hata döndürür. Örneğin, `http://localhost:5000/Movies/Details` 404 hatası döndürür. Kimliği isteğe bağlı yapmak için ekleme `?` rota kısıtlaması için:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Güncelleştirme eşzamanlılık özel durum işleme

Güncelleştirme `OnPostAsync` yönteminde *Pages/Movies/Edit.cshtml.cs* dosya. Aşağıdaki vurgulanmış kodu değişiklikleri gösterir:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

Önceki kod, yalnızca ilk eşzamanlı istemci film siler ve ikinci eşzamanlı istemci film değişiklikler yazılarını eşzamanlılık algılar.

Test etmek için `catch` engelle:

* Bir kesme noktası ayarlayın `catch (DbUpdateConcurrencyException)`
* Bir filmi düzenleyin.
* Başka bir tarayıcı penceresinde seçin **silmek** bağlamak için aynı film ve film silin.
* Önceki tarayıcı penceresinde film değişiklikler gönderin.

Üretim kodu genellikle eşzamanlılık çakışması iki algılamaz veya daha fazla istemciler eşzamanlı olarak güncelleştirilen bir kaydı. Bkz: [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) daha fazla bilgi için.

### <a name="posting-and-binding-review"></a>Gönderme ve bağlama gözden geçirin

İncelemek *Pages/Movies/Edit.cshtml.cs* dosyası: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

Ne zaman bir HTTP GET isteği yapıldığında filmler/Düzenle sayfasına (örneğin, `http://localhost:5000/Movies/Edit/2`):

* `OnGetAsync` Yöntemi veritabanından film getirir ve döndürür `Page` yöntemi. 
* `Page` Yöntemi işler *Pages/Movies/Edit.cshtml* Razor sayfası. *Pages/Movies/Edit.cshtml* dosyasını içeren model yönergesi (`@model RazorPagesMovie.Pages.Movies.EditModel`), hangi kullanılabilir hale getirir film modeli sayfasında.
* Düzenleme formu film değerlerle görüntülenir.

Ne zaman filmler/düzenleme sayfasını nakledilir:

* Form değerleri sayfasında bağlı olan `Movie` özelliği. `[BindProperty]` Özniteliği etkinleştirir [bağlama Model](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Model durumuna bir hata varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), formu yeniden gönderilen değerlerle nakledilir.
* Model hatalar varsa, film kaydedilir.

Dizin oluşturma ve silme Razor sayfalarının HTTP GET yöntemlere benzer bir desen izleyin. HTTP POST `OnPostAsync` Razor Sayfa Oluştur yönteminde benzer bir desen izler `OnPostAsync` Razor sayfasını Düzenle yöntemi.

Arama sonraki öğreticide eklenir.