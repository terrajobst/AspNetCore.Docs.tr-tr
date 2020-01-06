---
title: ASP.NET Core içindeki denetleyici yöntemleri ve görünümleri
author: rick-anderson
description: ASP.NET Core 'de denetleyici yöntemleri, görünümler ve Dataaçıklamalarla çalışmayı öğrenin.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 2c442060872ab1d2d79a2e355ae257fdf1005914
ms.sourcegitcommit: 991442dfb16ef08a0aae05bc79f9e9a2d819c587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75492649"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>ASP.NET Core içindeki denetleyici yöntemleri ve görünümleri

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulamasına iyi başladık, ancak sunu ideal değildir, örneğin, **ReleaseDate** iki sözcükten oluşmalıdır.

![Dizin görünümü: Yayın tarihi bir sözcükdür (boşluk yok) ve her film yayın tarihi 12 saat içinde görüntülenir](working-with-sql/_static/m55.png)

*Modeller/film. cs* dosyasını açın ve vurgulanan satırları aşağıda gösterildiği gibi ekleyin:

[!code-csharp[](start-mvc/sample/MvcMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

Sonraki öğreticide [veri ek açıklamalarını](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) ele aldık. [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği bir alanın adı için (Bu durumda "ReleaseDate" yerine "Yayın tarihi") görüntüleneceğini belirtir. [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih), bu nedenle alanda depolanan zaman bilgileri gösterilmez.

Entity Framework Core `[Column(TypeName = "decimal(18, 2)")]` veri ek açıklaması gerekir, bu nedenle `Price` veritabanındaki para birimine doğru şekilde eşleyebilir. Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).

`Movies` denetleyicisine gidin ve hedef URL 'yi görmek için fare işaretçisini bir **düzenleme** bağlantısı üzerine tutun.

![Düzenleme bağlantısı üzerinde fare olan tarayıcı penceresi ve https://localhost:5001/Movies/Edit/5 bağlantı URL 'Si gösteriliyor](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

**Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Görünümler/fılmler/Index. cshtml* dosyasındaki Core MVC bağlayıcı etiketi Yardımcısı tarafından oluşturulur.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Yukarıdaki kodda `AnchorTagHelper`, denetleyici eylem yönteminden ve yol kimliğinden HTML `href` öznitelik değerini dinamik olarak oluşturur. En sevdiğiniz tarayıcıdan **Görünüm kaynağını** kullanıyorsunuz veya oluşturulan biçimlendirmeyi incelemek için geliştirici araçlarını kullanıyorsunuz. Oluşturulan HTML 'nin bir bölümü aşağıda gösterilmiştir:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

*Startup.cs* dosyasındaki [yönlendirme](xref:mvc/controllers/routing) kümesi için biçimi geri çekin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core, `https://localhost:5001/Movies/Edit/4` `Movies` `Edit` Action yöntemine bir isteğe çevirir, 4 parametresi `Id`. (Denetleyici yöntemleri de eylem yöntemleri olarak bilinir.)

[Etiket yardımcıları](xref:mvc/views/tag-helpers/intro) ASP.NET Core ' deki en popüler yeni özelliklerden biridir. Daha fazla bilgi için bkz. [ek kaynaklar](#additional-resources).

`Movies` denetleyiciyi açın ve iki `Edit` eylemi yöntemini inceleyin. Aşağıdaki kod, filmi getiren ve *Edit. cshtml* Razor dosyası tarafından oluşturulan düzenleme formunu dolduran `HTTP GET Edit` yöntemini gösterir.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Aşağıdaki kod, postalanan film değerlerini işleyen `HTTP POST Edit` yöntemini gösterir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Aşağıdaki kod, postalanan film değerlerini işleyen `HTTP POST Edit` yöntemini gösterir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[Bind]` özniteliği, daha [fazla](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost)gönderim için korumanın bir yoludur. Yalnızca değiştirmek istediğiniz `[Bind]` özniteliğine özellikler dahil etmelisiniz. Daha fazla bilgi için, bkz. [denetleyicinizi yeniden gönderme durumundan koruma](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application). [Viewmodeller](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) , daha fazla nakletmeyi engellemek için alternatif bir yaklaşım sağlar.

İkinci `Edit` Action yönteminin önünde `[HttpPost]` özniteliği olduğuna dikkat edin.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

`HttpPost` özniteliği, bu `Edit` yönteminin *yalnızca* `POST` istekleri için çağrılabileceğini belirtir. `[HttpGet]` özniteliğini ilk düzenleme yöntemine uygulayabilirsiniz, ancak `[HttpGet]` varsayılan olduğundan bu gerekli değildir.

`ValidateAntiForgeryToken` özniteliği, [bir isteğin sahteciliği engellemek](xref:security/anti-request-forgery) için kullanılır ve düzenleme görünümü dosyasında (*Görünümler/filmler/Düzenle. cshtml*) oluşturulan bir güvenlik yumuşatma belirteciyle eşleştirilir. Düzenleme görünümü dosyası, [form etiketi Yardımcısı](xref:mvc/views/working-with-forms)ile karşı koruma belirteci oluşturur.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Form etiketi Yardımcısı](xref:mvc/views/working-with-forms) , film denetleyicisinin `Edit` yönteminde üretilen `[ValidateAntiForgeryToken]` Anti-forgery belirteci ile eşleşmesi gereken gizli bir Anti-forgery belirteci oluşturur. Daha fazla bilgi için bkz. [Istek önleyici](xref:security/anti-request-forgery)güvenlik.

`HttpGet Edit` yöntemi, film `ID` parametresini alır, Entity Framework `FindAsync` yöntemini kullanarak filmi arar ve seçili filmi düzenleme görünümüne döndürür. Bir film bulunamazsa, `NotFound` (HTTP 404) döndürülür.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Scafkatlama sistemi düzenleme görünümü oluştururken, `Movie` sınıfını inceledi ve sınıfın her özelliği için `<label>` ve `<input>` öğelerini işlemek üzere kodu oluşturmuştur. Aşağıdaki örnekte, Visual Studio scafkatlama sistemi tarafından oluşturulan düzenleme görünümü gösterilmektedir:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/EditOriginal.cshtml)]

Görünüm şablonunun dosyanın en üstünde bir `@model MvcMovie.Models.Movie` bildirimine sahip olduğuna dikkat edin. `@model MvcMovie.Models.Movie` görünümün görünüm şablonunun modelinin `Movie`türünde olmasını beklediğini belirtir.

Scafkatlanmış kod, HTML işaretlemesini kolaylaştırmak için birkaç etiket Yardımcısı yöntemi kullanır. \- [Label etiket Yardımcısı](xref:mvc/views/working-with-forms) alanın adını ("title", "ReleaseDate", "tarz" veya "Price") görüntüler. [Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) bir HTML `<input>` öğesi işler. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms) , bu özellikle ilişkili tüm doğrulama iletilerini görüntüler.

Uygulamayı çalıştırın ve `/Movies` URL 'sine gidin. Bir **düzenleme** bağlantısına tıklayın. Tarayıcıda, sayfanın kaynağını görüntüleyin. `<form>` öğesi için oluşturulan HTML aşağıda gösterilmiştir.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` öğeleri `action` özniteliği `/Movies/Edit/id` URL 'ye gönderi olarak ayarlanan bir `HTML <form>` öğesidir. Form verileri `Save` düğmesine tıklandığında sunucuya gönderilir. Kapanış `</form>` öğesinden önceki son satır, [form etiketi Yardımcısı](xref:mvc/views/working-with-forms)tarafından oluşturulan gizli [XSRF](xref:security/anti-request-forgery) belirtecini gösterir.

## <a name="processing-the-post-request"></a>POST Isteği işleniyor

Aşağıdaki listede `Edit` eylem yönteminin `[HttpPost]` sürümü gösterilmektedir.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[ValidateAntiForgeryToken]` öznitelik, [form etiketi Yardımcısı](xref:mvc/views/working-with-forms) 'nda Anti-forgery belirteç Oluşturucu tarafından oluşturulan gizli [XSRF](xref:security/anti-request-forgery) belirtecini doğrular

[Model bağlama](xref:mvc/models/model-binding) sistemi, postalanan form değerlerini alır ve `movie` parametresi olarak geçirilmiş bir `Movie` nesnesi oluşturur. `ModelState.IsValid` yöntemi, formda gönderilen verilerin bir `Movie` nesnesini değiştirmek (düzenlemek veya güncelleştirmek) için kullanılabileceğini doğrular. Veriler geçerliyse, kaydedilir. Güncelleştirilmiş (düzenlenmiş) film verileri veritabanı bağlamının `SaveChangesAsync` yöntemi çağırarak veritabanına kaydedilir. Veriler kaydedildikten sonra, kod, yeni yapılan değişiklikler de dahil olmak üzere, film koleksiyonunu görüntüleyen `MoviesController` sınıfının `Index` Action metoduna Kullanıcı yönlendirir.

Form sunucuya gönderilmeden önce, istemci tarafı doğrulaması alanlarda tüm doğrulama kurallarını denetler. Herhangi bir doğrulama hatası varsa, bir hata iletisi görüntülenir ve form nakledilmez. JavaScript devre dışıysa, istemci tarafı doğrulamaya sahip olmayacaktır, ancak sunucu geçerli olmayan gönderilen değerleri tespit eder ve form değerleri hata iletileriyle birlikte görüntülenir. Öğreticide daha sonra [model doğrulamayı](xref:mvc/models/validation) daha ayrıntılı bir şekilde inceleyeceğiz. *Görünümler/filmler/Edit. cshtml* görünüm şablonundaki [doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms) , uygun hata iletilerini görüntülemeyi üstlenir.

![Görünümü Düzenle: ABC durumlarının yanlış bir fiyat değeri için bir özel durum, alan fiyatının bir sayı olması gerektiğini belirtir. Xyz durumlarının yanlış Yayın tarihi değeri için bir özel durum lütfen geçerli bir tarih girin.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Film denetleyicisindeki tüm `HttpGet` Yöntemler benzer bir düzene uyar. Bunlar bir film nesnesi (veya `Index`bir nesne listesi) alır ve nesneyi (model) görünüme geçirebilir. `Create` yöntemi, `Create` görünümüne boş bir film nesnesi geçirir. Verileri oluşturan, düzenleme, silme ya da başka bir şekilde değiştiren tüm yöntemler, yönteminin `[HttpPost]` aşırı yüküne göre yapılır. `HTTP GET` yöntemdeki verileri değiştirme bir güvenlik riskidir. `HTTP GET` bir yöntemde verilerin değiştirilmesi, HTTP en iyi uygulamalarını ve mimari [rest](http://rest.elkstein.org/) modelini ihlal ediyor, bu da GET isteklerinin uygulamanızın durumunu değiştirmemesi gerektiğini belirtir. Diğer bir deyişle, bir GET işleminin gerçekleştirilmesi, yan etkileri olmayan ve kalıcı verilerinizi değiştirmeyen bir güvenli işlem olmalıdır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket yardımcılarına giriş](xref:mvc/views/tag-helpers/intro)
* [Yazar etiketi yardımcıları](xref:mvc/views/tag-helpers/authoring)
* [İstek Sahteciliğinden Koruma](xref:security/anti-request-forgery)
* Denetleyicinizi [yeniden gönderme](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application) durumundan koruma
* [ViewModel 'lar](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Form Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Giriş Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Etiket Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Seçim Etiketi Yardımcısı](xref:mvc/views/working-with-forms)
* [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms)

> [!div class="step-by-step"]
> [Önceki](working-with-sql.md)
> [İleri](search.md)  
