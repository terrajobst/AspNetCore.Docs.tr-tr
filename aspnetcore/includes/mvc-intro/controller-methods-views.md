
Şu konulara değineceğiz [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide. [Görüntülemek](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği ne bir alanın adını (Bu durumda "ReleaseDate" yerine "yayın tarihi") için görüntülenecek belirtir. [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) öznitelik alanında depolanan saat bilgisi görüntülenmiyor şekilde (tarih), veri türünü belirtir.

Gözat `Movies` denetleyicisi ve fare işaretçisini tutun bir **Düzenle** hedef URL görmek için bağlantı.

![Tarayıcı penceresini düzenleme bağlantısını ve bir bağlantı üzerinden fareyle URL'sini http://localhost:1234/Movies/Edit/5 gösterilir](../../tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

**Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar çekirdek MVC yer işareti etiketi yardımcı tarafından üretilen *Views/Movies/Index.cshtml* dosya.

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Yukarıdaki kod `AnchorTagHelper` dinamik olarak HTML oluşturan `href` denetleyici eylem yöntemi ve rota kimliğinden öznitelik değeri. Kullandığınız **kaynağı görüntüle** sık kullanılan tarayıcı ya da kullanım oluşturulan biçimlendirme incelemek için geliştirici araçları. Oluşturulan HTML bir bölümü aşağıda verilmiştir:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

Biçim için geri çağırma [yönlendirme](xref:mvc/controllers/routing) kümesinde *haline* dosyası:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core çevirir `http://localhost:1234/Movies/Edit/4` bir istek içine `Edit` eylem yöntemi `Movies` parametresiyle denetleyicisi `Id` 4. (Denetleyici olarak da bilinen eylem yöntemleri yöntemleridir.)

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ASP.NET Core en popüler yeni özellikler vardır. Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.

Açık `Movies` denetleyicisi ve iki inceleyin `Edit` eylem yöntemleri. Aşağıdaki kodda gösterildiği `HTTP GET Edit` film getirir ve tarafından oluşturulan düzenleme formu doldurur yöntemi *Edit.cshtml* Razor dosya.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Aşağıdaki kodda gösterildiği `HTTP POST Edit` gönderilen film değerlerini işler yöntemi:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

`[Bind]` Karşı koruma yollarından özniteliğidir [aşırı gönderim](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost). Özellikler yalnızca içermelidir `[Bind]` değiştirmek istediğiniz özniteliği. Bkz: [atlayarak nakil denetleyicinizi korumak](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application) daha fazla bilgi için. [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) atlayarak önlemek için alternatif bir yaklaşım sağlayın.

İkinci fark `Edit` tarafından eylem yöntemi öncesinde `[HttpPost]` özniteliği.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

`HttpPost` Özniteliği belirtir bu `Edit` yöntemi çağrılacak *yalnızca* için `POST` istekleri. Geçerli olabilir `[HttpGet]` ilk öznitelik Düzenle yöntemi, ancak gerekli değildir çünkü `[HttpGet]` varsayılandır.

`ValidateAntiForgeryToken` Özniteliği için kullanılan [istek sahteciliğini önleme](xref:security/anti-request-forgery) ve düzenleme görünümü dosyasında oluşturulan sahteciliğe karşı koruma belirteci ile eşleştirilmiş (*Views/Movies/Edit.cshtml*). Düzenleme görünümü dosya sahteciliğe karşı koruma belirteci oluşturur [Form etiketi yardımcı](xref:mvc/views/working-with-forms).

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Form etiketi yardımcı](xref:mvc/views/working-with-forms) eşleşmelidir gizli bir sahteciliğe karşı koruma belirteci oluşturur `[ValidateAntiForgeryToken]` sahteciliğe karşı koruma belirteci oluşturulan `Edit` filmler denetleyicisinin yöntemi. Daha fazla bilgi için bkz: [karşı istek Sahteciliğine](xref:security/anti-request-forgery).

`HttpGet Edit` Yöntemi alır film `ID` parametresi arar Entity Framework kullanarak filmi `SingleOrDefaultAsync` yöntemi ve seçili film düzenleme görünümü döndürür. Bir filmi bulunamazsa `NotFound` (HTTP 404) döndürülür.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Yapı iskelesi sistem düzenleme görünümü oluşturduğunuzda, incelenmesi `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` ve `<input>` sınıfın her bir özellik için öğeleri. Aşağıdaki örnek, Visual Studio yapı iskelesi sistem tarafından oluşturulan düzenleme görünümü gösterir:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

Şablonu görüntüleme nasıl sahip fark bir `@model MvcMovie.Models.Movie` deyimini dosyanın üst. `@model MvcMovie.Models.Movie` Görünüm model türünde olmasını şablonu görüntüleme için beklediğini belirtir `Movie`.

İskele kurulmuş kod, HTML biçimlendirmesi kolaylaştırmak için birkaç etiketi yardımcı yöntemler kullanır. [Etiket etiket Yardımcısı](xref:mvc/views/working-with-forms) ("Title", "ReleaseDate", "Tarz" veya "Price") alanın adını görüntüler. [Giriş etiketi yardımcı](xref:mvc/views/working-with-forms) bir HTML işleyen `<input>` öğesi. [Doğrulama etiket Yardımcısı](xref:mvc/views/working-with-forms) bu özellik ile ilişkili herhangi bir doğrulama iletisi görüntüler.

Uygulamayı çalıştırın ve gidin `/Movies` URL. Tıklatın bir **Düzenle** bağlantı. Tarayıcıda, sayfa için kaynağı görüntüleyin. İçin oluşturulan HTML `<form>` öğesi aşağıda gösterilmektedir.

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` Öğeleri olan bir `HTML <form>` öğesi, `action` özniteliği postalamak için ayarlanmış `/Movies/Edit/id` URL. Form verileri sunucuya nakledilir zaman `Save` düğmesine tıklandığında. Son satırı kapatmadan önce `</form>` öğesini gösterir gizli [XSRF](xref:security/anti-request-forgery) tarafından oluşturulan belirteç [Form etiketi yardımcı](xref:mvc/views/working-with-forms).

## <a name="processing-the-post-request"></a>POST isteği işleme

Aşağıdaki liste gösterildiği `[HttpPost]` sürümü `Edit` eylem yöntemi.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

`[ValidateAntiForgeryToken]` Özniteliği gizli doğrular [XSRF](xref:security/anti-request-forgery) sahteciliğe karşı koruma belirteci üreteci tarafından oluşturulan belirteç [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms)

[Model bağlama](xref:mvc/models/model-binding) sistem gönderilen form değerleri alır ve oluşturur bir `Movie` olarak geçirilen nesne `movie` parametresi. `ModelState.IsValid` Yöntemi doğrular biçiminde gönderilen veriler (düzenleme veya güncelleştirme) değiştirmek için kullanılabilir bir `Movie` nesnesi. Veriler geçerliyse kaydedilir. Güncelleştirilmiş (düzenlenen) film verileri çağırarak veritabanına kaydedilir `SaveChangesAsync` veritabanı bağlamının yöntemi. Veriler kaydedildikten sonra kodu kullanıcı için yönlendiren `Index` eylem yöntemi `MoviesController` yaptığınız değişiklikler dahil film koleksiyon görüntüler sınıfı.

Form sunucuya gönderilen önce istemci tarafı doğrulama alanlar tüm doğrulama kurallarını denetler. Herhangi bir doğrulama hatası varsa, bir hata iletisi görüntülenir ve form gönderilen değil. JavaScript devre dışıysa, istemci tarafı doğrulama olmaz ancak sunucu geçerli olmayan gönderilen değerler algılar ve form değerleri hata iletileri ile görünürler. Daha sonra öğreticide inceleyeceğiz [Model doğrulama](xref:mvc/models/validation) daha ayrıntılı. [Doğrulama etiket Yardımcısı](xref:mvc/views/working-with-forms) içinde *Views/Movies/Edit.cshtml* şablonu uygun hata iletilerini görüntüleme mvc'deki görünümü.

![Görünümü Düzenle: ABC hatalı bir fiyat değer için bir özel durum durumları fiyat alan bir sayı olmalıdır. Hatalı bir yayın tarihi değer için bir özel durum xyz durumlardan Lütfen geçerli bir tarih girin.](../../tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Tüm `HttpGet` benzer bir desen film denetleyicisi yöntemleri uygulayın. Film nesnesini alın (veya durumunda nesnelerin listesini `Index`) ve görünümüne nesne (modeli) geçirin. `Create` Yöntemi geçirir boş film nesneye `Create` görünümü. Bu nedenle oluşturmak, düzenlemek, silmek veya aksi halde verileri değiştirme tüm yöntemleri yapmak `[HttpPost]` yönteminin. Verileri değiştirme bir `HTTP GET` bir güvenlik riski bir yöntemdir. Verileri değiştirme bir `HTTP GET` yöntemi de ihlal HTTP en iyi yöntemler ve mimari [REST](http://rest.elkstein.org/) desen, GET istekleri uygulamanızın durumunu değiştirmemeniz belirtir. Diğer bir deyişle, bir GET işlemi gerçekleştirilirken hiçbir yan etkisi olan ve kalıcı verilerinizi değiştirmeyen güvenli bir işlem olmalıdır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket Yardımcıları giriş](xref:mvc/views/tag-helpers/intro)
* [Yazar etiket Yardımcıları](xref:mvc/views/tag-helpers/authoring)
* [İstek Sahteciliğinden Koruma](xref:security/anti-request-forgery)
* Denetleyicisinden korumak [aşırı gönderim](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Etiket etiket Yardımcısı](xref:mvc/views/working-with-forms)
* [Etiket Yardımcısı seçin](xref:mvc/views/working-with-forms)
* [Doğrulama etiket Yardımcısı](xref:mvc/views/working-with-forms)
