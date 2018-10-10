---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sıralama, filtreleme ve bir ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: riande
ms.date: 10/08/2018
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9fabb5a90af715d4e96ff79b43bfff5a4600ac08
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912780"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Sıralama, filtreleme ve bir ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama

tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Önceki öğreticide, bir dizi web sayfaları için temel CRUD işlemleri için uygulanan `Student` varlıklar. Bu öğreticide, sıralama, filtreleme ve sayfalama işlevselliğinin ekleyeceksiniz **Öğrenciler** dizin sayfası. Basit gruplandırma yapan bir sayfa da oluşturacaksınız.

Aşağıdaki çizim, hazır olduğunuzda sayfanın nasıl görüneceğini gösterir. Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıklayabileceği bağlantılar verilmiştir. Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme

Öğrenci dizin sayfasına sıralama eklemek için değiştireceksiniz `Index` yöntemi `Student` denetleyicisi ve kodu ekleyin `Student` dizin görünümü.

### <a name="add-sorting-functionality-to-the-index-method"></a>Sıralama işlevleri dizin yöntemine ekleyin.

- İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Bu kod alır bir `sortOrder` URL'ye sorgu dizesi parametresi. Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET MVC tarafından sağlanır. Ardından isteğe bağlı olarak bir alt çizgi ve azalan düzende belirtmek için "desc" dize "Name" veya "Tarih" olan dizesi parametresidir. Varsayılan sıralama artan düzendedir.

Dizin Sayfası istendi, ilk kez hiçbir sorgu dizesi yoktur. Öğrenciler tarafından artan düzende görüntülenen `LastName`, başarısızlık durumunda tarafından belirlenen varsayılan değerdir `switch` deyimi. Kullanıcı uygun sütun başlığına köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.

İki `ViewBag` görünüm sütun başlığı köprüler uygun sorgu dizesi değerleri yapılandırabilirsiniz. böylece değişkenler kullanılır:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Üçlü deyimleri şunlardır. Olmadığını birincinin belirtir `sortOrder` parametresi null veya boş `ViewBag.NameSortParm` ayarlanması gerekir "adı\_desc"; Aksi takdirde boş bir dize olarak ayarlanması gerekir. Bu iki deyimden sütun başlığı köprüler şu şekilde ayarlayın görünümün etkinleştir:

| Geçerli bir sıralama düzeni | Son adı köprü | Tarih köprü |
| --- | --- | --- |
| Adı artan en son | descending | ascending |
| Son azalan düzende ad | ascending | ascending |
| Artan tarihi | ascending | descending |
| Azalan düzende tarihi | ascending | ascending |

Yöntemini kullanan [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) göre sıralamak için sütun belirtmek için. Kod oluşturur bir <xref:System.Linq.IQueryable%601> önce değişken `switch` ifadesi, değiştiren içinde `switch` deyimi ve çağrılarını `ToList` sonrasına `switch` deyimi. Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir. Sorgu, dönüştürmek kadar yürütülmez `IQueryable` nesne gibi bir yöntem çağırarak koleksiyonuna `ToList`. Bu nedenle, bu kod, kadar yürütülmez tek bir sorgu sonuçları `return View` deyimi.

Her bir sıralama düzeni için farklı LINQ deyimleri yazılırken alternatif olarak, dinamik olarak bir LINQ ifadesini oluşturabilirsiniz. Dinamik LINQ hakkında daha fazla bilgi için bkz. [dinamik LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Öğrenci dizin görünüme sütun başlık köprüler ekleyin

1. İçinde *Views\Student\Index.cshtml*, değiştirin `<tr>` ve `<th>` vurgulanmış kodu başlık satırı için öğeleri:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Bilgiler, bu kodu kullanan `ViewBag` uygun sorgu köprülerle ayarlamak için özellikler dize değerleri.

2. Sayfayı çalıştırın ve tıklayın **Soyadı** ve **kayıt tarihi** sütun başlıkları, sıralama doğrulamak için çalışır.

   ![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

   Tıkladıktan sonra **Soyadı** başlığı Öğrenciler soyadına göre azalan düzende görüntülenir.

   ![Web tarayıcısında Öğrenci dizini görüntüle](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Bir arama kutusu Öğrenciler dizin sayfasına ekleme

Öğrenciler dizin sayfasına filtre eklemek için görünümü için metin kutusu ve bir Gönder düğmesi ekleyin ve karşılık gelen değişiklik `Index` yöntemi. Metin kutusu için ad ve soyadı alanları aramak için bir dize girmenize olanak tanır.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin.

- İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kodla (değişiklikleri vurgulanır):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Kod ekler bir `searchString` parametresi `Index` yöntemi. Dizin görünümüne ekleyeceksiniz bir metin kutusundan arama dizesi değeri alındı. Ayrıca ekler bir `where` yan tümcesine yalnızca Öğrenciler, ad ve Soyadı arama dizesini içeren seçer LINQ ifadesi. Ekler deyimi <xref:System.Linq.Queryable.Where%2A> yan tümcesi yalnızca aramak için bir değer ise yürütür.

> [!NOTE]
> Çoğu durumda bir Entity Framework varlık kümesini veya bir bellek içi koleksiyonunda bir genişletme yöntemi olarak aynı yöntemi çağırabilirsiniz. Sonuçları normalde aynıdır, ancak bazı durumlarda farklı olabilir.
>
> Örneğin, .NET Framework uygulamasını `Contains` yöntem boş bir dizeyi geçirmek, ancak SQL Server Compact 4.0 için Entity Framework sağlayıcısı boş dizeler için sıfır satır döndürür. tüm satırları döndürür. Bu nedenle örnek kodu (yerleştirme `Where` deyimi içinde bir `if` deyimi) tüm SQL Server sürümleri için aynı sonuçları elde emin olur. Ayrıca, .NET Framework uygulamasını `Contains` yöntemi varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirir, ancak varsayılan olarak Entity Framework SQL Server sağlayıcıları gerçekleştirmek büyük küçük harf duyarsız karşılaştırmalar. Bu nedenle, çağırma `ToUpper` test açıkça duyarlı hale getirmek için yöntem sağlar döndüreceği bir depoyu daha sonra kullanmak için kodu değiştirdiğinizde sonuçları değiştirmeyin bir `IEnumerable` koleksiyonu yerine bir `IQueryable` nesne. (Çağırdığınızda `Contains` metodunda bir `IEnumerable` koleksiyonu, .NET Framework uygulaması alın; çağırdığınızda, üzerinde bir `IQueryable` nesne veritabanı sağlayıcısı uygulamasını edinin.)
>
> Null işleme ayrıca farklı veritabanı sağlayıcıları veya kullandığınızda için farklı olabilir bir `IQueryable` nesne karşılaştırma için kullandığınızda bir `IEnumerable` koleksiyonu. Örneğin, bazı senaryolarda bir `Where` gibi koşul `table.Column != 0` sahip sütun döndürmeyebilir `null` değeri. Daha fazla bilgi için [hatalı 'where' yan tümcesinde null değişkenleri işlenmesi](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).

### <a name="add-a-search-box-to-the-student-index-view"></a>Bir arama kutusu Öğrenci dizini görünümü ekleme

1. İçinde *Views\Student\Index.cshtml*, açmadan önce hemen vurgulanmış kodu ekleyin `table` resim yazısı, bir metin kutusu oluşturmak için etiket ve **arama** düğmesi.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Çalıştırırsanız, bir arama dizesi girin ve tıklayın **arama** filtreleme çalıştığını doğrulayın.

   ![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

   Bu sayfaya yer işareti, yer işareti kullandığınızda, filtrelenmiş liste vermeyecektir, yani "bir" arama dizesi, URL içermiyor dikkat edin. Tam listeyi sıralamak şekilde bu sütun sıralama bağlantıları için de geçerlidir. Değiştireceksiniz **arama** sorgu dizeleri için filtre ölçütlerini öğreticinin ilerleyen bölümlerinde kullanmak için düğme.

## <a name="add-paging-to-the-students-index-page"></a>Disk belleği Öğrenciler dizin sayfasına ekleme

Disk belleği Öğrenciler dizin sayfasına eklemek için yükleyerek başlayacaksınız **PagedList.Mvc** NuGet paketi. Sonra ek değişiklik yapacaksınız `Index` yöntemi ve disk belleği bağlantılar ekleme `Index` görünümü. **PagedList.Mvc** birçok iyi sayfalama ve paketler için ASP.NET MVC sıralamayı biridir ve kullanımını burada yalnızca diğer seçenekleri üzerinde onun için bir öneri olarak değil, örnek olarak tasarlanmıştır. Aşağıdaki çizimde, disk belleği bağlantılarını gösterir.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet paketini yükle

NuGet **PagedList.Mvc** paket otomatik olarak yükler **PagedList** paketi bir bağımlılık olarak. **PagedList** paketini yükler bir `PagedList` için koleksiyon türü ve uzantısı yöntemleri `IQueryable` ve `IEnumerable` koleksiyonları. Genişletme yöntemleri verilerin tek bir sayfa oluşturmak bir `PagedList` koleksiyon dışı, `IQueryable` veya `IEnumerable`ve `PagedList` koleksiyonu çeşitli özellikler ve disk belleği kolaylaştıran yöntemler sağlar. **PagedList.Mvc** paket sayfalama düğmeleri görüntüleyen bir disk belleği Yardımcısı yükler.

1. Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.

2. İçinde **Paket Yöneticisi Konsolu** penceresinde emin **paket kaynağı** olduğu **nuget.org** ve **varsayılan proje** olan**ContosoUniversity**ve ardından aşağıdaki komutu girin:

   ```text
   Install-Package PagedList.Mvc
   ```

   ![PagedList.Mvc yükleyin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

3. Projeyi oluşturun.

### <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin.

1. İçinde *Controllers\StudentController.cs*, ekleme bir `using` bildirimi `PagedList` ad alanı:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Değiştirin `Index` yöntemini aşağıdaki kod ile:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Bu kod ekleyen bir `page` parametresi, geçerli bir sıralama sipariş parametresi ve yöntem imzası için geçerli bir filtre parametresi:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   Kullanıcı bir disk belleği veya bağlantı sıralama taşınmadığından seçeneğine tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir. Disk belleği bağlantıya tıkladıysanız `page` değişkeni görüntülemek için sayfa numarasını içerir.

   A `ViewBag` sıralama sırasında disk belleği aynı tutulabilmesi için disk belleği bağlantıları bu eklenmelidir çünkü geçerli sıralama düzenini görünümüyle özelliği sağlar:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Başka bir özellik `ViewBag.CurrentFilter`, geçerli bir filtre dizesi ile görünümü sağlar. Bu değer sırasında disk belleği filtre ayarlarını sürdürmek için disk belleği bağlantıları eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir. Arama dizesi sırasında disk belleği değiştirilirse, yeni filtre görüntülemek için farklı veri kaybına neden çünkü sayfa 1'e sıfırlanması gereken. Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir. Bu durumda, `searchString` parametresi null değil.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   Yönteminin sonuna `ToPagedList` Öğrenciler genişletme yöntemini `IQueryable` nesne disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür. Öğrenciler, tek sayfalık sonra görünümüne geçirilir:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   `ToPagedList` Yöntemi, bir sayfa numarasını alır. İki soru işareti temsil [null birleşim işleci](/dotnet/csharp/language-reference/operators/null-coalescing-operator). Boş değer atanabilir bir tür için varsayılan bir değer null birleşim işleci tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` bir değere sahip veya 1 döndürür, `page` null.

### <a name="add-paging-links-to-the-student-index-view"></a>Öğrenci dizin görünümüne sayfalama bağlantılar ekleme

1. İçinde *Views\Student\Index.cshtml*, mevcut kodu şu kodla değiştirin. Değişiklikler vurgulanır.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   `@model` Sayfanın üstündeki deyimi belirtir görünüme artık alır bir `PagedList` yerine Nesne bir `List` nesne.

   `using` Bildirimi `PagedList.Mvc` erişimi verir MVC yardımcıya için disk belleği düğmeleri.

   Kod bir aşırı yüklemesini kullanır [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) belirtmek üzere sağlayan [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Varsayılan [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) parametreleri HTTP ileti gövdesini ve URL'yi içinde değil sorgu dizeleri geçirilir, yani bir GÖNDERİ ile form verileri gönderir. HTTP GET belirttiğinizde, form verilerini URL'ye sorgu dizeleri kullanıcıların yer işareti URL'si sağlayan geçirilir. [HTTP GET kullanımı için W3C yönergeleri](http://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirme olarak sonuçlanmaz olduğunda GET kullanmanız önerilir.

   Yeni bir sayfa tıkladığınızda geçerli arama dizesinin görebilmeniz için metin kutusuna geçerli bir arama dizesi ile başlatılır.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   Sütun üst bilgisi bağlantıları, kullanıcının içinde filtre sonuçlarını sıralayabilirsiniz, böylece geçerli arama dizesinin denetleyiciye geçirilecek sorgu dizesi kullanın:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   Geçerli sayfayı ve toplam sayfa sayısı görüntülenir.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   Görüntülenecek sayfa varsa, "Sayfası 0 0" gösterilmektedir. (Bu durumda sayfa numarası sayfanın sayısından büyük olduğundan `Model.PageNumber` 1 ' dir ve `Model.PageCount` 0'dır.)

   Disk belleği düğme tarafından görüntülenen `PagedListPager` yardımcı:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   `PagedListPager` Yardımcısı, özelleştirebileceğiniz, stil ve URL'leri dahil olmak üzere birkaç seçenek sağlar. Daha fazla bilgi için [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub sitesinde.

2. Sayfayı çalıştırın.

   ![Disk belleği ile Öğrenciler dizin sayfası](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

   Disk belleği works emin olmak için farklı sıralamalar sayfalama bağlantıları tıklatın. Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği'ni deneyin.

   ![Öğrenciler, sayfanın arama filtre metni olan dizin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Öğrenci istatistiklerini gösteren bir hakkında sayfası oluşturma

Contoso University sitesinin için sayfa hakkında kaç Öğrenciler her kayıt tarihi için kayıtlı olan görüntüleyeceksiniz. Bu gruplar üzerinde gruplandırma ve basit hesaplama gerektirir. Bunu yapmak için aşağıdakileri:

- Görünüme iletmek için gereken verileri için bir görünüm modeli sınıfı oluşturun.
- Değiştirme `About` yönteminde `Home` denetleyicisi.
- Değiştirme `About` görünümü.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturun

Oluşturma bir *Viewmodel'lar* proje klasöründe. Bu klasörde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Giriş denetleyicisini değiştirmek

1. İçinde *HomeController.cs*, aşağıdaki `using` deyimini dosyanın üst:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Hemen sınıfı için açılış kaşlı ayracından sonra veritabanı bağlamı için bir sınıf değişkeni ekleyin:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Değiştirin `About` yöntemini aşağıdaki kod ile:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.

4. Ekleme bir `Dispose` yöntemi:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Değiştirme görünümü hakkında

1. Değiştirin *Views\Home\About.cshtml* dosyasındaki kodu aşağıdaki kodla:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Uygulamayı çalıştırın ve tıklayın **hakkında** bağlantı.

   Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntüler.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Özet

Bu öğreticide bir veri modeli oluşturma ve sıralama, filtreleme, sayfalama ve gruplandırma işlevi temel CRUD uygulama gördünüz. Sonraki öğreticide, veri modelini genişleterek daha ileri seviyeli konulara arama başlarsınız.

Lütfen bu öğreticide sevmediğinizi nasıl ve ne geliştirebileceğimiz hakkında geri bildirim bırakın.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [İleri](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
