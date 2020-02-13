---
title: ASP.NET Core MVC uygulamasına arama ekleme
author: rick-anderson
description: Temel bir ASP.NET Core MVC uygulamasına aramanın nasıl ekleneceğini gösterir
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 89f1fa84783430f160ca0b840bf7ae9699520cb7
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171632"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC uygulamasına arama ekleme

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, film *tarzında* veya *ada*göre arama yapmanızı sağlayan `Index` Action yöntemine arama özelliği eklersiniz.

*Controllers/MoviesController. cs* içinde bulunan `Index` yöntemi aşağıdaki kodla güncelleştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

`Index` Action yönteminin ilk satırı, filmleri seçmek için bir [LINQ](/dotnet/standard/using-linq) sorgusu oluşturur:

```csharp
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlanmış, veritabanında çalıştırılmadı.

`searchString` parametresi bir dize içeriyorsa, filmler sorgusu arama dizesinin değerine göre filtreleyecek şekilde değiştirilir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

Yukarıdaki `s => s.Title.Contains()` kodu bir [lambda ifadesidir](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambdalar, Yöntem tabanlı [LINQ](/dotnet/standard/using-linq) sorgularında, [Where](/dotnet/api/system.linq.enumerable.where) yöntemi veya `Contains` (Yukarıdaki kodda kullanılır) gibi standart sorgu işleci yöntemlerine bağımsız değişkenler olarak kullanılır. LINQ sorguları tanımlandıklarında veya `Where`, `Contains`veya `OrderBy`gibi bir yöntem çağırarak değiştirildiklerinde yürütülmez. Bunun yerine sorgu yürütmesi ertelenir.  Diğer bir deyişle, bir ifadenin değerlendirmesi, gerçekleştirilmiş değeri gerçekten yineleneceği veya `ToListAsync` yöntemi çağrıldığında geciktirilen anlamına gelir. Ertelenmiş sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Not: [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi yukarıda gösterilen c# kodunda değil, veritabanında çalıştırılır. Sorgudaki büyük/küçük harf duyarlılığı veritabanına ve harmanlamaya bağlıdır. SQL Server üzerinde [SQL gibi](/sql/t-sql/language-elements/like-transact-sql)eşlemeler [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) , büyük/küçük harfe duyarsız olur. SQLite ' da, varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.

`/Movies/Index` sayfasına gidin. URL 'ye `?searchString=Ghost` gibi bir sorgu dizesi ekleyin. Filtrelenmiş filmler görüntülenir.

![Dizin görünümü](~/tutorials/first-mvc-app/search/_static/ghost.png)

`Index` yönteminin imzasını `id`adlı bir parametreye sahip olacak şekilde değiştirirseniz `id` parametresi, *Startup.cs*içinde ayarlanan varsayılan yollar için isteğe bağlı `{id}` yer tutucusuyla eşleştirecektir.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

Parametresini `id` olarak değiştirin ve `searchString` değişikliğin tüm oluşumları `id`olarak değiştirin.

Önceki `Index` yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

`id` parametresine sahip güncelleştirilmiş `Index` yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_SearchID)]

Artık arama başlığını sorgu dizesi değeri yerine rota verileri (bir URL segmenti) olarak geçirebilirsiniz.

![URL 'ye hayalet sözcük eklenmiş olan dizin görünümü, Ghostbusters ve Ghostbusters ters ve 2 adet film listesi](~/tutorials/first-mvc-app/search/_static/g2.png)

Ancak, kullanıcıların bir filmi her arayışınızda URL 'YI değiştirmesini beklemeniz gerekmez. Böylece, filmlerin filtrelemesine yardımcı olmak için UI öğeleri ekleyeceğiz. Yol ile bağlantılı `ID` parametresinin nasıl geçirileceğini test etmek için `Index` yönteminin imzasını değiştirdiyseniz, `searchString`adlı bir parametre alması için onu geri değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

*Views/filmler/Index. cshtml* dosyasını açın ve aşağıda vurgulanan `<form>` biçimlendirmeyi ekleyin:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` etiketi, form [etiketi yardımcısını](xref:mvc/views/working-with-forms)kullanır, bu nedenle formu gönderdiğinizde, filtre dizesi, filmler denetleyicisinin `Index` eylemine gönderilir. Değişikliklerinizi kaydedin ve sonra filtreyi test edin.

![Başlık filtresi metin kutusuna hayalet sözcük türü ile dizin görünümü](~/tutorials/first-mvc-app/search/_static/filter.png)

`Index` yönteminin `[HttpPost]` aşırı yüklemesi beklenmeyebilir. Bunun için gerekli değildir, çünkü yöntem uygulamanın durumunu değiştirmediğinden verileri filtrelememeniz yeterlidir.

Aşağıdaki `[HttpPost] Index` yöntemini ekleyebilirsiniz.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` parametresi, `Index` yöntemi için aşırı yükleme oluşturmak için kullanılır. Öğreticide daha sonra konuşacağız.

Bu yöntemi eklerseniz, bu eylem `[HttpPost] Index` yöntemiyle eşleşir ve `[HttpPost] Index` yöntemi aşağıdaki görüntüde gösterildiği gibi çalışır.

![HttpPost dizininden uygulama yanıtı olan tarayıcı penceresi: hayalet üzerinde filtrele](~/tutorials/first-mvc-app/search/_static/fo.png)

Ancak, `Index` yönteminin bu `[HttpPost]` sürümünü eklemeseniz bile, tümünün nasıl uygulandığını gösteren bir sınırlama vardır. Belirli bir arama için yer işareti koymak istediğinizi veya aynı film filtrelenmiş listesini görmek için onlara tıklabilecekleri bir bağlantı göndermek istediğinizi düşünün. HTTP POST isteğinin URL 'SI GET isteğinin URL 'siyle (localhost: {PORT}/filmler/dizin) aynı olduğunu fark edin; URL 'de arama bilgisi yok. Arama dizesi bilgileri sunucuya [form alanı değeri](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)olarak gönderilir. Tarayıcı geliştirici araçları veya harika [Fiddler aracının](https://www.telerik.com/fiddler)olduğunu doğrulayabilirsiniz. Aşağıdaki görüntüde Chrome tarayıcı geliştirici araçları gösterilmektedir:

![Microsoft Edge 'de Geliştirici Araçları, bir searchString değeri hayalet olan bir istek gövdesini gösteren ağ sekmesi](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

Arama parametresini ve [XSRF](xref:security/anti-request-forgery) belirtecini istek gövdesinde görebilirsiniz. Bu şekilde, önceki öğreticide bahsedildiği gibi, [form etiketi Yardımcısı](xref:mvc/views/working-with-forms) , bir [XSRF](xref:security/anti-request-forgery) Anti-forgery belirteci oluşturur. Verileri değiştiriyoruz, bu nedenle denetleyiciyi denetleyici yönteminde doğrulamamız gerekmiyor.

Arama parametresi, URL değil, istek gövdesinde olduğundan, bu arama bilgilerini, yer işareti veya başkalarıyla paylaşmak için yakalayamazsınız. İsteğin, *Görünümler/filmler/Index. cshtml* dosyasında bulunan `HTTP GET` olması gerektiğini belirterek bunu düzeltemedi.

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGet.cshtml?highlight=12&range=1-23)]

Artık bir arama gönderdiğinizde, URL arama sorgu dizesini içerir. Arama, bir `HttpPost Index` yönteminiz olsa bile `HttpGet Index` Action yöntemine de gidecektir.

![URL 'de searchString = hayalet ve döndürülen Filmler, Ghostbusters ve Ghostbusters 'ler 2 olan tarayıcı penceresi, hayalet sözcüğünü içerir](~/tutorials/first-mvc-app/search/_static/search_get.png)

Aşağıdaki biçimlendirme `form` etiketine yapılan değişikliği gösterir:

```cshtml
<form asp-controller="Movies" asp-action="Index" method="get">
```

## <a name="add-search-by-genre"></a>Türe göre arama Ekle

Aşağıdaki `MovieGenreViewModel` sınıfını *modeller* klasörüne ekleyin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Film tarzı görünüm modeli şunları içerir:

* Bir film listesi.
* Tarzlar listesini içeren bir `SelectList`. Bu, kullanıcının listeden bir tarz seçmesine olanak sağlar.
* Seçili tarzı içeren `MovieGenre`.
* `SearchString`, kullanıcılar arama metin kutusuna girdiğiniz metni içerir.

`MoviesController.cs` `Index` yöntemini aşağıdaki kodla değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Aşağıdaki kod, veritabanından tüm tarzları alan `LINQ` bir sorgudur.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_LINQ)]

Tarzın `SelectList`, farklı tarzlar yansıtımdan oluşturulur (Select listenizin yinelenen tarzlarına sahip olmasını istemiyorum).

Kullanıcı öğeyi aradığında arama değeri arama kutusuna tutulur.

## <a name="add-search-by-genre-to-the-index-view"></a>Tarzı, dizin görünümüne göre ara ekleme

*Görünümlerde/filmlerde* bulunan güncelleştirme `Index.cshtml`/aşağıdaki gibi:

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,19,28,31,34,37,43)]

Aşağıdaki HTML Yardımcısı 'nda kullanılan lambda ifadesini inceleyin:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`

Yukarıdaki kodda, `DisplayNameFor` HTML Yardımcısı, görünen adı belirlemede lambda ifadesinde başvurulan `Title` özelliğini inceler. Lambda ifadesi değerlendirilmek yerine incelenebileceğinden, `model`, `model.Movies`veya `model.Movies[0]` `null` veya boş olduğunda bir erişim ihlali almazsınız. Lambda ifadesi değerlendirildiğinde (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

Türe göre, film başlığına göre ve her ikisine birden arayarak uygulamayı test edin:

![https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2 sonuçlarını gösteren tarayıcı penceresi](~/tutorials/first-mvc-app/search/_static/s2.png)

> [!div class="step-by-step"]
> [Önceki](controller-methods-views.md)
> [İleri](new-field.md)
