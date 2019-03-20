---
title: Bir ASP.NET Core MVC uygulaması için arama Ekle
author: rick-anderson
description: Basit bir ASP.NET Core MVC uygulaması için arama ekleme işlemi açıklanır
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 41d7494b77edaddbf719cab087142f0132dd3ed6
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208388"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için arama Ekle

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, arama özelliği ekleyin `Index` olanak sağlayan bir eylem yöntemi tarafından filmler arama *Tarz* veya *adı*.

Güncelleştirme `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

İlk satırı `Index` eylem yöntemi oluşturur bir [LINQ](/dotnet/standard/using-linq) filmler seçmek için sorgu:

```csharp
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.

Varsa `searchString` parametresi içeren bir dize, filmler sorgu arama dizesinin değerini temel filtrelemek için değiştirilir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

`s => s.Title.Contains()` Kodu yukarıdaki bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/standard/using-linq) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/api/system.linq.enumerable.where) yöntemi veya `Contains` (Yukarıdaki kod kullanılır). LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where`, `Contains`, veya `OrderBy`. Bunun yerine, sorgu yürütme ertelenir.  Anlamına gerçekleşen değeri gerçekten üzerinden yinelenir kadar bir ifade değerlendirmesi ertelendi veya `ToListAsync` yöntemi çağrılır. Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Not: [İçerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, yukarıda gösterilen c# kodunda değil, veritabanı üzerinde çalıştırılır. Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır. SQL Server'da [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu. SQLite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.

`/Movies/Index` sayfasına gidin. Bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'si. Filtrelenmiş filmler görüntülenir.

![Dizini görüntüle](~/tutorials/first-mvc-app/search/_static/ghost.png)

İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi isteğe bağlı olarak eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

Parametre değiştirme `id` ve tüm oluşumlarını `searchString` değiştirmek `id`.

Önceki `Index` yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

Güncelleştirilmiş `Index` yöntemiyle `id` parametresi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_SearchID)]

Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](~/tutorials/first-mvc-app/search/_static/g2.png)

Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Şimdi, filmler filtre yardımcı olmak için kullanıcı Arabirimi öğeleri ekleyeceksiniz. İmzası değiştirdiyseniz `Index` rota bağlı nasıl test etmek için yöntemi `ID` parametresi, yeniden adlandırılmış bir parametre alır, böylece değişiklik `searchString`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

Açık *Views/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme vurgulanmış aşağıda:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` etiketi kullanan [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms), formu gönderdiğinde, filtre dizesi nakledilir `Index` denetleyici filmler eylemi. Yaptığınız değişiklikleri kaydedin ve ardından filtre sınayın.

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](~/tutorials/first-mvc-app/search/_static/filter.png)

Var olan hiçbir `[HttpPost]` aşırı yükünü `Index` yöntemi olarak, beklediğiniz. Yöntemi, uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez.

Aşağıdaki ekleyebilirsiniz `[HttpPost] Index` yöntemi.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` Parametresi için bir aşırı yükleme oluşturmak için kullanılan `Index` yöntemi. Öğreticinin ilerleyen bölümlerinde, hakkında konuşacağız.

Eylem çağırıcısı, bu yöntem eklerseniz, BC `[HttpPost] Index` yöntemi ve `[HttpPost] Index` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.

![Tarayıcı penceresi ile HttpPost dizininin'ndan uygulama yanıt: ghost filtreleme](~/tutorials/first-mvc-app/search/_static/fo.png)

Ancak, bu eklediğiniz bile `[HttpPost]` sürümünü `Index` yöntemi nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur. Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklayabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün. HTTP POST isteği URL'sini GET isteği (localhost:xxxxx filmler/dizin) URL'si ile aynıdır; hiçbir arama bilgisi URL'sindeki dikkat edin. Arama dizesi bilgilerini sunucuya olarak gönderilen bir [form alanının değeri](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Tarayıcının geliştirici araçları veya mükemmel ile doğrulayabilirsiniz [Fiddler aracı](http://www.telerik.com/fiddler). Aşağıdaki resimde Chrome tarayıcı geliştirici araçları gösterilmektedir:

![Ghost AramaDizesi değeri istek gövdesi gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

Arama parametresi görebilirsiniz ve [XSRF](xref:security/anti-request-forgery) istek gövdesindeki belirteci. Unutmayın, önceki öğreticide belirtildiği gibi [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms) oluşturur bir [XSRF](xref:security/anti-request-forgery) sahteciliğe karşı koruma belirteci. Denetleyici yönteminin belirteci doğrulamak gerekmez için biz veri, değişiklik yaptığınız değil.

İstek gövdesini ve URL'yi arama parametresi olduğundan, bu arama bilgilere yer işareti yakalamak veya başkalarıyla paylaşmak. Düzeltme isteği belirterek olmalıdır `HTTP GET`:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGet.cshtml?highlight=12&range=1-23)]

Artık bir arama gönderdiğinizde, URL'yi arama sorgu dizesi içerir. Arama de gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.

![AramaDizesi gösteren tarayıcı penceresinde ghost = URL'sini ve döndürülen, filmler Ghostbusters ve Ghostbusters 2, word ghost içerir](~/tutorials/first-mvc-app/search/_static/search_get.png)

Aşağıdaki biçimlendirme değişikliği gösterir `form` etiketi:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="add-search-by-genre"></a>Türe göre arama Ekle

Aşağıdaki `MovieGenreViewModel` sınıfının *modelleri* klasörü:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Film Tarz görünüm modeli içerir:

* Filmler listesi.
* A `SelectList` türleri listesini içeren. Bu kullanıcının listeden bir türe izin verir.
* `MovieGenre`, seçilen türe içerir.
* `SearchString`, kullanıcıların, arama metin kutusuna girdiğiniz metnin içerir.

Değiştirin `Index` yönteminde `MoviesController.cs` aşağıdaki kod ile:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Aşağıdaki kod bir `LINQ` veritabanından tüm türleri alan sorgu.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_LINQ)]

`SelectList` Türleri (biz istemiyorsanız seçin listemize yinelenen tür sahip) farklı tür yansıtma tarafından oluşturulur.

Kullanıcı için olan öğeyle aradığında, arama kutusuna arama değeri korunur.

## <a name="add-search-by-genre-to-the-index-view"></a>Arama dizini görünümü türe göre Ekle

Güncelleştirme `Index.cshtml` gibi:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`

Önceki kodda, `DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği. Lambda ifadesi inceledi değerlendirilmesi yerine olduğundan, bir erişim ihlali almadığınız olduğunda `model`, `model.Movies`, veya `model.Movies[0]` olan `null` veya boş. Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

Uygulamayı Tarz, film adı ve her ikisi tarafından arama yaparak test edin:

![Tarayıcı penceresini gösteren sonuçları https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2](~/tutorials/first-mvc-app/search/_static/s2.png)

> [!div class="step-by-step"]
> [Önceki](controller-methods-views.md)
> [İleri](new-field.md)
