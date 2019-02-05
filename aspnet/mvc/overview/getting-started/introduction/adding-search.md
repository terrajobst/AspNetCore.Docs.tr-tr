---
uid: mvc/overview/getting-started/introduction/adding-search
title: Arama | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 31fd35ac63f3eb31d824e1710833ad83a0852ac9
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712269"
---
<a name="search"></a>Ara
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Bir arama yöntemi ve arama görünümü ekleme

Bu bölümde, arama özelliği ekleyeceksiniz `Index` olanak sağlayan bir eylem yöntemi filmler Tarz veya adı arayın.

## <a name="updating-the-index-form"></a>Dizin formun güncelleştiriliyor

Başlangıç güncelleştirerek `Index` mevcut eylem yöntemine `MoviesController` sınıfı. Kod aşağıdaki gibidir:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

İlk satırı `Index` yöntemi oluşturur aşağıdaki [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) filmler seçmek için sorgu:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Sorgu, bu noktada tanımlanır ancak veritabanında henüz çalıştırılmadı.

Varsa `searchString` parametresi içeren bir dize, filmler sorgu aşağıdaki kodu kullanarak, bir arama dizesi değerini temel filtrelemek için değiştirilir:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

`s => s.Title` Kodu yukarıdaki bir [Lambda ifadesi](https://msdn.microsoft.com/library/bb397687.aspx). Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) Yukarıdaki kod içinde kullanılan yöntem. LINQ sorguları tanımlandıkları veya bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where` veya `OrderBy`. Bunun yerine, sorgu yürütmesi, üzerinden gerçekleştirilen değeri gerçekten yinelendiğinde kadar bir ifade değerlendirmesi ertelendi anlamına gelir ertelenmiştir veya [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) yöntemi çağrılır. İçinde `Search` örnek, sorgu yürütülürse *Index.cshtml* görünümü. Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> [İçerir](https://msdn.microsoft.com/library/bb155125.aspx) yöntemi, veritabanı, c# kodunda değil yukarıdaki çalıştırılır. Veritabanında [içerir](https://msdn.microsoft.com/library/bb155125.aspx) eşlendiği [SQL gibi](https://msdn.microsoft.com/library/ms179859.aspx), büyük küçük harfe duyarlı olduğu.

Şimdi güncelleştirebilirsiniz `Index` kullanıcıya form görüntüleyecek görünümü.

Uygulamayı çalıştırmak ve gidin */filmler/dizin*. Bir sorgu dizesi gibi ekleme `?searchString=ghost` URL'si. Filtrelenmiş filmler görüntülenir.

![SearchQryStr](adding-search/_static/image1.png)

İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *uygulama\_Start\ RouteConfig.cs* dosya.

[!code-json[Main](adding-search/samples/sample4.json)]

Özgün `Index` yöntemi aşağıdaki gibi görünür:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Değiştirilmiş `Index` yöntemi şu şekilde görünür:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.

![](adding-search/_static/image2.png)

Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Artık, yardımcı olmak için kullanıcı Arabirimi ekleyeceksiniz filmler filtreleyebilirsiniz. İmzası değiştirdiyseniz `Index` metodu rota bağlı ID parametresine geçirilecek nasıl test etmek için değiştirme geri böylece, `Index` yöntemi adlı bir dize parametresi alır `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Açık *Views\Movies\Index.cshtml* dosyasını açın ve sonra yalnızca `@Html.ActionLink("Create New", "Create")`, aşağıda Vurgulanan form biçimlendirme ekleyin:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Yardımcısı oluşturur açılış `<form>` etiketi. `Html.BeginForm` Yardımcısı neden tıklayarak kullanıcı formu gönderdiğinde, kendisine göndermek form **filtre** düğmesi.

Visual Studio 2013, görüntüleme ve düzenleme görünümü dosyaları iyi bir gelişme vardır. Bir görünümü dosyasıyla uygulamayı açık çalıştırdığınızda Visual Studio 2013 görüntülemek için doğru denetleyici eylem yöntemi çağırır.

![](adding-search/_static/image3.png)

Dizin görünümünün (yukarıdaki görüntüde gösterildiği gibi), Visual Studio'da açın, uygulamayı çalıştırın ve ardından bir filmi için aramayı deneyin için CTRL-F5 veya F5'e dokunun.

![](adding-search/_static/image4.png)

Var olan hiçbir `HttpPost` aşırı yükünü `Index` yöntemi. Yöntemi uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez.

Aşağıdaki ekleyebilirsiniz `HttpPost Index` yöntemi. Bu durumda, eylem çağırıcısını BC `HttpPost Index` yöntemi ve `HttpPost Index` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Ancak, bu eklediğiniz bile `HttpPost` sürümünü `Index` yöntemi nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur. Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklayabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün. HTTP POST isteği URL'sini GET isteği (localhost:xxxxx filmler/dizin) URL'si ile aynıdır; URL içinde hiçbir arama bilgisi dikkat edin. Sağ artık, arama dizesi bilgilerini sunucuya biçiminde alan değeri gönderilir. Bu, yer işareti veya bir URL içinde arkadaş göndermek için bu arama bilgileri yakalayamazsınız anlamına gelir.

Çözüm bir aşırı yüklemesini kullanmaktır `BeginForm` POST isteği URL'sini arama bilgilerini eklemeniz gerekir ve bunun için yönlendirileceğini belirtir `HttpGet` sürümünü `Index` yöntemi. Parametresiz varolan `BeginForm` aşağıdaki işaretlemeyle yöntemi:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Artık bir arama gönderdiğinizde, URL'yi bir arama sorgu dizesi içerir. Arama de gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Türe göre arama ekleme

Eklediyseniz `HttpPost` sürümünü `Index` yöntemi, şimdi silin.

Ardından, kullanıcıların filmler için türe göre arama bir özellik ekleyeceksiniz. Değiştirin `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Bu sürümü `Index` yöntemi alır ek bir parametre, yani `movieGenre`. İlk birkaç kod satırlarının bir `List` film türleri veritabanından tutacak nesne.

Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Kod `AddRange` genel yöntemini `List` farklı türleri listesine eklemek için koleksiyonu. (Olmadan `Distinct` değiştiricisi, yinelenen tür eklenecek — Örneğin, iki kez örneğimizi Komedi eklenir). Kod içinde türleri listesini ardından depolar `ViewBag.MovieGenre` nesne. Kategori veri (böyle bir film türe ait) olarak depolama bir [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) nesnesine bir `ViewBag`, MVC uygulamaları için tipik bir yaklaşım ise bir açılan liste kutusunda kategori verilere erişme.

Aşağıdaki kod nasıl kontrol edileceğini göstermektedir `movieGenre` parametresi. Boş değilse, belirtilen türe seçili film sınırlamak için filmler sorgu kodu daha ayrıntılı kısıtlar.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Film listesi üzerinden yinelenir kadar daha önce bahsedildiği gibi sorgu veritabanında çalıştırılmaz (hangi olur Görünümü'nde sonra `Index` eylem yöntemine döndürür).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Biçimlendirme türe göre ara desteklemek için dizini görünümü ekleme

Ekleme bir `Html.DropDownList` yardımcıya *Views\Movies\Index.cshtml* hemen önce dosya `TextBox` Yardımcısı. Tamamlanan biçimlendirme aşağıda gösterilmiştir:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Aşağıdaki kodda:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

"MovieGenre" parametresi için anahtar sağlar `DropDownList` bulmaya yardımcı bir `IEnumerable<SelectListItem>` içinde `ViewBag`. `ViewBag` Eylem yönteminde doldurulup doldurulmadığına:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Parametre bir seçenek etiketini "Tüm" sağlar. Bu seçenek, tarayıcınızda inceleyin, kendi "value" özniteliği boş olduğunu görürsünüz. Denetleyici yalnızca filtreler bu yana `if` dize değil `null` veya göndermek için boş değer boş, `movieGenre` tüm türleri gösterir.

Varsayılan olarak seçilecek bir seçenek de ayarlayabilirsiniz. Varsayılan seçenek olarak "Komedi" istediyseniz, denetleyici kodda değiştirirsiniz şu şekilde:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Uygulamayı çalıştırmak ve göz atın */filmler/dizin*. Türe, film adı ve her iki ölçütlere göre bir arama deneyin.

![](adding-search/_static/image8.png)

Bu bölümde bir arama eylem yöntemi ve kullanıcıların filmi Tarz arama görünüm oluşturuldu. Sonraki bölümde, bir özelliğin ekleneceği nasıl şu konuları inceleyeceğiz `Movie` modeli ve bir test veritabanı otomatik olarak oluşturacak bir başlatıcı ekleme.

> [!div class="step-by-step"]
> [Önceki](examining-the-edit-methods-and-edit-view.md)
> [İleri](adding-a-new-field.md)
