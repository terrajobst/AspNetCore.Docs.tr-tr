---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sıralama, filtreleme ve (3 10) ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c2b53c5f35b0d7cd519ab3fdcb57c39463c40a8a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401230"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sıralama, filtreleme ve (3 10) ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide, bir dizi web sayfaları için temel CRUD işlemleri için uygulanan `Student` varlıklar. Bu öğreticide, sıralama, filtreleme ve sayfalama işlevselliğinin ekleyeceksiniz **Öğrenciler** dizin sayfası. Basit gruplandırma yapan bir sayfa da oluşturacaksınız.

Aşağıdaki çizim, hazır olduğunuzda sayfanın nasıl görüneceğini gösterir. Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıklayabileceği bağlantılar verilmiştir. Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme

Öğrenci dizin sayfasına sıralama eklemek için değiştireceksiniz `Index` yöntemi `Student` denetleyicisi ve kodu ekleyin `Student` dizin görünümü.

### <a name="add-sorting-functionality-to-the-index-method"></a>Index yöntemi işlevsellik sıralama Ekle

İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Bu kod alır bir `sortOrder` URL'ye sorgu dizesi parametresi. Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET MVC tarafından sağlanır. Parametre "Name" veya "Tarih", ardından isteğe bağlı olarak bir alt çizgi ve azalan düzende belirtmek için "desc" dizesi bir dize olur. Varsayılan sıralama artan düzendedir.

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

Yöntemini kullanan [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) göre sıralamak için sütun belirtmek için. Kod oluşturur bir [Iqueryable](https://msdn.microsoft.com/library/bb351562.aspx) önce değişken `switch` ifadesi, değiştiren içinde `switch` deyimi ve çağrılarını `ToList` sonrasına `switch` deyimi. Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir. Sorgu, dönüştürmek kadar yürütülmez `IQueryable` nesne gibi bir yöntem çağırarak koleksiyonuna `ToList`. Bu nedenle, bu kod, kadar yürütülmez tek bir sorgu sonuçları `return View` deyimi.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Sütun başlığı köprüler Öğrenci dizini görünümü ekleme

İçinde *Views\Student\Index.cshtml*, değiştirin `<tr>` ve `<th>` vurgulanmış kodu başlık satırı için öğeleri:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Bilgiler, bu kodu kullanan `ViewBag` uygun sorgu köprülerle ayarlamak için özellikler dize değerleri.

Sayfayı çalıştırın ve tıklayın **Soyadı** ve **kayıt tarihi** sütun başlıkları, sıralama doğrulamak için çalışır.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Tıkladıktan sonra **Soyadı** başlığı Öğrenciler soyadına göre azalan düzende görüntülenir.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Bir arama kutusu Öğrenciler dizin sayfasına ekleme

Öğrenciler dizin sayfasına filtre eklemek için görünümü için metin kutusu ve bir Gönder düğmesi ekleyin ve karşılık gelen değişiklik `Index` yöntemi. Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin.

İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kodla (değişiklikleri vurgulanır):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Eklediğiniz bir `searchString` parametresi `Index` yöntemi. Ayrıca LINQ deyime eklediğiniz bir `where` clausethat yalnızca Öğrenciler, ad ve Soyadı arama dizesini içeren seçer. Dizin görünümüne ekleyeceksiniz bir metin kutusundan arama dizesi değeri alındı. Ekler deyimi [burada](https://msdn.microsoft.com/library/bb535040.aspx) yan tümcesi yalnızca aramak için bir değer ise yürütülür.

> [!NOTE]
> Çoğu durumda bir Entity Framework varlık kümesini veya bir bellek içi koleksiyonunda bir genişletme yöntemi olarak aynı yöntemi çağırabilirsiniz. Sonuçları normalde aynıdır, ancak bazı durumlarda farklı olabilir. Örneğin, .NET Framework uygulamasını `Contains` yöntem boş bir dizeyi geçirmek, ancak SQL Server Compact 4.0 için Entity Framework sağlayıcısı boş dizeler için sıfır satır döndürür. tüm satırları döndürür. Bu nedenle örnek kodu (yerleştirme `Where` deyimi içinde bir `if` deyimi) tüm SQL Server sürümleri için aynı sonuçları elde emin olur. Ayrıca, .NET Framework uygulamasını `Contains` yöntemi varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirir, ancak varsayılan olarak Entity Framework SQL Server sağlayıcıları gerçekleştirmek büyük küçük harf duyarsız karşılaştırmalar. Bu nedenle, çağırma `ToUpper` test açıkça duyarlı hale getirmek için yöntem sağlar döndüreceği bir depoyu daha sonra kullanmak için kodu değiştirdiğinizde sonuçları değiştirmeyin bir `IEnumerable` koleksiyonu yerine bir `IQueryable` nesne. (Çağırdığınızda `Contains` metodunda bir `IEnumerable` koleksiyonu, .NET Framework uygulaması alın; çağırdığınızda, üzerinde bir `IQueryable` nesne veritabanı sağlayıcısı uygulamasını edinin.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Bir arama kutusu Öğrenci dizini görünümü ekleme

İçinde *Views\Student\Index.cshtml*, açmadan önce hemen vurgulanmış kodu ekleyin `table` resim yazısı, bir metin kutusu oluşturmak için etiket ve **arama** düğmesi.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Çalıştırırsanız, bir arama dizesi girin ve tıklayın **arama** filtreleme çalıştığını doğrulayın.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Bu sayfaya yer işareti, yer işareti kullandığınızda, filtrelenmiş liste vermeyecektir, yani "bir" arama dizesi, URL içermiyor dikkat edin. Değiştireceksiniz **arama** sorgu dizeleri için filtre ölçütlerini öğreticinin ilerleyen bölümlerinde kullanmak için düğme.

## <a name="add-paging-to-the-students-index-page"></a>Disk belleği Öğrenciler dizin sayfasına ekleme

Disk belleği Öğrenciler dizin sayfasına eklemek için yükleyerek başlayacaksınız **PagedList.Mvc** NuGet paketi. Sonra ek değişiklik yapacaksınız `Index` yöntemi ve disk belleği bağlantılar ekleme `Index` görünümü. **PagedList.Mvc** birçok iyi sayfalama ve paketler için ASP.NET MVC sıralamayı biridir ve kullanımını burada yalnızca diğer seçenekleri üzerinde onun için bir öneri olarak değil, örnek olarak tasarlanmıştır. Aşağıdaki çizimde, disk belleği bağlantılarını gösterir.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet paketini yükle

NuGet **PagedList.Mvc** paket otomatik olarak yükler **PagedList** paketi bir bağımlılık olarak. **PagedList** paketini yükler bir `PagedList` için koleksiyon türü ve uzantısı yöntemleri `IQueryable` ve `IEnumerable` koleksiyonları. Genişletme yöntemleri verilerin tek bir sayfa oluşturmak bir `PagedList` koleksiyon dışı, `IQueryable` veya `IEnumerable`ve `PagedList` koleksiyonu çeşitli özellikler ve disk belleği kolaylaştıran yöntemler sağlar. **PagedList.Mvc** paket sayfalama düğmeleri görüntüleyen bir disk belleği Yardımcısı yükler.

Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi** ardından **çözüm için NuGet paketlerini Yönet**.

İçinde **NuGet paketlerini Yönet** iletişim kutusu, tıklayın **çevrimiçi** soldaki sekmesinde ve arama kutusuna "disk belleğine alınan" girin. Gördüğünüzde **PagedList.Mvc** paketini, tıklayın **yükleme**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

İçinde **projeleri seçin** kutusunun **Tamam**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin.

İçinde *Controllers\StudentController.cs*, ekleme bir `using` bildirimi `PagedList` ad alanı:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Değiştirin `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Bu kod ekleyen bir `page` parametresi, geçerli bir sıralama sipariş parametresi ve yöntem imzası, burada gösterildiği gibi geçerli bir filtre parametresi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Kullanıcı bir disk belleği veya bağlantı sıralama taşınmadığından seçeneğine tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir. Disk belleği bağlantıya tıkladıysanız `page` değişkeni görüntülemek için sayfa numarasını içerir.

`A ViewBag` Bu disk belleği bağlantıları sıralama sırasında disk belleği aynı tutulabilmesi için eklenmesi gerekir çünkü geçerli sıralama düzenini görünümüyle özelliği sağlar:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Başka bir özellik `ViewBag.CurrentFilter`, geçerli bir filtre dizesi ile görünümü sağlar. Bu değer sırasında disk belleği filtre ayarlarını sürdürmek için disk belleği bağlantıları eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir. Arama dizesi sırasında disk belleği değiştirilirse, yeni filtre görüntülemek için farklı veri kaybına neden çünkü sayfa 1'e sıfırlanması gereken. Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir. Bu durumda, `searchString` parametresi null değil.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Yönteminin sonuna `ToPagedList` Öğrenciler genişletme yöntemini `IQueryable` nesne disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür. Öğrenciler, tek sayfalık sonra görünümüne geçirilir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Yöntemi, bir sayfa numarasını alır. İki soru işareti temsil [null birleşim işleci](https://msdn.microsoft.com/library/ms173224.aspx). Boş değer atanabilir bir tür için varsayılan bir değer null birleşim işleci tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` bir değere sahip veya 1 döndürür, `page` null.

### <a name="add-paging-links-to-the-student-index-view"></a>Öğrenci dizin görünümüne sayfalama bağlantılar ekleme

İçinde *Views\Student\Index.cshtml*, mevcut kodu şu kodla değiştirin:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

`@model` Sayfanın üstündeki deyimi belirtir görünüme artık alır bir `PagedList` yerine Nesne bir `List` nesne.

`using` Bildirimi `PagedList.Mvc` erişimi verir MVC yardımcıya için disk belleği düğmeleri.

Kod bir aşırı yüklemesini kullanır [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) belirtmek üzere sağlayan [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Varsayılan [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) parametreleri HTTP ileti gövdesini ve URL'yi içinde değil sorgu dizeleri geçirilir, yani bir GÖNDERİ ile form verileri gönderir. HTTP GET belirttiğinizde, form verilerini URL'ye sorgu dizeleri kullanıcıların yer işareti URL'si sağlayan geçirilir. [HTTP GET kullanımı için W3C yönergeleri](http://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirme olarak sonuçlanmaz olduğunda GET kullanması gerektiğini belirtin.

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

Sayfayı çalıştırın.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Disk belleği works emin olmak için farklı sıralamalar sayfalama bağlantıları tıklatın. Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği'ni deneyin.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Oluşturma bir öğrenci istatistiklerini gösteren bir sayfa hakkında

Contoso University sitesinin için sayfa hakkında kaç Öğrenciler her kayıt tarihi için kayıtlı olan görüntüleyeceksiniz. Bu gruplar üzerinde gruplandırma ve basit hesaplama gerektirir. Bunu yapmak için aşağıdakileri:

- Görünüme iletmek için gereken verileri için bir görünüm modeli sınıfı oluşturun.
- Değiştirme `About` yönteminde `Home` denetleyicisi.
- Değiştirme `About` görünümü.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturun

Oluşturma bir *Viewmodel'lar* klasör. Bu klasörde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Giriş denetleyicisini değiştirmek

İçinde *HomeController.cs*, aşağıdaki `using` deyimini dosyanın üst:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Hemen sınıfı için açılış kaşlı ayracından sonra veritabanı bağlamı için bir sınıf değişkeni ekleyin:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Değiştirin `About` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.

Ekleme bir `Dispose` yöntemi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Değiştirme görünümü hakkında

Değiştirin *Views\Home\About.cshtml* dosyasındaki kodu aşağıdaki kodla:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Uygulamayı çalıştırın ve tıklayın **hakkında** bağlantı. Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntülenir.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>İsteğe bağlı: uygulamayı Windows Azure'a dağıtma

Şu ana kadar uygulamanızı yerel olarak IIS Express'te URL'i geliştirme bilgisayarınızda çalışıyor. Internet üzerinden diğer kullanıcılar için kullanılabilir hale getirmek için bir web barındırma sağlayıcısına dağıtmak zorunda. Öğreticinin Bu isteğe bağlı bölümde bir Windows Azure Web sitesine dağıtacaksınız.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Veritabanını dağıtmak için Code First Migrations'ı kullanma

Veritabanını dağıtmak için Code First Migrations kullanacaksınız. Visual Studio'dan dağıtmak için ayarları yapılandırmak için kullandığınız yayımlama profili oluşturduğunuzda, etiketli onay kutusunu seçersiniz **yürütme Code First Migrations (uygulama başlatılırken çalışır)**. Bu ayar uygulamayı otomatik olarak yapılandırmak dağıtım işlemini neden *Web.config* Code First kullanmasını sağlayacak şekilde hedef sunucuda dosya `MigrateDatabaseToLatestVersion` Başlatıcı sınıfı.

Visual Studio dağıtım işlemi sırasında veritabanı ile herhangi bir şey yapmaz. Dağıtılan uygulamayı ilk kez dağıtımdan sonra veritabanına eriştiğinde, Code First otomatik olarak veritabanı oluşturur veya veritabanı şeması en son sürüme güncelleştirir. Uygulama bir geçişleri uyguluyorsa `Seed` yöntemi, veritabanı oluşturulur veya şema güncelleştirildikten sonra yöntemi çalışır.

Geçiş `Seed` yöntemi test verilerini ekler. Bir üretim ortamına dağıtma, değişikliği yapmanız gerekir `Seed` BT'nin yalnızca üretim veritabanınız eklenmesini istediğiniz verileri ekler için yöntemi. Örneğin, geçerli veri modelinizde gerçek kursları ancak kurgusal Öğrenciler geliştirme veritabanında sahip olmak isteyebilirsiniz. Yazabileceğiniz bir `Seed` hem de geliştirme yüklemek ve üretim ortamına dağıtmadan önce kurgusal Öğrenciler yorum yapmak için yöntem. Veya yazabileceğiniz bir `Seed` yalnızca kursları yüklenip kurgusal Öğrenciler uygulamanın kullanıcı arabirimini kullanarak test veritabanında el ile girin. yöntemi.

### <a name="get-a-windows-azure-account"></a>Bir Windows Azure hesabı edinin

Bir Windows Azure hesabınızın olması gerekir. Zaten yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Windows Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Windows Azure'da bir web sitesi ve SQL veritabanı oluşturma

Windows Azure Web sitenizi, diğer Windows Azure istemcilerle paylaşılan sanal makineler (VM) üzerinde çalıştığı anlamına gelir. paylaşılan bir barındırma ortamında çalıştırın. Paylaşılan bir barındırma ortamı, bulutta kullanmaya başlamak için bir düşük maliyetli yoludur. Daha sonra web trafiğiniz arttıkça, uygulama ayrılmış sanal makineler üzerinde çalıştırarak gereksinimini karşılayacak şekilde ölçeklendirilebilir. Daha karmaşık bir mimari gerekiyorsa, bir Windows Azure bulut hizmetine geçirebilirsiniz. Bulut Hizmetleri, sizin ihtiyaçlarınıza göre yapılandırabileceğiniz özel VM'ler üzerinde çalıştırın.

Windows Azure SQL veritabanı, SQL Server teknolojileri üzerine kurulmuş bir bulut tabanlı bir ilişkisel veritabanı hizmetidir. Araçları ve SQL Server ile çalışan uygulamalar SQL veritabanı ile de çalışır.

1. İçinde [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/), tıklayın **Web siteleri** sol sekmesini ve ardından **yeni**.

    ![Yönetim Portalı'nda yeni düğme](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Tıklayın **özel Oluştur**.

    ![Yönetim Portalı'nda veritabanı bağlantısı oluşturma](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **Yeni Web sitesi - özel Oluştur** Sihirbazı açılır.
3. İçinde **yeni Web sitesi** adım Sihirbazı'nın bir dize girin **URL** kutusunu uygulamanız için benzersiz bir URL olarak kullanın. Tam URL ne buraya girdiğiniz ve metin kutusunun yanındaki gördüğünüz bir son eke oluşur. "ConU" çizimde gösterilmektedir, ancak farklı bir parola seçmeniz gerekecektir. Bu nedenle söz konusu URL büyük olasılıkla alınır.

    ![Yönetim Portalı'nda veritabanı bağlantısı oluşturma](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. İçinde **bölge** aşağı açılan listesinde, size yakın bir bölge seçin. Bu ayar, web siteniz çalışır veri merkezini belirtir.
5. İçinde **veritabanı** aşağı açılan listesinde **ücretsiz 20 MB SQL veritabanı oluşturma**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. İçinde **DB bağlantı DİZESİ adı**, girin *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. İşaret kutusunun altındaki sağ oka tıklayın. Sihirbaz ilerler **veritabanı ayarlarını** adım.
8. İçinde **adı** kutusuna *ContosoUniversityDB*.
9. İçinde **sunucu** kutusunda **yeni SQL veritabanı sunucusu**. Alternatif olarak, daha önce bir sunucu oluşturduysanız, aşağı açılan listeden bu sunucuyu seçebilirsiniz.
10. Yönetici girin **oturum açma adı** ve **parola**. Seçtiyseniz **yeni SQL veritabanı sunucusu** mevcut bir ad ve burada parolayı girmezsiniz, yeni bir ad ve daha sonra veritabanına eriştiğinizde kullanmak üzere şimdi tanımlayacağınız parolayı girersiniz. Daha önce oluşturduğunuz bir sunucuyu seçtiyseniz, bu sunucu için kimlik bilgilerini girmenizi isteriz. Bu öğretici için seçtiğiniz olmaz ***Gelişmiş*** onay kutusu. ***Gelişmiş*** seçeneklerini veritabanı etkinleştirme [harmanlama](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Aynı seçin **bölge** web sitesi için seçtiğiniz.
12. Tamamlanmış göstermek için sağ kutusunun altındaki onay işaretine tıklayın.   
  
    ![Veritabanı ayarları adım, yeni Web sitesi - Veritabanı Sihirbazı](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Aşağıdaki resimde, bir var olan SQL Server ve oturum açma kullanma gösterilmektedir.   
  
    ![Veritabanı ayarları adım, yeni Web sitesi - Veritabanı Sihirbazı](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Yönetim Portalı Web siteleri sayfasına döndürür ve **durumu** sütun site oluşturulduğunu gösterir. (Genellikle bir dakikadan az), bir süre sonra **durumu** sütunda görüntülenir sitesi başarıyla oluşturuldu. Sol gezinti çubuğunda, hesabınızda kullandığınız sitelerin sayısı yanında görünür **Web siteleri** simgesi ve veritabanlarının sayısını yanında görünüyorsa **SQL veritabanları** simgesi.

## <a name="deploy-the-application-to-windows-azure"></a>Uygulamayı Windows azure'a dağıtma

1. Visual Studio'da projeye sağ **Çözüm Gezgini** seçip **Yayımla** bağlam menüsünden.  
  
    ![Proje bağlam menüsünde Yayımla](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. İçinde **profili** sekmesinde **Web'i Yayımla** Sihirbazı'nı tıklatın **alma**.  
  
    ![Yayımlama ayarlarını İçeri Aktar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Windows Azure aboneliğinizi Visual Studio'da daha önce eklemediyseniz, aşağıdaki adımları gerçekleştirin. Bu adımlarda açılır listesi altında olacak şekilde, abonelik Ekle **bir Windows Azure web sitesinden içeri aktarma** web sitenizi içerecektir.

    a. İçinde **yayımlama profilini içeri aktarma** iletişim kutusu, tıklayın **bir Windows Azure web sitesinden içeri aktarma**ve ardından **Ekle Windows Azure abonelik**.

    ![Windows Azure aboneliği ekleme](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. İçinde **alma Windows Azure abonelikleri** iletişim kutusu, tıklayın **indirme abonelik dosyası**.

    ![Abonelik dosyasını indir](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Tarayıcı pencerenizde Kaydet *.publishsettings* dosya.

    ![.publishsettings dosyasını indirin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Güvenlik - *publishsettings* dosyası, Windows Azure abonelik ve hizmetleri yönetmek için kullanılan (kodlanmamış), kimlik bilgilerini içerir. Dışında kaynak dizinleri geçici olarak depolamak için bu dosya için en iyi güvenlik uygulaması olan (örneğin *Libraries\Documents* klasör) ve içeri aktarma tamamlandıktan sonra silebilirsiniz. Erişim kazanır kötü niyetli bir kullanıcının `.publishsettings` dosya düzenleme, oluşturabilir ve Windows Azure hizmetlerinizi silin.

    d. İçinde **alma Windows Azure abonelikleri** iletişim kutusu, tıklayın **Gözat** gidin *.publishsettings* dosya.

    ![Sub indirin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. **İçeri aktar**'a tıklayın.

    ![içeri aktar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. İçinde **yayımlama profilini içeri aktarma** iletişim kutusunda **bir Windows Azure web sitesinden içeri aktarma**aşağı açılan listeden web sitenizi seçin ve ardından **Tamam**.  
  
    ![Yayımlama profili içeri aktar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. İçinde **bağlantı** sekmesinde **bağlantıyı doğrula** ayarlarının doğru olduğundan emin olmak için.  
  
    ![Bağlantısını doğrulama](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Bağlantı doğrulandı, yeşil bir onay işareti yanında gösterilen **bağlantıyı doğrula** düğmesi. **İleri**'ye tıklayın.  
  
    ![Bağlantı başarıyla doğrulandı](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Açık **uzak bağlantı dizesi** altındaki aşağı açılan listeden **SchoolContext** ve oluşturduğunuz veritabanı bağlantı dizesini seçin.
8. Seçin **yürütme Code First Migrations (uygulama başlatılırken çalışır)**.
9. Onay kutusunu temizleyin **çalışma zamanında Bu bağlantı dizesini kullan** için **UserContext (DefaultConnection)**, bu uygulama, üyelik veritabanının kullanmadığından.   
  
    ![Ayarlar sekmesi](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. **İleri**'ye tıklayın.
11. İçinde **Önizleme** sekmesinde **önizlemeyi Başlat**.  
  
    ![Önizleme sekmesini StartPreview düğmesi](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Sekme sunucuya kopyalanacak dosyaların bir listesini görüntüler. Önizleme görüntüleme uygulamayı yayımlamak için gerekli değildir, ancak dikkat edilmesi gereken yararlı bir işlevdir. Bu durumda, görüntülenen dosyaların listesini içeren herhangi bir şey yapmanız gerekmez. Bu uygulamayı dağıtmadan sonraki açışınızda değişmiş olan dosyaları bu listede yer alacaktır.  
  
    ![StartPreview dosya çıktısı](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Tıklayın **yayımlama**.  
    Visual Studio, dosyalar Windows Azure sunucusuna kopyalama işlemi başlar.
13. **Çıkış** penceresi hangi dağıtım eylemlerinin gerçekleştirildiğini gösterir ve dağıtımın başarılı olarak tamamlanmasına bildirir.  
  
    ![Çıkış penceresinde dağıtımın başarılı olduğunu bildiren](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Başarılı dağıtımdan sonra varsayılan tarayıcı otomatik olarak dağıtılan web sitesinin URL'sini açar.  
    Oluşturduğunuz uygulama artık bulutta çalışıyor. Öğrenciler sekmesine tıklayın.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

Bu noktada, *SchoolContext* veritabanı seçtiğiniz için Windows Azure SQL veritabanı'nda oluşturuldu **yürütme Code First Migrations (uygulama başlatılırken çalışır)**. *Web.config* web sitesi dağıtıldı dosyasında değiştirildi böylece [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Başlatıcı Kodunuzu okuyan veya veritabanına veri Yazar ilk kez çalıştırma (Bu Seçtiğiniz zaman oldu **Öğrenciler** sekmesi):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Dağıtım işlemi de oluşturulan yeni bir bağlantı dizesi *(SchoolContext\_DatabasePublish*) için Code First Migrations'veritabanı şeması güncelleştiriliyor ve veritabanında dengeli dağıtım için kullanılacak.

![Database_Publish bağlantı dizesi](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection* (Bu öğreticide kullanmıyorsanız) bir üyelik veritabanı bağlantı dizesi içindir. *SchoolContext* bağlantı dizesi için ContosoUniversity veritabanıdır.

Web.config dosyasının içinde kendi bilgisayarınıza dağıtılmış sürümünde bulabilirsiniz *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dağıtılan erişim *Web.config* kendisini FTP kullanarak dosya. Yönergeler için [Visual Studio kullanarak ASP.NET Web Dağıtımı: kod güncelleştirmesi dağıtma](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). İle başlayan yönergeleri "bir FTP aracını kullanmak için üç şeyi gerekir: FTP URL'si, kullanıcı adı ve parola."

> [!NOTE]
> URL bulur herkes veri değiştirebilmeniz için web uygulaması güvenlik uygulamaz. Web sitesini güvenli hale getirmek yönergeler için bkz: [üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulaması bir Windows Azure Web sitesine dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Windows Azure Yönetim Portalı'nı kullanarak site kullanarak diğer kişilerin engelleyebilir veya **Sunucu Gezgini** siteyi durdurmak için Visual Studio'da.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Kod ilk başlatıcıları

Dağıtım bölümünde gördüğünüz [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) kullanılan başlatıcı. Kod ilk, dahil olmak üzere kullanabileceğiniz diğer başlatıcılar ayrıca sağlar [Createdatabaseıfnotexists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (varsayılan), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) ve [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Başlatıcı birim testleri için koşullar ayarlamak için yararlı olabilir. Ayrıca, kendi başlatıcılar yazabilirsiniz ve uygulama okur veya veritabanına yazar beklemek istemiyorsanız, bir başlatıcı açıkça çağırabilirsiniz. Bölüm 6 kitabın başlatıcılar kapsamlı bir açıklaması için bkz. [Entity Framework programlama: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman ve Rowan Miller.

## <a name="summary"></a>Özet

Bu öğreticide bir veri modeli oluşturma ve sıralama, filtreleme, sayfalama ve gruplandırma işlevi temel CRUD uygulama gördünüz. Sonraki öğreticide veri modelini genişleterek daha ileri seviyeli konulara arama başlarsınız.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [İleri](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
