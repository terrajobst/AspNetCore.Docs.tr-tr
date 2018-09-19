Öğesinin içeriğini değiştirin *Views/HelloWorld/Index.cshtml* aşağıdaki Razor görünüm dosyası:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Gidin `http://localhost:xxxx/HelloWorld`. `Index` Yönteminde `HelloWorldController` çok başarmadık; çalıştığını deyim `return View();`, hangi belirtilen tarayıcı yanıt işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Görünüm şablon dosyası adı açıkça belirtmediğiniz için MVC için kullanılacak varsayılan *Index.cshtml* görünümü dosyasında */Views/HelloWorld* klasör. Aşağıdaki görüntüde "Bizim görünümü şablondan Merhaba!" dizesini gösterir. sabit kodlanmış görüntüleme.

![Tarayıcı penceresi](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Tarayıcı pencerenizin (örneğin bir mobil cihazda) küçük ise, iki durumlu düğmeyi (tap) gerekebilir [önyükleme gezinti düğmesi](http://getbootstrap.com/components/#navbar) görmek için sağ üst köşedeki **giriş**, **hakkında**, ve **kişi** bağlantıları.

![Tarayıcı penceresi önyükleme gezinti düğmesi vurgulama](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Görünümlere ve sayfalara düzenini değiştirme

Menü bağlantıları dokunun (**MvcMovie**, **giriş**, **hakkında**). Her sayfada aynı menüsü düzeni gösterilir. Menüsü düzeni uygulanan *Views/Shared/_Layout.cshtml* dosya. Açık *Views/Shared/_Layout.cshtml* dosya.

[Düzen](xref:mvc/views/layout) , tek bir yerde sitenizin HTML kapsayıcı düzenini belirtin ve ardından sitenizdeki birden çok sayfada uygulamak şablonlar sağlar. Bulma `@RenderBody()` satır. `RenderBody` olduğundan burada tüm görünüm özel sayfalar, yer tutucu oluşturma Göster, *sarmalanmış* Düzen sayfasında. Örneğin, **hakkında** bağlantı **Views/Home/About.cshtml** görünümü içinde işlenir `RenderBody` yöntemi.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Düzen dosyası başlığı ve menü Bağlantıyı Değiştir

Başlık öğesinde `MvcMovie` için `Movie App`. Düzen şablonu bağlantı metni değiştirme `MvcMovie` için `Movie App` ve denetleyicisinden `Home` için `Movies` aşağıda vurgulanan:

::: moniker range="<= aspnetcore-2.0"

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=7,31)]

::: moniker-end

>[!WARNING]
> Biz uygulamadığınız `Movies` , henüz denetleyici, dolayısıyla bu bağlantıyı tıklatın, 404 (bulunamadı) hatası alırsınız.

Değişikliklerinizi kaydedip dokunun **hakkında** bağlantı. Tarayıcı sekmesinde başlığı şimdi nasıl görüntülendiğine dikkat edin **film uygulaması hakkında -** yerine **hakkında - Mvc film**: 

![Sekme hakkında](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Dokunun **kişi** bağlamak ve başlık ve bağlantı metni de görüntülemek fark **film uygulaması**. Biz Düzen şablonda değişiklik kez sunmayı başarabilseydiniz nasıl olurdu ve sahip sitesindeki tüm sayfalara yansıtan yeni bir başlık ve yeni bağlantı metni.

İnceleme *Views/_ViewStart.cshtml* dosyası:


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* dosya getirdiği *Views/Shared/_Layout.cshtml* dosyaya her görünüm. Kullanabileceğiniz `Layout` farklı düzeni görünümünde veya ayarlamak, özellik `null` hiçbir Düzen dosyası yeniden kullanılacak şekilde.

Başlığını değiştirme `Index` görünümü.

Açık *Views/HelloWorld/Index.cshtml*. Değişiklik yapmak için iki yerde vardır:

   * Tarayıcının başlık görüntülenen metin.
   * İkincil üstbilgi (`<h2>` öğesi).

Uygulamanın hangi parçası hangi bit kod değişiklikleri görebilmeniz için biraz daha farklı yapmanız.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` koddaki kümeleri üstündeki `Title` özelliği `ViewData` sözlüğe "Film listesi". `Title` Özelliği kullanılan `<title>` HTML öğesini düzen sayfası:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Değişikliği kaydetmek ve gidin `http://localhost:xxxx/HelloWorld`. Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin. (Tarayıcı değişiklikleri görmüyorsanız, önbelleğe alınmış içerikleri görüntülüyor olabilirsiniz. Tarayıcınızda yüklenmesine sunucudan yanıt zorlamak için CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewData["Title"]` biz kümesinde *Index.cshtml* şablonu ve ek görüntüleme "-film uygulaması" Düzen dosyasında eklendi.

Ayrıca dikkat edin nasıl içeriği *Index.cshtml* görünüm şablonu ile birleştirilmiş *Views/Shared/_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderilen. Düzen şablonları gerçekten tüm uygulamanızdaki sayfaların uygulanacak değişiklikler yapmak kolaylaştırır. Daha fazla bilgi edinmek için [Düzen](xref:mvc/views/layout).

![Film liste görünümü](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

(Bu durumda "Hello bizim görünümü şablondan!", "veri" az bizim bit ileti) sabit kodlanmıştır, ancak. MVC uygulaması bir "V" (view) ve "C" (denetleyicisi), ancak hiçbir "M" (modeli) henüz kendinizi.

## <a name="passing-data-from-the-controller-to-the-view"></a>Görünüm denetleyicisinden veri geçirme

Denetleyici eylemleri, bir gelen URL isteğine yanıt olarak çağrılır. Burada, tarayıcı gelen istekleri işleyen kodu yazdığınız bir denetleyici sınıftır. Denetleyici, verileri bir veri kaynağından alır ve ne tür bir tarayıcıya gönderilecek yanıt karar verir. Bir denetleyiciden görünüm şablonları oluşturmak ve bir HTML yanıtını tarayıcıya biçimlendirmek için kullanılabilir.

Denetleyicileri sırada bir yanıt işlemek bir görünüm şablon için gerekli olan veriler sağlamaktan sorumludur. En iyi yöntem: görünüm şablonları gereken **değil** iş mantığını gerçekleştirebilir veya bir veritabanı ile doğrudan etkileşim. Bunun yerine, bir şablonu görüntüleme için denetleyici tarafından sağlanan veri ile çalışması gerekir. Bu "ayrımı" bakımı, kodunuzu temiz, sınanabilir ve sürdürülebilir tutmaya yardımcı olur.

Şu anda `Welcome` yönteminde `HelloWorldController` sınıfı alır bir `name` ve `ID` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine bir şablonu görüntüleme kullanmayı denetleyici değiştirin. Şablonu Görüntüle uygun veri bitleri denetleyiciden görünüm yanıtı oluşturmak için geçirilmesi gerektiğini yani dinamik bir yanıt oluşturur. Şablonu görüntüleme, gereken dinamik (Parametreler) verilerinizden denetleyicisi sağlayarak bunu bir `ViewData` görünüm şablonu erişebiliyorsa sözlüğü.

Geri dönüp *HelloWorldController.cs* dosya ve değiştirme `Welcome` yöntemi eklemek için bir `Message` ve `NumTimes` değerini `ViewData` sözlüğü. `ViewData` Sözlüktür, koyabilir ne olursa olsun istediğiniz kendisine; yani dinamik bir nesne `ViewData` nesnesi içine yerleştirin kadar tanımlı hiçbir özellik sahiptir. [MVC model bağlama sistemi](xref:mvc/models/model-binding) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Sözlük nesnesi, görünüme iletilen verileri içerir. 

Adlı bir Hoş Geldiniz görünüm şablonu oluşturma *Views/HelloWorld/Welcome.cshtml*.

Bir döngüde oluşturacaksınız *Welcome.cshtml* "Hello" görüntüleyen bir görünüm şablonu `NumTimes`. Öğesinin içeriğini değiştirin *Views/HelloWorld/Welcome.cshtml* aşağıdaki:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Yaptığınız değişiklikleri kaydedin ve aşağıdaki URL'ye gidin:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Verileri URL'den gerçekleştirilen ve denetleyicisi kullanarak geçirilen [MVC model bağlayıcı](xref:mvc/models/model-binding) . Denetleyici verileri paketleri bir `ViewData` sözlük ve Görünüm nesnesi geçer. Görünümü ardından verilerin tarayıcıya HTML olarak işler.

![Hoş Geldiniz etiket ve Hello dört kez gösterilen Rick ifadesini gösteren görünümü hakkında](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

Yukarıdaki örnekte kullandık `ViewData` denetleyicisinden bir görünüme veri iletmek için Sözlük. Öğreticinin sonraki bölümlerinde bir denetleyiciden bir görünüme veri iletmek için bir görünüm modeli kullanacağız. Veri geçirme görünüm modeli yaklaşım genellikle çok tercih üzerinden `ViewData` sözlük yaklaşım. Bkz: [ViewModel vs ViewData vs ViewBag vs TempData vs MVC oturumunda](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) daha fazla bilgi için.

İyi modeli, ancak veritabanı türü için "M" bir tür olan. Ne biz öğrendiniz ve film veritabanı oluşturma ele alalım.
