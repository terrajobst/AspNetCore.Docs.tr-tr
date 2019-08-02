---
title: ASP.NET Core MVC uygulamasına görünüm ekleme
author: rick-anderson
description: Basit bir ASP.NET Core MVC uygulamasına görünüm ekleme
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: b6a3b1e673b8b67530b82c7d850e810a6c11e555
ms.sourcegitcommit: 3204bc89ae6354b61ee0a9b2770ebe5214b7790c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707884"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC uygulamasına görünüm ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Bu bölümde, bir istemciye HTML `HelloWorldController` yanıtları oluşturma işlemini düzgün bir şekilde kapsüllemek için [Razor](xref:mvc/views/razor) görünüm dosyalarını kullanmak için sınıfını değiştirirsiniz.

Razor kullanarak bir görünüm şablonu dosyası oluşturursunuz. Razor tabanlı görünüm şablonlarının *. cshtml* dosya uzantısı vardır. Bu kişiler ile C#HTML çıktısı oluşturmanın zarif bir yolunu sağlarlar.

Şu anda `Index` yöntemi, denetleyici sınıfında sabit kodlanmış bir ileti içeren bir dize döndürür. Sınıfında, `Index` yöntemini aşağıdaki kodla değiştirin: `HelloWorldController`

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Önceki kod denetleyicinin <xref:Microsoft.AspNetCore.Mvc.Controller.View*> yöntemini çağırır. HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Yukarıdaki `Index` Yöntem gibi denetleyici Yöntemleri ( *eylem yöntemleri*olarak da bilinir), genellikle gibi `string`bir tür değil, <xref:Microsoft.AspNetCore.Mvc.IActionResult> genellikle bir (veya öğesinden <xref:Microsoft.AspNetCore.Mvc.ActionResult>türetilmiş bir sınıf) döndürür.

## <a name="add-a-view"></a>Görünüm ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Görünümler* klasörüne sağ tıklayın ve ardından **Yeni > klasör ekleyin** ve *HelloWorld*klasörünü adlandırın.

* *Görünümler/HelloWorld* klasörüne sağ tıklayın ve ardından **> yeni öğe ekleyin**.

* **Yeni öğe Ekle-Mvcfilmi** iletişim kutusunda

  * Sağ üst köşedeki arama kutusuna *Görünüm* girin

  * **Razor görünümü** seçin

  * *Index. cshtml* **adlı ad** kutusu değerini saklayın.

  * **Ekle** 'yi seçin

![Yeni öğe Ekle iletişim kutusu](adding-view/_static/add_view.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

İçin bir `Index` görünüm ekleyin. `HelloWorldController`

* *Views/HelloWorld*adlı yeni bir klasör ekleyin.
* *Views/HelloWorld* klasör adı *Index. cshtml*dosyasına yeni bir dosya ekleyin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* *Görünümler* klasörüne sağ tıklayın ve ardından **Yeni > klasör ekleyin** ve *HelloWorld*klasörünü adlandırın.
* *Görünümler/HelloWorld* klasörüne sağ tıklayın ve ardından **> yeni dosya ekleyin**.
* İçinde **yeni dosya** iletişim:

  * Sol bölmedeki **Web** ' i seçin.
  * Orta bölmedeki **boş HTML dosyasını** seçin.
  * **Ad** kutusuna *Index. cshtml* yazın.
  * **Yeni**' yi seçin.

![Yeni öğe Ekle iletişim kutusu](adding-view/_static/add_view.png)

---

*Views/HelloWorld/Index. cshtml* Razor görünüm dosyasının içeriğini aşağıdakiler ile değiştirin:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]


          `https://localhost:xxxx/HelloWorld` sayfasına gidin. `return View();`' Deki `Index` yöntemiçokfazladeğil,yönteminintarayıcıyayanıtişlemekiçinbirgörünümşablonu`HelloWorldController` dosyası kullanması gerektiğini belirten ifadesini çalıştırdı. Bir görünüm şablonu dosya adı belirtilmediğinden, MVC varsayılan görünüm dosyasını kullanmaya göre varsayılan olarak ayarlanmış. Varsayılan görünüm dosyası yöntemiyle aynı ada sahiptir (`Index`), bu nedenle */views/HelloWorld/Index.cshtml* kullanılır. Aşağıdaki görüntüde "görünüm Şablonumuzdan Merhaba!" dizesi gösterilmektedir görünümde sabit kodlanmış.

![Tarayıcı penceresi](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>Görünümleri ve düzen sayfalarını değiştirme

Menü bağlantılarını (**Mvcmovie**, **Home**ve **Gizlilik**) seçin. Her sayfada aynı menü düzeni gösterilir. Menü düzeni *Görünümler/Shared/_Layout. cshtml* dosyasında uygulanır. *Views/Shared/_Layout. cshtml* dosyasını açın.

[Düzen](xref:mvc/views/layout) şablonları, sitenizin HTML kapsayıcı yerleşimini tek bir yerde belirtmenize ve sonra sitenizdeki birden çok sayfaya uygulamanıza olanak tanır. `@RenderBody()` Satırı bulun. `RenderBody`, oluşturduğunuz tüm görünüme özgü sayfaların, Düzen sayfasında *kaydırılan* bir yer tutucudur. Örneğin, **Gizlilik** bağlantısını seçerseniz, **Görünümler/Home/privacy. cshtml** görünümü `RenderBody` yöntemin içinde işlenir.

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>Düzen dosyasındaki başlık, altbilgi ve menü bağlantısını değiştirme

*Views\shared\_Layout. cshtml* dosyasının içeriğini aşağıdaki biçimlendirme ile değiştirin. Değişiklikler vurgulanır:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Views/Shared/_Layout.cshtml?highlight=6,14,40)]

Yukarıdaki biçimlendirme aşağıdaki değişiklikleri yaptı:

* ' nin `MvcMovie` ' öğesine `Movie App`tekrarı.
* Öğesine bağla öğesi `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>`. `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`

Önceki biçimlendirmede, `asp-area=""` bu uygulama [alan](xref:mvc/controllers/areas)kullandığından [tutturucu etiketi Yardımcısı özniteliği](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ve öznitelik değeri atlandı.

**Not**: `Movies` Denetleyici uygulanmadı. Bu noktada `Movie App` bağlantı işlevsel değildir.

Değişikliklerinizi kaydedin ve **Gizlilik** bağlantısını seçin. Tarayıcı sekmesindeki başlığın Gizlilik ilkesi yerine bir **film uygulaması** (Gizlilik ilkesi değil) nasıl görüntülediğini fark edin **-MVC filmi**:

![Gizlilik sekmesi](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

**Giriş** bağlantısını seçin ve başlık ve bağlantı metninin **film uygulamasını**da görüntülediğine dikkat edin. Düzen şablonunda değişikliği bir kez yapabildik ve sitedeki tüm sayfalar yeni bağlantı metnini ve yeni başlığı yansıtmaktadır.

*Views/_ViewStart. cshtml* dosyasını inceleyin:

```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart. cshtml* dosyası, *Görünümler/Shared/_Layout. cshtml* dosyasını her bir görünüme getirir. Özelliği, farklı bir düzen görünümü ayarlamak veya bir düzen dosyası kullanılmayacak `null` şekilde ayarlamak için kullanılabilir. `Layout`

`<h2>` *Views/HelloWorld/Index. cshtml* görünüm dosyasının başlığını ve öğesini değiştirin:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

Başlık ve `<h2>` öğe biraz farklı olduğundan, hangi kod bitini görüntülemeyi değiştirmekten daha fazla bilgi alabilirsiniz.

`ViewData["Title"] = "Movie List";`Yukarıdaki kodda, `Title` `ViewData` sözlüğün özelliğini "film listesi" olarak ayarlar. Özelliği, Düzen sayfasındaki `<title>` HTML öğesinde kullanılır: `Title`

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Değişikliği kaydedin ve öğesine `https://localhost:xxxx/HelloWorld`gidin. Tarayıcı başlığı, birincil başlık ve ikincil başlıkların değiştirildiğini unutmayın. (Tarayıcıda değişiklik görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz. Sunucudan gelen yanıtı zorlamak için tarayıcınızda CTRL + F5 tuşlarına basın.) Tarayıcı başlığı *Index. cshtml* Görünüm `ViewData["Title"]` şablonunda ve düzen dosyasına eklenen ek "-film uygulaması" olarak ayarlandığımızda oluşturulur.

*Index. cshtml* görünüm şablonundaki içerik *views/Shared/_Layout. cshtml* görünüm şablonuyla birleştirilir. Tarayıcıya tek bir HTML yanıtı gönderilir. Düzen şablonları, bir uygulamadaki tüm sayfalara uygulanan değişiklikler yapmayı kolaylaştırır. Daha fazla bilgi için bkz. [Düzen](xref:mvc/views/layout).

![Film listesi görünümü](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

"Data" (Bu durumda "Görünümümüzden Merhaba!") çok az. ileti) sabit kodludur, ancak. MVC uygulamasında bir "V" (görünüm) var ve bir "C" (denetleyici) var, ancak henüz "M" (model) yok.

## <a name="passing-data-from-the-controller-to-the-view"></a>Denetleyiciden görünüme veri geçirme

Gelen URL isteğine yanıt olarak denetleyici eylemleri çağrılır. Bir denetleyici sınıfı, gelen tarayıcı isteklerini işleyen kodun yazıldığı yerdir. Denetleyici verileri bir veri kaynağından alır ve tarayıcıya ne tür bir yanıt gönderileceğini belirler. Görünüm şablonları bir denetleyiciden, tarayıcıya HTML yanıtı oluşturmak ve biçimlendirmek için kullanılabilir.

Bir görünüm şablonunun yanıt işlemesi için gereken verileri sağlamaktan denetleyiciler sorumludur. En iyi uygulama: Görünüm şablonları iş **mantığı gerçekleştirmemelidir** veya doğrudan bir veritabanıyla etkileşime girmemelidir. Bunun yerine, bir görünüm şablonu yalnızca denetleyici tarafından sunulan verilerle birlikte çalışmalıdır. Bu "kaygıları ayrımı", kodun temiz, test edilebilir ve sürdürülebilir kalmasına yardımcı olur.

Şu anda, `Welcome` `HelloWorldController` sınıfındakiyöntemi`name` bir ve parametresialırvesonradeğerleridoğrudantarayıcıyaçıkarır.`ID` Denetleyicinin bu yanıtı bir dize olarak işlemesini sağlamak yerine, denetleyiciyi bir görünüm şablonu kullanacak şekilde değiştirin. Görünüm şablonu dinamik bir yanıt üretir, bu, yanıtı oluşturmak için denetleyiciden görünüme uygun veri bitlerinin geçirilmesi gereken anlamına gelir. Bu, denetleyicinin görünüm şablonu tarafından daha sonra erişebileceği bir `ViewData` sözlükte görünüm şablonunun gerektirdiği dinamik verileri (parametreler) yerleştirerek bunu yapın.

*HelloWorldController.cs*içinde `Welcome` , `Message` sözlüğe bir ve `NumTimes` değeri eklemek için yöntemini değiştirin. `ViewData` Sözlük dinamik bir nesnedir, yani herhangi bir tür kullanılabilir `ViewData` ; nesnenin içine bir öğe yerleştirene kadar tanımlanmış bir özelliği yoktur. `ViewData` [MVC modeli bağlama sistemi](xref:mvc/models/model-binding) , adlandırılmış parametreleri (`name` ve `numTimes`) adres çubuğundaki sorgu dizesinden, yöntemizdeki parametrelere otomatik olarak eşler. Tüm *HelloWorldController.cs* dosyası şuna benzer:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Sözlük nesnesi görünüme geçirilecek verileri içerir.

*Görünümler/HelloWorld/Welcome. cshtml*adlı bir hoş geldiniz görünüm şablonu oluşturun.

*Welcome. cshtml* görünüm şablonunda "Hello" `NumTimes`öğesini görüntüleyen bir döngü oluşturacaksınız. *Views/HelloWorld/Welcome. cshtml* içeriğini aşağıdakiler ile değiştirin:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Değişikliklerinizi kaydedin ve aşağıdaki URL 'ye gidin:

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Veriler URL 'den alınır ve [MVC model Bağlayıcısı](xref:mvc/models/model-binding) kullanılarak denetleyiciye geçirilir. Denetleyici, verileri bir `ViewData` sözlükte paketler ve bu nesneyi görünüme geçirir. Daha sonra Görünüm, verileri tarayıcıda HTML olarak işler.

![Bir hoş geldiniz etiketi ve dört kez gösterilen Hello Rick ifadesi gösteren gizlilik görünümü](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

Yukarıdaki `ViewData` örnekte sözlük, denetleyicideki verileri bir görünüme geçirmek için kullanıldı. Öğreticide daha sonra bir görünüm modeli, bir denetleyicideki verileri bir görünüme geçirmek için kullanılır. Veri geçirme yaklaşımına yönelik görünüm modeli, `ViewData` sözlük yaklaşımına göre genel olarak tercih edilir. Daha fazla bilgi için bkz. [ViewBag, ViewData veya TempData kullanma](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) .

Sonraki öğreticide, bir film veritabanı oluşturulur.

> [!div class="step-by-step"]
> [Önceki](adding-controller.md)İleri
> [](adding-model.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu bölümde, bir istemciye HTML `HelloWorldController` yanıtları oluşturma işlemini düzgün bir şekilde kapsüllemek için [Razor](xref:mvc/views/razor) görünüm dosyalarını kullanmak için sınıfını değiştirirsiniz.

Razor kullanarak bir görünüm şablonu dosyası oluşturursunuz. Razor tabanlı görünüm şablonlarının *. cshtml* dosya uzantısı vardır. Bu kişiler ile C#HTML çıktısı oluşturmanın zarif bir yolunu sağlarlar.

Şu anda `Index` yöntemi, denetleyici sınıfında sabit kodlanmış bir ileti içeren bir dize döndürür. Sınıfında, `Index` yöntemini aşağıdaki kodla değiştirin: `HelloWorldController`

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Önceki kod denetleyicinin <xref:Microsoft.AspNetCore.Mvc.Controller.View*> yöntemini çağırır. HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Yukarıdaki `Index` Yöntem gibi denetleyici Yöntemleri ( *eylem yöntemleri*olarak da bilinir), genellikle gibi `string`bir tür değil, <xref:Microsoft.AspNetCore.Mvc.IActionResult> genellikle bir (veya öğesinden <xref:Microsoft.AspNetCore.Mvc.ActionResult>türetilmiş bir sınıf) döndürür.

## <a name="add-a-view"></a>Görünüm ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Görünümler* klasörüne sağ tıklayın ve ardından **Yeni > klasör ekleyin** ve *HelloWorld*klasörünü adlandırın.

* *Görünümler/HelloWorld* klasörüne sağ tıklayın ve ardından **> yeni öğe ekleyin**.

* **Yeni öğe Ekle-Mvcfilmi** iletişim kutusunda

  * Sağ üst köşedeki arama kutusuna *Görünüm* girin

  * **Razor görünümü** seçin

  * *Index. cshtml* **adlı ad** kutusu değerini saklayın.

  * **Ekle** 'yi seçin

![Yeni öğe Ekle iletişim kutusu](adding-view/_static/add_view.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

İçin bir `Index` görünüm ekleyin. `HelloWorldController`

* *Views/HelloWorld*adlı yeni bir klasör ekleyin.
* *Views/HelloWorld* klasör adı *Index. cshtml*dosyasına yeni bir dosya ekleyin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* *Görünümler* klasörüne sağ tıklayın ve ardından **Yeni > klasör ekleyin** ve *HelloWorld*klasörünü adlandırın.
* *Görünümler/HelloWorld* klasörüne sağ tıklayın ve ardından **> yeni dosya ekleyin**.
* İçinde **yeni dosya** iletişim:

  * Sol bölmedeki **Web** ' i seçin.
  * Orta bölmedeki **boş HTML dosyasını** seçin.
  * **Ad** kutusuna *Index. cshtml* yazın.
  * **Yeni**' yi seçin.

![Yeni öğe Ekle iletişim kutusu](adding-view/_static/add_view_mac.png)

---

*Views/HelloWorld/Index. cshtml* Razor görünüm dosyasının içeriğini aşağıdakiler ile değiştirin:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]


          `https://localhost:xxxx/HelloWorld` sayfasına gidin. `return View();`' Deki `Index` yöntemiçokfazladeğil,yönteminintarayıcıyayanıtişlemekiçinbirgörünümşablonu`HelloWorldController` dosyası kullanması gerektiğini belirten ifadesini çalıştırdı. Bir görünüm şablonu dosya adı belirtilmediğinden, MVC varsayılan görünüm dosyasını kullanmaya göre varsayılan olarak ayarlanmış. Varsayılan görünüm dosyası yöntemiyle aynı ada sahiptir (`Index`), bu nedenle */views/HelloWorld/Index.cshtml* kullanılır. Aşağıdaki görüntüde "görünüm Şablonumuzdan Merhaba!" dizesi gösterilmektedir görünümde sabit kodlanmış.

![Tarayıcı penceresi](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>Görünümleri ve düzen sayfalarını değiştirme

Menü bağlantılarını (**Mvcmovie**, **Home**ve **Gizlilik**) seçin. Her sayfada aynı menü düzeni gösterilir. Menü düzeni *Görünümler/Shared/_Layout. cshtml* dosyasında uygulanır. *Views/Shared/_Layout. cshtml* dosyasını açın.

[Düzen](xref:mvc/views/layout) şablonları, sitenizin HTML kapsayıcı yerleşimini tek bir yerde belirtmenize ve sonra sitenizdeki birden çok sayfaya uygulamanıza olanak tanır. `@RenderBody()` Satırı bulun. `RenderBody`, oluşturduğunuz tüm görünüme özgü sayfaların, Düzen sayfasında *kaydırılan* bir yer tutucudur. Örneğin, **Gizlilik** bağlantısını seçerseniz, **Görünümler/Home/privacy. cshtml** görünümü `RenderBody` yöntemin içinde işlenir.

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>Düzen dosyasındaki başlık, altbilgi ve menü bağlantısını değiştirme

* Başlık ve altbilgi öğelerinde öğesini olarak `MvcMovie` `Movie App`değiştirin.
* Tutturucu öğesini `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` olarak `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`değiştirin.

Aşağıdaki biçimlendirme vurgulanan değişiklikleri göstermektedir:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24,51)]

Önceki biçimlendirmede, `asp-area` bu uygulama [alan](xref:mvc/controllers/areas)kullandığından [tutturucu etiketi yardımcı özniteliği](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) atlandı.

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

**Not**: `Movies` Denetleyici uygulanmadı. Bu noktada `Movie App` bağlantı işlevsel değildir.

Değişikliklerinizi kaydedin ve **Gizlilik** bağlantısını seçin. Tarayıcı sekmesindeki başlığın Gizlilik ilkesi yerine bir **film uygulaması** (Gizlilik ilkesi değil) nasıl görüntülediğini fark edin **-MVC filmi**:

![Gizlilik sekmesi](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

**Giriş** bağlantısını seçin ve başlık ve bağlantı metninin **film uygulamasını**da görüntülediğine dikkat edin. Düzen şablonunda değişikliği bir kez yapabildik ve sitedeki tüm sayfalar yeni bağlantı metnini ve yeni başlığı yansıtmaktadır.

*Views/_ViewStart. cshtml* dosyasını inceleyin:

```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart. cshtml* dosyası, *Görünümler/Shared/_Layout. cshtml* dosyasını her bir görünüme getirir. Özelliği, farklı bir düzen görünümü ayarlamak veya bir düzen dosyası kullanılmayacak `null` şekilde ayarlamak için kullanılabilir. `Layout`

`<h2>` *Views/HelloWorld/Index. cshtml* görünüm dosyasının başlığını ve öğesini değiştirin:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

Başlık ve `<h2>` öğe biraz farklı olduğundan, hangi kod bitini görüntülemeyi değiştirmekten daha fazla bilgi alabilirsiniz.

`ViewData["Title"] = "Movie List";`Yukarıdaki kodda, `Title` `ViewData` sözlüğün özelliğini "film listesi" olarak ayarlar. Özelliği, Düzen sayfasındaki `<title>` HTML öğesinde kullanılır: `Title`

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Değişikliği kaydedin ve öğesine `https://localhost:xxxx/HelloWorld`gidin. Tarayıcı başlığı, birincil başlık ve ikincil başlıkların değiştirildiğini unutmayın. (Tarayıcıda değişiklik görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz. Sunucudan gelen yanıtı zorlamak için tarayıcınızda CTRL + F5 tuşlarına basın.) Tarayıcı başlığı *Index. cshtml* Görünüm `ViewData["Title"]` şablonunda ve düzen dosyasına eklenen ek "-film uygulaması" olarak ayarlandığımızda oluşturulur.

Ayrıca, *Index. cshtml* görünüm şablonundaki içeriğin *Görünümler/Shared/_Layout. cshtml* görünüm şablonuyla nasıl birleştirildiğine ve tarayıcıya tek bir HTML yanıtının gönderildiğine dikkat edin. Düzen şablonları, uygulamanızdaki tüm sayfalara uygulanan değişiklikler yapmayı gerçekten kolaylaştırır. Daha fazla bilgi için bkz. [Düzen](xref:mvc/views/layout).

![Film listesi görünümü](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

"Data" (Bu durumda "Görünümümüzden Merhaba!") çok az. ileti) sabit kodludur, ancak. MVC uygulamasında bir "V" (görünüm) var ve bir "C" (denetleyici) var, ancak henüz "M" (model) yok.

## <a name="passing-data-from-the-controller-to-the-view"></a>Denetleyiciden görünüme veri geçirme

Gelen URL isteğine yanıt olarak denetleyici eylemleri çağrılır. Bir denetleyici sınıfı, gelen tarayıcı isteklerini işleyen kodun yazıldığı yerdir. Denetleyici verileri bir veri kaynağından alır ve tarayıcıya ne tür bir yanıt gönderileceğini belirler. Görünüm şablonları bir denetleyiciden, tarayıcıya HTML yanıtı oluşturmak ve biçimlendirmek için kullanılabilir.

Bir görünüm şablonunun yanıt işlemesi için gereken verileri sağlamaktan denetleyiciler sorumludur. En iyi uygulama: Görünüm şablonları iş **mantığı gerçekleştirmemelidir** veya doğrudan bir veritabanıyla etkileşime girmemelidir. Bunun yerine, bir görünüm şablonu yalnızca denetleyici tarafından sunulan verilerle birlikte çalışmalıdır. Bu "kaygıları ayrımı", kodun temiz, test edilebilir ve sürdürülebilir kalmasına yardımcı olur.

Şu anda, `Welcome` `HelloWorldController` sınıfındakiyöntemi`name` bir ve parametresialırvesonradeğerleridoğrudantarayıcıyaçıkarır.`ID` Denetleyicinin bu yanıtı bir dize olarak işlemesini sağlamak yerine, denetleyiciyi bir görünüm şablonu kullanacak şekilde değiştirin. Görünüm şablonu dinamik bir yanıt üretir, bu, yanıtı oluşturmak için denetleyiciden görünüme uygun veri bitlerinin geçirilmesi gereken anlamına gelir. Bu, denetleyicinin görünüm şablonu tarafından daha sonra erişebileceği bir `ViewData` sözlükte görünüm şablonunun gerektirdiği dinamik verileri (parametreler) yerleştirerek bunu yapın.

*HelloWorldController.cs*içinde `Welcome` , `Message` sözlüğe bir ve `NumTimes` değeri eklemek için yöntemini değiştirin. `ViewData` Sözlük dinamik bir nesnedir, yani herhangi bir tür kullanılabilir `ViewData` ; nesnenin içine bir öğe yerleştirene kadar tanımlanmış bir özelliği yoktur. `ViewData` [MVC modeli bağlama sistemi](xref:mvc/models/model-binding) , adlandırılmış parametreleri (`name` ve `numTimes`) adres çubuğundaki sorgu dizesinden, yöntemizdeki parametrelere otomatik olarak eşler. Tüm *HelloWorldController.cs* dosyası şuna benzer:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Sözlük nesnesi görünüme geçirilecek verileri içerir.

*Görünümler/HelloWorld/Welcome. cshtml*adlı bir hoş geldiniz görünüm şablonu oluşturun.

*Welcome. cshtml* görünüm şablonunda "Hello" `NumTimes`öğesini görüntüleyen bir döngü oluşturacaksınız. *Views/HelloWorld/Welcome. cshtml* içeriğini aşağıdakiler ile değiştirin:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Değişikliklerinizi kaydedin ve aşağıdaki URL 'ye gidin:

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Veriler URL 'den alınır ve [MVC model Bağlayıcısı](xref:mvc/models/model-binding) kullanılarak denetleyiciye geçirilir. Denetleyici, verileri bir `ViewData` sözlükte paketler ve bu nesneyi görünüme geçirir. Daha sonra Görünüm, verileri tarayıcıda HTML olarak işler.

![Bir hoş geldiniz etiketi ve dört kez gösterilen Hello Rick ifadesi gösteren gizlilik görünümü](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

Yukarıdaki `ViewData` örnekte sözlük, denetleyicideki verileri bir görünüme geçirmek için kullanıldı. Öğreticide daha sonra bir görünüm modeli, bir denetleyicideki verileri bir görünüme geçirmek için kullanılır. Veri geçirme yaklaşımına yönelik görünüm modeli, `ViewData` sözlük yaklaşımına göre genel olarak tercih edilir. Daha fazla bilgi için bkz. [ViewBag, ViewData veya TempData kullanma](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) .

Sonraki öğreticide, bir film veritabanı oluşturulur.

> [!div class="step-by-step"]
> [Önceki](adding-controller.md)İleri
> [](adding-model.md)

::: moniker-end