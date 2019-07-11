
Biz karşılarız [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide. [Görüntüleme](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) öznitelik adı (Bu durumda "ReleaseDate" yerine "yayın tarihi") bir alan için görüntülenecek öğeleri belirtir. [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) öznitelik alanında depolanan saat bilgilerini görüntülenmediğini şekilde (tarih), veri türünü belirtir.

`[Column(TypeName = "decimal(18, 2)")]` Veri ek açıklama, Entity Framework Core doğru şekilde eşleyebilirsiniz biçimde gereklidir `Price` veritabanında para birimi. Daha fazla bilgi için [veri türleri](/ef/core/modeling/relational/data-types).

Gözat `Movies` denetleyicisi ve fare işaretçisini tutun bir **Düzenle** hedef URL'ye görmek için bağlantıyı.

![Düzenleme bağlantısını ve bir bağlantı üzerinde fare ile tarayıcı penceresinde URL'sini http://localhost:1234/Movies/Edit/5 gösterilir](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

**Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları Core MVC yer işareti etiketi Yardımcısı tarafından üretilen *Views/Movies/Index.cshtml* dosya.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Yukarıdaki kodda `AnchorTagHelper` dinamik olarak HTML oluşturan `href` denetleyici eylem yöntemi ve rota kimliğinden öznitelik değeri. Kullandığınız **kaynağı görüntüle** sık kullandığınız tarayıcıyı ya da kullanım oluşturulan biçimlendirme incelemek için geliştirici araçları. Oluşturulan HTML değerinin bir bölümü aşağıda gösterilmiştir:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

Biçim için geri çağırma [yönlendirme](xref:mvc/controllers/routing) kümesinde *Startup.cs* dosyası:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core çevirir `http://localhost:1234/Movies/Edit/4` bir istek halinde `Edit` eylem yöntemi `Movies` denetleyicisi parametresiyle `Id` 4. (Denetleyici olarak da bilinen eylem yöntemleri yöntemlerdir.)

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ASP.NET Core en popüler yeni özellikler biridir. Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.

Açık `Movies` denetleyicisi ve iki inceleyin `Edit` eylem yöntemleri. Aşağıdaki kodda gösterildiği `HTTP GET Edit` film getirir ve tarafından oluşturulan düzenleme formu dolduran yöntemi *Edit.cshtml* Razor dosya.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Aşağıdaki kodda gösterildiği `HTTP POST Edit` gönderilen film değerleri işleyen yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Aşağıdaki kodda gösterildiği `HTTP POST Edit` gönderilen film değerleri işleyen yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[Bind]` Özniteliktir karşı korumak için bir yol [aşırı yayınlayarak](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost). Özellikler yalnızca içermelidir `[Bind]` değiştirmek istediğiniz özniteliği. Bkz: [denetleyicinizin atlayarak nakil korumak](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application) daha fazla bilgi için. [Viewmodel'lar](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) atlayarak önlemek için alternatif bir yaklaşım sağlar.

İkinci fark `Edit` eylem yöntemine öncesinde `[HttpPost]` özniteliği.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

`HttpPost` Özniteliği belirtir bu `Edit` yöntemi çağrılacak *yalnızca* için `POST` istekleri. Geçerli olabilir `[HttpGet]` ilk özniteliği Düzenle yöntemi, ancak gerekli değildir çünkü `[HttpGet]` varsayılandır.

`ValidateAntiForgeryToken` Özniteliktir için kullanılan [istek sahteciliğini önleme](xref:security/anti-request-forgery) ve düzenleme görünümü dosyasında oluşturulan bir sahteciliğe karşı koruma belirteci ile eşleştirilmiş (*Views/Movies/Edit.cshtml*). Sahteciliğe karşı koruma belirteci ile düzenleme görünüm dosyası oluşturur [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms).

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Form etiketi Yardımcısı](xref:mvc/views/working-with-forms) eşleşmelidir gizli bir sahteciliğe karşı koruma belirteci oluşturan `[ValidateAntiForgeryToken]` oluşturulan sahteciliğe karşı koruma belirtecine `Edit` denetleyici filmler yöntemi. Daha fazla bilgi için [istek sahteciliğinden koruma](xref:security/anti-request-forgery).

`HttpGet Edit` Yöntemi alır film `ID` parametresini arar Entity Framework kullanarak filmi `SingleOrDefaultAsync` yöntemi ve düzenleme görünümü seçili film döndürür. Bir filmi bulunamazsa `NotFound` (HTTP 404) döndürülür.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

::: moniker-end

Yapı iskelesi sistem düzenleme görünümü oluşturduğunuzda, onu incelenirken `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` ve `<input>` sınıfın her bir özellik için öğeleri. Aşağıdaki örnek, Visual Studio yapı iskelesi sistem tarafından oluşturulan düzenleme görünümünü gösterir:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

Şablonu Görüntüle nasıl olduğunu fark bir `@model MvcMovie.Models.Movie` deyimini dosyanın üst. `@model MvcMovie.Models.Movie` Görünüm model görünüm şablonu türünde olmasını bekliyor belirtir `Movie`.

İskele kurulan kodu birkaç etiketi Yardımcısı yöntemleri HTML biçimlendirmeyi kolaylaştırmak için kullanır. [Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms) ("Title", "ReleaseDate", "Tarzı" veya "Price") alanın adını görüntüler. [Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) bir HTML işleyen `<input>` öğesi. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms) bu özellikle ilişkili herhangi bir doğrulama iletisi görüntüler.

Uygulamayı çalıştırmak ve gidin `/Movies` URL'si. ' A tıklayın bir **Düzenle** bağlantı. Tarayıcıda, sayfa için kaynağı görüntüleyin. İçin oluşturulan HTML `<form>` öğesi aşağıda gösterilmektedir.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` Öğeler içinde bir `HTML <form>` öğesi olan `action` özniteliğinin ayarlanmış gönderinin yayımlanacağı `/Movies/Edit/id` URL'si. Form verileri sunucuya yayımlanacak olduğunda `Save` düğmesine tıklandığında. Son satırı kapatmadan önce `</form>` öğenin gizli gösterir [XSRF](xref:security/anti-request-forgery) tarafından oluşturulan belirteç [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms).

## <a name="processing-the-post-request"></a>POST isteğini işleme

Aşağıdaki liste gösterildiği `[HttpPost]` sürümünü `Edit` eylem yöntemi.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[ValidateAntiForgeryToken]` Özniteliği gizli doğrular [XSRF](xref:security/anti-request-forgery) içinde sahteciliğe karşı koruma belirteci Oluşturucu tarafından oluşturulan belirteç [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms)

[Model bağlama](xref:mvc/models/model-binding) sistem gönderilen form değerlerini alır ve oluşturan bir `Movie` olarak geçirilen nesne `movie` parametresi. `ModelState.IsValid` Yöntemi doğrular (düzenleme veya güncelleştirme) değiştirileceğini biçiminde gönderilen veriler kullanılabilir bir `Movie` nesne. Veriler geçerliyse kaydedilir. Güncelleştirilmiş (düzenlenen) film verileri çağırarak veritabanına kaydedilir `SaveChangesAsync` veritabanı bağlamının yöntemi. Verileri kaydettikten sonra kodu kullanıcı için yönlendiren `Index` eylem yöntemi `MoviesController` yaptığınız değişiklikleri içeren film koleksiyonu görüntüleyen sınıfı.

Form, sunucuya gönderilen önce istemci tarafı doğrulama alanlarda tüm doğrulama kurallarını denetler. Herhangi bir doğrulama hatası varsa, bir hata iletisi görüntülenir ve form gönderilen değil. JavaScript devre dışı bırakılırsa, istemci tarafı doğrulama olmaz ancak sunucu, geçerli olmayan gönderilen değerlerden algılar ve form değerleri, hata iletileri ile yeniden. Öğreticinin ilerleyen bölümlerinde inceleyeceğiz [Model doğrulama](xref:mvc/models/validation) daha ayrıntılı bir şekilde. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms) içinde *Views/Movies/Edit.cshtml* görünüm şablonu uygun hata iletilerini görüntüleme üstlenir.

![Görünümü düzenleyin: Yanlış bir fiyat değer için bir özel durum ABC Fiyat alanı bir sayı olmalıdır belirtir. Yayın tarihi yanlış bir değer için bir özel durum xyz durumlarının Lütfen geçerli bir tarih girin.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Tüm `HttpGet` film denetleyici yöntemleri benzer bir desen uygulayın. Bir film nesnesi aldıkları (veya durumunda nesnelerin listesini `Index`) ve ' % s'nesne (modeli) görünümüne geçirin. `Create` Boş film nesneye yöntemi geçirir `Create` görünümü. Bu nedenle, oluşturmak, düzenlemek, silmek veya aksi halde verileri değiştiren tüm yöntemler yapmak `[HttpPost]` yöntemi aşırı yüklemesi. Verileri değiştirme bir `HTTP GET` bir güvenlik riski yöntemidir. Verileri değiştirme bir `HTTP GET` yöntemi de ihlal HTTP en iyi yöntemler ve mimari [REST](http://rest.elkstein.org/) desen, GET istekleri, uygulamanızın durumunu değiştirmemeniz belirtir. Diğer bir deyişle, bir GET işlemi gerçekleştirilirken yan etkileri olan ve verilerinizi kalıcı değiştirmez güvenli bir işlem olmalıdır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket Yardımcıları giriş](xref:mvc/views/tag-helpers/intro)
* [Yazma etiketi Yardımcıları](xref:mvc/views/tag-helpers/authoring)
* [İstek Sahteciliğinden Koruma](xref:security/anti-request-forgery)
* Denetleyicinizden korumak [aşırı gönderme](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [Viewmodel'lar](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Form Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Giriş Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Etiket Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Seçim Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms)
