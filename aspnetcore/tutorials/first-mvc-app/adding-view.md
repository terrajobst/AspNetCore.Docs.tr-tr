---
title: Bir ASP.NET Core MVC uygulaması için bir Görünüm Ekle
author: rick-anderson
description: Basit bir ASP.NET Core MVC uygulaması görünüm ekleme
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: f241a19c8821019f327fb160f01fe01eca53c5d0
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836915"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için bir Görünüm Ekle

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, değişiklik `HelloWorldController` kullanılacak sınıfı [Razor](xref:mvc/views/razor) görünüm dosyalarına indrebilirsiniz kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemi.

Razor kullanarak görünüm şablon dosyası oluşturun. Razor tabanlı bir görünüm şablonları bir *.cshtml* dosya uzantısı. HTML çıkışı ile oluşturmak için zarif bir şekilde sağladıkları C#.

Şu anda `Index` yöntemi controller sınıfında sabit kodlu olduğunu belirten bir ileti içeren bir dize döndürür. İçinde `HelloWorldController` sınıfı, yerine `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Önceki kod, denetleyicinin çağırır <xref:Microsoft.AspNetCore.Mvc.Controller.View*> yöntemi. HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Denetleyici yöntemlerinde (olarak da bilinen *eylem yöntemleri*), gibi `Index` yukarıdaki yöntemi genellikle iade bir <xref:Microsoft.AspNetCore.Mvc.IActionResult> (veya türetilmiş bir sınıf <xref:Microsoft.AspNetCore.Mvc.ActionResult>), bir tür değil istediğiniz `string`.

## <a name="add-a-view"></a>Görünüm ekleme

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Sağ tıklayın *görünümleri* klasörünü ve ardından **Ekle > Yeni klasör** ve klasörünü adlandırın *HelloWorld*.

* Sağ tıklayın *görünümler/HelloWorld* klasörünü ve ardından **Ekle > Yeni öğe**.

* İçinde **Yeni Öğe Ekle - MvcMovie** iletişim

  * Sağ üst arama kutusuna girin *görüntüle*

  * Seçin **Razor Görünüm**

  * Tutun **adı** değer kutusuna *Index.cshtml*.

  * Seçin **Ekle**

![Yeni öğe iletişim kutusu Ekle](adding-view/_static/add_view.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ekleme bir `Index` görüntüleme `HelloWorldController`.

* Adlı yeni bir klasör eklemek *görünümler/HelloWorld*.
* Yeni bir dosya ekleyin *görünümler/HelloWorld* klasör adı *Index.cshtml*.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ tıklayın *görünümleri* klasörünü ve ardından **Ekle > Yeni klasör** ve klasörünü adlandırın *HelloWorld*.
* Sağ tıklayın *görünümler/HelloWorld* klasörünü ve ardından **Ekle > yeni dosya**.
* İçinde **yeni dosya** iletişim:

  * Seçin **Web** sol bölmesinde.
  * Seçin **boş HTML dosyası** Orta bölmedeki.
  * Tür *Index.cshtml* içinde **adı** kutusu.
  * Seçin **yeni**.

![Yeni öğe iletişim kutusu Ekle](adding-view/_static/add_view.png)

---  
<!-- End of VS tabs -->

Öğesinin içeriğini değiştirin *Views/HelloWorld/Index.cshtml* aşağıdaki Razor görünüm dosyası:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

`https://localhost:xxxx/HelloWorld` sayfasına gidin. `Index` Yönteminde `HelloWorldController` çok başarmadık; çalıştığını deyim `return View();`, hangi belirtilen tarayıcı yanıt işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Görünüm şablon dosyası adı açıkça belirtmediğiniz için MVC için kullanılacak varsayılan *Index.cshtml* görünümü dosyasında */Views/HelloWorld* klasör. Aşağıdaki görüntüde "Bizim görünümü şablondan Merhaba!" dizesini gösterir. sabit kodlanmış görüntüleme.

![Tarayıcı penceresi](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>Değişiklik görünümleri ve yerleşim sayfaları

Menü bağlantıyı seçin (**MvcMovie**, **giriş**, ve **gizlilik**). Her sayfada aynı menüsü düzeni gösterilir. Menüsü düzeni uygulanan *Views/Shared/_Layout.cshtml* dosya. Açık *Views/Shared/_Layout.cshtml* dosya.

[Düzen](xref:mvc/views/layout) , tek bir yerde sitenizin HTML kapsayıcı düzenini belirtin ve ardından sitenizdeki birden çok sayfada uygulamak şablonlar sağlar. Bulma `@RenderBody()` satır. `RenderBody` olduğundan burada tüm görünüm özel sayfalar, yer tutucu oluşturma Göster, *sarmalanmış* Düzen sayfasında. Örneğin, **gizlilik** bağlantı **Views/Home/Privacy.cshtml** görünümü içinde işlenir `RenderBody` yöntemi.

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>Düzen dosyası başlık, alt bilgi ve menü Bağlantıyı Değiştir

* Başlık ve alt öğelerini değiştirmeniz `MvcMovie` için `Movie App`.
* Bağlayıcı bir öğe değiştirme `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` için `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.

Aşağıdaki biçimlendirmede vurgulanan değişiklikleri gösterir:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24)]

Önceki biçimlendirmede `asp-area` [yer işareti etiketi Yardımcısı özniteliği](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) bu uygulamayı değil kullandığından atlandı [alanları](xref:mvc/controllers/areas).

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

**Not**: `Movies` Denetleyicisi uygulanmamış. Bu noktada, `Movie App` bağlantısı işlevsel değil.

Seçin ve değişiklikleri kaydetmek **gizlilik** bağlantı. Tarayıcı sekmesini başlığında nasıl görüntülendiğine dikkat edin **gizlilik ilkesi - film uygulaması** yerine **gizlilik ilkesi - Mvc film**:

![Gizlilik sekmesi](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Seçin **giriş** bağlamak ve başlık ve bağlantı metni de görüntülemek fark **film uygulaması**. Biz Düzen şablonda değişiklik kez sunmayı başarabilseydiniz nasıl olurdu ve sahip sitesindeki tüm sayfalara yansıtan yeni bir başlık ve yeni bağlantı metni.

İnceleme *Views/_ViewStart.cshtml* dosyası:

```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* dosya getirdiği *Views/Shared/_Layout.cshtml* dosyaya her görünüm. `Layout` Özelliği, farklı bir görünümü ayarlamak veya ayarlamanız için kullanılabilir `null` hiçbir Düzen dosyası yeniden kullanılacak şekilde.

Başlığı değiştirmek ve `<h2>` öğesinin *Views/HelloWorld/Index.cshtml* dosyasını görüntüle:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

Başlığı ve `<h2>` hangi bit kod değişiklikleri görüntüleme görebilmeniz için öğe biraz farklı.

`ViewData["Title"] = "Movie List";` koddaki kümeleri üstündeki `Title` özelliği `ViewData` sözlüğe "Film listesi". `Title` Özelliği kullanılan `<title>` HTML öğesini düzen sayfası:

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Değişikliği kaydetmek ve gidin `https://localhost:xxxx/HelloWorld`. Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin. (Tarayıcı değişiklikleri görmüyorsanız, önbelleğe alınmış içerikleri görüntülüyor olabilirsiniz. Tarayıcınızda yüklenmesine sunucudan yanıt zorlamak için CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewData["Title"]` biz kümesinde *Index.cshtml* şablonu ve ek görüntüleme "-film uygulaması" Düzen dosyasında eklendi.

Ayrıca dikkat edin nasıl içeriği *Index.cshtml* görünüm şablonu ile birleştirilmiş *Views/Shared/_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderilen. Düzen şablonları gerçekten tüm uygulamanızdaki sayfaların uygulanacak değişiklikler yapmak kolaylaştırır. Daha fazla bilgi edinmek için [Düzen](xref:mvc/views/layout).

![Film liste görünümü](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

(Bu durumda "Hello bizim görünümü şablondan!", "veri" az bizim bit ileti) sabit kodlanmıştır, ancak. MVC uygulaması bir "V" (view) ve "C" (denetleyicisi), ancak hiçbir "M" (modeli) henüz kendinizi.

## <a name="passing-data-from-the-controller-to-the-view"></a>Görünüm denetleyicisinden veri geçirme

Denetleyici eylemleri, bir gelen URL isteğine yanıt olarak çağrılır. Kod gelen tarayıcı istekleri işleyen yazıldığı bir denetleyici sınıftır. Denetleyici, verileri bir veri kaynağından alır ve ne tür bir tarayıcıya gönderilecek yanıt karar verir. Bir denetleyiciden görünüm şablonları oluşturmak ve bir HTML yanıtını tarayıcıya biçimlendirmek için kullanılabilir.

Denetleyicileri sırada bir yanıt işlemek bir görünüm şablon için gerekli olan veriler sağlamaktan sorumludur. En iyi yöntem: Görünüm şablonları gereken **değil** iş mantığını gerçekleştirebilir veya bir veritabanı ile doğrudan etkileşim. Bunun yerine, bir şablonu görüntüleme için denetleyici tarafından sağlanan veri ile çalışması gerekir. Bu "ayrımı" bakımı, temiz, sınanabilir ve sürdürülebilir Kod tutmaya yardımcı olur.

Şu anda `Welcome` yönteminde `HelloWorldController` sınıfı alır bir `name` ve `ID` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine bir şablonu görüntüleme kullanmayı denetleyici değiştirin. Şablonu Görüntüle uygun veri bitleri denetleyiciden görünüm yanıtı oluşturmak için geçirilmesi gerektiğini yani dinamik bir yanıt oluşturur. Şablonu görüntüleme, gereken dinamik (Parametreler) verilerinizden denetleyicisi sağlayarak bunu bir `ViewData` görünüm şablonu erişebiliyorsa sözlüğü.

İçinde *HelloWorldController.cs*, değiştirme `Welcome` yöntemi eklemek için bir `Message` ve `NumTimes` değerini `ViewData` sözlüğü. `ViewData` Sözlüktür her türlü kullanılabilir; yani dinamik bir nesne `ViewData` nesnesi içine yerleştirin kadar tanımlı hiçbir özellik sahiptir. [MVC model bağlama sistemi](xref:mvc/models/model-binding) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Sözlük nesnesi, görünüme iletilen verileri içerir. 

Adlı bir Hoş Geldiniz görünüm şablonu oluşturma *Views/HelloWorld/Welcome.cshtml*.

Bir döngüde oluşturacaksınız *Welcome.cshtml* "Hello" görüntüleyen bir görünüm şablonu `NumTimes`. Öğesinin içeriğini değiştirin *Views/HelloWorld/Welcome.cshtml* aşağıdaki:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Yaptığınız değişiklikleri kaydedin ve aşağıdaki URL'ye gidin:

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Verileri URL'den gerçekleştirilen ve denetleyicisi kullanarak geçirilen [MVC model bağlayıcı](xref:mvc/models/model-binding) . Denetleyici verileri paketleri bir `ViewData` sözlük ve Görünüm nesnesi geçer. Görünümü ardından verilerin tarayıcıya HTML olarak işler.

![Hoş Geldiniz bir etiket ve Hello dört kez gösterilen Rick ifadesini gösteren gizlilik görünümü](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

Yukarıdaki örnekteki `ViewData` denetleyicisinden bir görünüme veri iletmek için kullanılan sözlüğü. Öğreticinin sonraki bölümlerinde bir görünüm modeli, bir denetleyiciden bir görünüme veri iletmek için kullanılır. Veri geçirme görünüm modeli yaklaşım genellikle çok tercih üzerinden `ViewData` sözlük yaklaşım. Bkz: [ViewBag, ViewData veya TempData kullanıldığı durumlar ](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) daha fazla bilgi için.

Sonraki öğreticide, filmler, bir veritabanı oluşturulur.

> [!div class="step-by-step"]
> [Önceki](adding-controller.md)
> [İleri](adding-model.md)
