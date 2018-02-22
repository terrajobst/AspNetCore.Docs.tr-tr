Değiştir *Views/HelloWorld/Index.cshtml* aşağıdaki Razor görünüm dosyası:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Gidin `http://localhost:xxxx/HelloWorld`. `Index` Yönteminde `HelloWorldController` çok yapmak olmadı; deyim çalışan `return View();`, hangi belirtilen tarayıcı yanıta işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Görünüm şablonu dosyasının adı açıkça belirtmediğiniz çünkü MVC kullanarak varsayılan *Index.cshtml* görünüm dosyasında */görünümler/HelloWorld* klasör. Aşağıdaki görüntü dizesi "bizim Görünüm şablonundan! Hello ifadesini" gösterir sabit kodlanmış görünümünde.

![Bir tarayıcı penceresi](../../tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Tarayıcı pencerenizi (örneğin bir mobil cihazda) küçükse, geçiş (tap) gerekebilecek [önyükleme gezinti düğmesi](http://getbootstrap.com/components/#navbar) görmek için sağ üst **giriş**, **hakkında**, ve **kişi** bağlantılar.

![Tarayıcı penceresini önyükleme gezinti düğmesi vurgulama](../../tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Görünümleri ve Düzen sayfaları değiştirme

Menü Bağlantılar'a dokunun (**MvcMovie**, **giriş**, **hakkında**). Her bir sayfa aynı menü düzeni gösterilir. Menü düzeni uygulanan *Views/Shared/_Layout.cshtml* dosya. Açık *Views/Shared/_Layout.cshtml* dosya.

[Düzen](xref:mvc/views/layout) şablonları tek bir yerde, sitenizin HTML kapsayıcı düzeni belirtin ve sitenizin birden çok sayfada üzerinden uygulanan olanak sağlar. Bul `@RenderBody()` satır. `RenderBody` olan burada tüm görünüm özgü sayfaları, bir yer tutucu oluşturmak Göster, *Sarmalanan* düzeni sayfasında. Örneğin, **hakkında** bağlantı **Views/Home/About.cshtml** görünümü içinde işlenir `RenderBody` yöntemi.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Düzen dosyasını başlığı ve menü bağlantıyı değiştirme

Başlık öğesinde `MvcMovie` için `Movie App`. Düzen şablonu bağlantı metinde değiştirmek `MvcMovie` için `Movie App` ve denetleyicisinden `Home` için `Movies` aşağıda vurgulanan:

Not: ASP.NET Core 2.0 sürümü biraz farklıdır. Bunu içermiyor `@inject ApplicationInsights` ve `@Html.Raw(JavaScriptSnippet.FullScript)`.

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

>[!WARNING]
> Uygulanan henüz `Movies` , henüz denetleyicisi, dolayısıyla bu bağlantıyı tıklatın, 404 (bulunamadı) hata iletisi alırsınız.

Yaptığınız değişiklikleri kaydedin ve dokunun **hakkında** bağlantı. Nasıl başlık tarayıcı sekmesinde şimdi görüntülendiğine dikkat edin **- film uygulaması hakkında** yerine **hakkında - Mvc film**: 

![Hakkında sekmesi](../../tutorials/first-mvc-app/adding-view/_static/about2.png)

Dokunun **kişi** başlık ve bağlantı metni ayrıca görüntüle dikkat edin ve bağlama **film uygulaması**. Düzen şablonda değişiklik kez bulduk ve sahip sitesindeki tüm sayfalara yansıtmak yeni bağlantı metnini ve yeni başlığı.

İncelemek *Views/_ViewStart.cshtml* dosyası:


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* dosya getirir *Views/Shared/_Layout.cshtml* her görünüm dosyasına. Kullanabileceğiniz `Layout` farklı bir görünümü ayarlayın veya ayarlamak özellik `null` herhangi bir düzen dosyası kullanılacak şekilde.

Başlığını değiştirmek `Index` görünümü.

Açık *Views/HelloWorld/Index.cshtml*. Bir değişiklik yapmak için iki yerde vardır:

   * Tarayıcının başlık görüntülenen metin.
   * İkincil üstbilgi (`<h2>` öğesi).

Hangi bölümünün uygulamanın hangi bit kod değişiklikleri görebilmeleri biraz farklı yapmanız.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` ayarlar yukarıdaki kodda `Title` özelliği `ViewData` "Film listesine" sözlük. `Title` Özelliği kullanılıyor `<title>` HTML öğesi düzeni sayfasında:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Değişikliklerinizi kaydetmek ve gidin `http://localhost:xxxx/HelloWorld`. Tarayıcı başlığı, birincil başlık ve ikincil başlıklar değişmiş dikkat edin. (Değişiklikleri tarayıcıda görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz. Yüklenecek sunucudan yanıt zorlamak için tarayıcınızda CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewData["Title"]` biz kümesinde *Index.cshtml* şablonu ve ek görüntüle "-film uygulaması" düzeni dosyasına eklendi.

Ayrıca dikkat edin nasıl içerik *Index.cshtml* görünüm şablonu birleştirilmiş ile *Views/Shared/_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderildi. Düzen şablonları, uygulamanızdaki sayfaların tümünü uygulamak değişiklik gerçekten kolay hale getirir. Daha fazla bilgi edinmek için [düzeni](../../mvc/views/layout.md).

![Film liste görünümü](../../tutorials/first-mvc-app/adding-view/_static/hell3.png)

Bizim az bitlik (Bu durumda "Hello bizim görünüm şablondan!" "veri" ileti), sabit kodlanmış, ancak. MVC uygulama bir "V" (Görünüm) vardır ve bir "C" (denetleyicisi), ancak hiçbir "M" (modeli) henüz var.

## <a name="passing-data-from-the-controller-to-the-view"></a>Denetleyici geçirme verileri görüntülemek için

Denetleyici eylemleri, gelen bir URL isteğine yanıt olarak çağrılır. Denetleyici gelen tarayıcı istekleri işleyen kodu yazma burada sınıfıdır. Denetleyici bir veri kaynağından veri alır ve ne tür bir tarayıcıya göndermek için yanıt verir. Görünüm şablonları oluşturmak ve bir HTML yanıtını tarayıcıya biçimlendirmek için bir denetleyicisinden kullanılabilir.

Denetleyicileri sırada bir yanıtı işlemek bir şablonu görüntüleme için gerekli verileri sunmak için sorumludur. En iyi yöntem: görünüm şablonları gereken **değil** iş mantığı gerçekleştirmek ya da bir veritabanı ile doğrudan etkileşim. Bunun yerine, şablonu görüntüleme için denetleyici tarafından sağlanan verileri ile çalışması gerekir. "Sorunları ayrımı" bakımı, temiz, sınanabilir ve sürdürülebilir kodunuzu tutmaya yardımcı olur.

Şu anda `Welcome` yönteminde `HelloWorldController` sınıfını alır bir `name` ve `ID` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine bir görünüm şablonu kullanmayı denetleyicisini değiştirin. Şablonu görüntüleme uygun bit veri denetleyicisinden görünümüne yanıtı oluşturmak için geçirilmelidir, yani dinamik bir yanıt oluşturur. Şablonu görüntüleme gereksinimlerinize dinamik veri (parametre) put denetleyicisi sağlayarak bunu bir `ViewData` şablonu görüntüle daha sonra erişebilirsiniz sözlük.

Geri dönüp *HelloWorldController.cs* dosya ve değişiklik `Welcome` ekleme yöntemi bir `Message` ve `NumTimes` değeri `ViewData` sözlük. `ViewData` Dictionary'si, koyabilir istediğiniz ona; yani dinamik bir nesne `ViewData` nesnesi içindeki put kadar tanımlı hiçbir özellik sahiptir. [MVC model bağlama sistem](xref:mvc/models/model-binding) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Sözlük nesnesi görünüme iletilen verileri içerir. 

Adlı bir Hoş Geldiniz görünüm şablonu oluşturma *Views/HelloWorld/Welcome.cshtml*.

Bir döngüde oluşturacaksınız *Welcome.cshtml* "Hello" görüntüler şablonu görüntüleme `NumTimes`. Değiştir *Views/HelloWorld/Welcome.cshtml* aşağıdaki:

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Yaptığınız değişiklikleri kaydedin ve aşağıdaki URL'ye gidin:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Veri URL'den alınır ve denetleyici kullanmaya geçirilen [MVC model bağlayıcı](xref:mvc/models/model-binding) . Denetleyici verileri paketleri bir `ViewData` sözlük ve nesne görünümüne geçirir. Görünümü sonra verileri tarayıcıya HTML olarak işler.

![Hoş Geldiniz etiket ve Hello dört kez gösterilen Rick deyimi gösteren görünüm hakkında](../../tutorials/first-mvc-app/adding-view/_static/rick2.png)

Yukarıdaki örnekte, kullandık `ViewData` denetleyicisinden bir görünüme veri iletmek için Sözlük. Öğreticide daha sonra bir denetleyicisinden bir görünüme veri iletmek için bir görünüm modeli kullanacağız. Veri geçirme görünüm modeli genellikle çok tercih edilen üzerinden yaklaşımdır `ViewData` sözlük yaklaşım. Bkz: [ViewModel vs ViewData vs ViewBag vs TempData vs MVC oturumunda](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) daha fazla bilgi için.

De, "M" türde bir model, ancak veritabanı türü için oluştu. Ne biz öğrendiğinize ve film bir veritabanı oluşturmak atalım.
