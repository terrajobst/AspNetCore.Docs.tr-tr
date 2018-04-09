---
uid: mvc/overview/getting-started/introduction/adding-search
title: Arama | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 8afa72d4dbc4695e7d26c6ef4052be08a7c69080
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="search"></a>Ara
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Search yöntemini ve arama görünümü ekleme

Bu bölümde için arama yeteneğine ekleyeceksiniz `Index` olanak sağlayan eylem yöntemi arama filmler Tarz veya adı.

## <a name="updating-the-index-form"></a>Dizin formun güncelleştiriliyor

Başlangıç güncelleştirerek `Index` varolan eylem yönteminin `MoviesController` sınıfı. Kod aşağıdaki gibidir:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

İlk satırı `Index` yöntemi, aşağıdaki oluşturur [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) filmler seçmek için sorgu:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Sorgu, bu noktada tanımlandı ancak veritabanına karşı henüz çalıştırılmadı.

Varsa `searchString` parametre içeren bir dize, filmler sorgu aşağıdaki kodu kullanarak, arama dizesi değerini filtre şekilde değiştirilir:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

`s => s.Title` Kodu yukarıdaki bir [Lambda ifadesi](https://msdn.microsoft.com/library/bb397687.aspx). Lambda'lar yöntemi tabanlı içinde kullanılan [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) standart sorgu işleci yöntemlerinden bağımsız değişken olarak gibi sorgular [burada](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) Yukarıdaki kod içinde kullanılan yöntem. LINQ sorgularını değil tanımlanmış olan veya ne zaman bir yöntemi çağrılarak değiştirildiğinde yürütülen `Where` veya `OrderBy`. Bunun yerine, sorgu yürütme, gerçekleşen değeri gerçekte üzerinden yinelendiğinde kadar bir ifadenin değerlendirmesine Gecikmeli yani ertelenir veya [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) yöntemi çağrılır. İçinde `Search` örnek, sorgu yürütülürse *Index.cshtml* görünümü. Ertelenmiş sorgu yürütme hakkında daha fazla bilgi için bkz: [sorgu yürütme](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> [İçerir](https://msdn.microsoft.com/library/bb155125.aspx) yöntemi, veritabanı, c# kod değil yukarıdaki çalıştırılır. Veritabanı üzerindeki [içerir](https://msdn.microsoft.com/library/bb155125.aspx) eşlendiği [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), büyük küçük harfe duyarlı olduğu.

Güncelleştirebilirsiniz şimdi `Index` form kullanıcıya görüntüleyecek görünümü.

Uygulamayı çalıştırın ve gidin */filmler/dizin*. Bir sorgu dizesi gibi ekleme `?searchString=ghost` URL. Filtrelenmiş filmler görüntülenir.

![SearchQryStr](adding-search/_static/image1.png)

İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi eşleşir `{id}` varsayılan için yer tutucu yönlendirir kümesinde *uygulama\_Start\ RouteConfig.cs* dosya.

[!code-json[Main](adding-search/samples/sample4.json)]

Özgün `Index` yöntemi şöyle görünür:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Değiştirilen `Index` yöntemi uygulamamız görünecektir gibi:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine olarak arama başlık şimdi geçirebilirsiniz.

![](adding-search/_static/image2.png)

Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Size yardımcı olmak için kullanıcı Arabirimi ekleyeceksiniz artık bunu filmler filtre. İmzası değiştirdiyseniz `Index` rota bağlı ID parametresi geçirmek nasıl test etmek için yöntemini değiştirme geri böylece, `Index` yöntemi adlı bir dize parametresi alan `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Açık *Views\Movies\Index.cshtml* dosya ve henüz sonra `@Html.ActionLink("Create New", "Create")`, aşağıda vurgulanmış form biçimlendirmeyi ekleyin:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Yardımcı oluşturur açılış `<form>` etiketi. `Html.BeginForm` Yardımcı neden tıklayarak kullanıcı formu gönderdiğinde kendisine gönderme formun **filtre** düğmesi.

Visual Studio 2013 görüntülerken ve görünüm dosyalarını düzenleme iyi bir geliştirme vardır. Uygulama, bir görünüm dosyasıyla açık'e çalıştırdığınızda, Visual Studio 2013 görüntülemek için doğru denetleyici eylem yöntemini çağırır.

![](adding-search/_static/image3.png)

Dizin görünümünün (yukarıdaki resimde gösterildiği gibi) Visual Studio'da açıkken, uygulamayı çalıştırın ve ardından film aramayı deneyin için CTRL F5'e veya F5'e dokunun.

![](adding-search/_static/image4.png)

Var olan hiçbir `HttpPost` , aşırı `Index` yöntemi. Yöntemi uygulama durumunu değiştirme değil çünkü bu, yalnızca verileri filtreleme gerekmez.

Aşağıdaki ekleyebilirsiniz `HttpPost Index` yöntemi. Bu durumda, eylem Başlatıcısı eşleşir `HttpPost Index` yöntemi ve `HttpPost Index` yöntemi, aşağıdaki resimde gösterildiği gibi çalışıyor.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Ancak, bu bile eklerseniz `HttpPost` sürümü `Index` yöntemini nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur. Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklatabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün. HTTP POST isteği için URL GET isteğini (localhost:xxxxx/filmler/dizin) için URL ile aynıdır--URL arama bilgisi yok dikkat edin. Sağ şimdi, arama dizesi bilgileri sunucuya bir form alanı değerini gönderilir. Bu, yer işareti veya bir URL içinde arkadaş göndermek için arama bilgilerini yakalayamazsınız anlamına gelir.

Çözüm bir aşırı yüklemesini kullanmaktır `BeginForm` POST isteğini URL'ye arama bilgilerini eklemeniz gerekir ve bunun için yönlendirileceğini belirler `HttpGet` sürümü `Index` yöntemi. Parametresiz varolan `BeginForm` aşağıdaki biçimlendirme yöntemiyle:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Şimdi bir arama gönderdiğinizde, URL bir arama sorgu dizesi içeriyor. Arama da gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Türe göre arama ekleme

Eklediyseniz `HttpPost` sürümü `Index` yöntemi, şimdi silin.

Ardından, filmler için türe göre arama kullanıcıların izin vermek için bir özellik ekleyeceksiniz. Değiştir `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Bu sürümü `Index` yöntemi alır ek bir parametre öğesine `movieGenre`. İlk birkaç satır kod oluşturma bir `List` film türler veritabanından tutacak nesne.

Tüm türler veritabanından alır bir LINQ Sorgu kodudur.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Kod kullanan `AddRange` genel yöntemini `List` farklı türler listesine eklemek için koleksiyon. (Olmadan `Distinct` değiştiricisi, yinelenen türler eklenmesi — Örneğin, iki kez örneğimizde Komedi ekleneceği). Kod içinde türler listesi sonra depolar `ViewBag.MovieGenre` nesnesi. Kategori verileri (böyle bir filmi Tarz 's) olarak depolamak bir [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) nesnesinde bir `ViewBag`, MVC uygulamaları için tipik bir yaklaşım ise bir açılır liste kutusu kategori verilere erişme.

Aşağıdaki kod nasıl denetleneceğini gösterir `movieGenre` parametresi. Boş değilse, daha fazla kod belirtilen Tarz seçili filmlere sınırlamak için filmler sorgu kısıtlar.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Film listesi üzerinden yinelendiğinde kadar daha önce belirtildiği gibi sorgu veri temel çalıştırılmaz (hangi olur görünümünde sonra `Index` eylem yöntemine döndürür).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Türe göre arama destekleyecek şekilde dizin görünümünün biçimlendirme ekleme

Ekleme bir `Html.DropDownList` yardımcıya *Views\Movies\Index.cshtml* hemen önce dosya `TextBox` Yardımcısı. Tamamlanan biçimlendirme aşağıda gösterilmiştir:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Aşağıdaki kodda:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

"MovieGenre" parametresi için anahtar sağlar `DropDownList` bulmaya yardımcı bir `IEnumerable<SelectListItem>` içinde `ViewBag`. `ViewBag` Eylem yönteminde doldurulmuş:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Parametre "Tümü" bir seçenek etiketini sağlar. Bu seçenek, tarayıcınızda inceleyin, kendi "value" özniteliği boş olduğunu görürsünüz. Bizim denetleyicisi filtreler yalnızca bu yana `if` dizesi değil `null` ya da boş bir değer için gönderme boş `movieGenre` tüm türler gösterir.

Varsayılan olarak seçili bir seçenek de ayarlayabilirsiniz. Varsayılan seçenek olarak "Komedi" istediyseniz, denetleyici kodda değişeceğinden sözlüğüdür:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Uygulamayı çalıştırın ve Gözat */filmler/dizin*. Bir arama Tarz, film adı ve her iki ölçütlere göre deneyin.

![](adding-search/_static/image8.png)

Bu bölümde bir arama eylem yöntemi ve filmi ve türe göre arama, kullanıcıların izin görünüm oluşturuldu. Sonraki bölümde, yeni özellik eklemek nasıl göreceğiz `Movie` modeli ve otomatik olarak bir test veritabanı oluşturacak bir başlatıcı ekleme.

> [!div class="step-by-step"]
> [Önceki](examining-the-edit-methods-and-edit-view.md)
> [sonraki](adding-a-new-field.md)
