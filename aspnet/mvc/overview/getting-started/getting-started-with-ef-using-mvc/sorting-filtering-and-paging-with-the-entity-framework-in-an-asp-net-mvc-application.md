---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Sıralama, filtreleme ve ASP.NET MVC uygulamasındaki Entity Framework disk belleği | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8d11bf47f8c43040ef30d7132f0bb756748dbacd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Sıralama, filtreleme ve ASP.NET MVC uygulamasındaki Entity Framework disk belleği
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Web sayfaları için temel CRUD işlemleri için bir dizi uygulanan önceki öğreticide `Student` varlıklar. Bu öğreticide, sıralama, filtreleme ve disk belleği işlevsellik ekleyeceksiniz **Öğrenciler** dizin sayfası. Ayrıca, basit gruplandırma yapan bir sayfa oluşturacaksınız.

Aşağıdaki çizimde tamamladığınızda sayfa nasıl görüneceği gösterilmektedir. Sütun başlıkları kullanıcının sütuna göre sıralamak için tıklatabileceği bağlantı bulunmaktadır. Bir sütun başlığına sürekli olarak artan veya azalan sıralama düzeni arasında geçiş yapar.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme

Öğrenci dizin sayfasına sıralama eklemek için değiştireceğiz `Index` yöntemi `Student` denetleyicisi ve kodu ekleyin `Student` dizin görünümü.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dizin yöntemi işlevsellik sıralama Ekle

İçinde *Controllers\StudentController.cs*, yerine `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Bu kod alan bir `sortOrder` URL'deki sorgu dizesi parametresi. Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET MVC tarafından sağlanır. Parametre "Ad" veya "Tarih", ardından isteğe bağlı olarak bir alt çizgi ve azalan belirtmek için "desc" dizesi olan bir dize olur. Varsayılan sıralama düzeni artan.

Dizin sayfası istenen ilk kez hiçbir sorgu dizesi yok. Öğrenciler göre artan sırada görüntülenen `LastName`, başarısızlık durumunda tarafından belirlenen varsayılan değerdir `switch` deyimi. Kullanıcı uygun sütun başlığını köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.

İki `ViewBag` değişkenleri görünümü uygun sorgu dizesi değerlerini sütun başlığını köprüler yapılandırabilmeniz kullanılır:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Üçlü deyimleri bunlar. Birinci anlama gelir `sortOrder` parametresi null veya boş, `ViewBag.NameSortParm` ayarlanması gerekir "adı\_desc"; Aksi halde, boş bir dize olarak ayarlanması gerekir. Bu iki ifade sütun başlığını köprüler şu şekilde ayarlamak görünümü etkinleştirin:

| Geçerli bir sıralama düzeni | Son adı köprü | Tarih köprü |
| --- | --- | --- |
| Ad artan en son | descending | ascending |
| Ad azalan en son | ascending | ascending |
| Artan tarihi | ascending | descending |
| Azalan tarihi | ascending | ascending |

Bir yöntem [LINQ to Entities](https://msdn.microsoft.com/en-us/library/bb386964.aspx) göre sıralamak için sütun belirtmek için. Kod oluşturur bir [Iqueryable](https://msdn.microsoft.com/en-us/library/bb351562.aspx) önce değişken `switch` deyimi içinde değiştirir `switch` deyimi ve çağrıları `ToList` sonra yöntemi `switch` deyimi. Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir. Dönüştürülünceye kadar sorgu yürütülmedi `IQueryable` gibi bir yöntemini çağırarak bir koleksiyon nesnesine `ToList`. Bu nedenle, bu kod kadar yürütülmedi tek bir sorgu sonuçları `return View` deyimi.

Her sıralama düzeni için farklı LINQ deyimleri yazma alternatif olarak, bir LINQ ifadesi dinamik olarak oluşturabilirsiniz. Dinamik LINQ hakkında daha fazla bilgi için bkz: [dinamik LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Sütun başlığı Öğrenci dizini görüntülemek için köprü ekleme

İçinde *Views\Student\Index.cshtml*, yerine `<tr>` ve `<th>` vurgulanmış kodu başlık satırı için öğeleri:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Bu kod bilgileri kullanan `ViewBag` uygun sorguyla köprüler ayarlamak için özellikler dize değerleri.

Sayfayı çalıştırın ve tıklatın **Soyadı** ve **kayıt tarihi** Bu sıralama doğrulamak için sütun başlıkları çalışır.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Tıklattıktan sonra **Soyadı** başlığı Öğrenciler son adı azalan sırada görüntülenir.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Bir arama kutusu Öğrenciler dizin sayfasına ekleme

Öğrenciler dizin sayfasına filtre eklemek için metin kutusu ve bir gönderme düğmesi görünümüne ekleyin ve karşılık gelen değişiklik `Index` yöntemi. Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin

İçinde *Controllers\StudentController.cs*, yerine `Index` (değişiklikleri vurgulanır) aşağıdaki kod ile yöntemi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Eklediğiniz bir `searchString` parametresi `Index` yöntemi. Bir metin kutusundan dizin görünümüne ekleyeceksiniz arama dizesi değeri alındı. LINQ ifadesi de eklediğiniz bir `where` yalnızca, ad ve Soyadı arama dizesini içeren Öğrenciler seçer yan tümcesi. Ekler deyimi [burada](https://msdn.microsoft.com/en-us/library/bb535040.aspx) yan tümcesi yalnızca arama için bir değer ise gerçekleştirilir.

> [!NOTE]
> Çoğu durumda aynı yöntemi bir Entity Framework varlık kümesi veya bir bellek içi koleksiyonda bir genişletme yöntemi olarak çağırabilirsiniz. Sonuçları normalde aynıdır, ancak bazı durumlarda farklı olabilir.
> 
> Örneğin, .NET Framework uygulamasını `Contains` yöntem boş bir dizeyi geçirmek, ancak SQL Server Compact 4.0 için Entity Framework sağlayıcısı boş dizeler için sıfır satır döndürür tüm satırları döndürür. Bu nedenle örnek kodda (koyma `Where` deyimi içinde bir `if` deyimi) SQL Server'ın tüm sürümleri için aynı sonucu elde emin olur. Ayrıca, .NET Framework uygulamasını `Contains` yöntemi, varsayılan olarak büyük küçük harfe duyarlı karşılaştırma gerçekleştirir, ancak Entity Framework SQL Server sağlayıcıları varsayılan olarak büyük küçük harf duyarlı karşılaştırmalar gerçekleştirin. Bu nedenle, çağırma `ToUpper` test açıkça büyük küçük harf duyarsız yapmak için yöntem sağlar, döndürülecek bir depo daha sonra kullanmak için kodu değiştirdiğinizde sonuçları değiştirmeyin bir `IEnumerable` koleksiyon yerine bir `IQueryable` nesnesi. (Çağırdığınızda `Contains` yöntemi bir `IEnumerable` koleksiyonu, .NET Framework uygulamasını alın; çağırdığınızda, üzerinde bir `IQueryable` nesnesi, veritabanı sağlayıcısı uygulaması alın.)
> 
> Null işleme da farklı bir veritabanı sağlayıcılar veya kullandığınızda için farklı olabilir bir `IQueryable` kullandığınızda nesnesi ile karşılaştırıldığında bir `IEnumerable` koleksiyonu. Örneğin, bazı senaryolarda bir `Where` gibi koşul `table.Column != 0` sahip sütunlar döndürmeyebilir `null` değeri olarak. Daha fazla bilgi için bkz: ['where' yan tümcesinde null değişkenlerinin yanlış işleme](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Bir arama kutusu Öğrenci dizin görünümüne ekleyin

İçinde *Views\Student\Index.cshtml*, açmadan önce hemen vurgulanmış kodu ekleyin `table` resim yazısı, bir metin kutusu oluşturmak için etiket ve **arama** düğmesi.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Sayfayı çalıştırın, bir arama dizesi girin ve tıklayın **arama** filtreleme çalıştığını doğrulayın.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

URL içermiyor. Bu sayfaya yer işareti varsa, yer işareti kullandığınızda, filtrelenmiş liste vermeyecektir, yani "bir", arama dizesi dikkat edin. Tam liste sıralayacağını gibi bu sütun sıralama bağlantılar için de geçerlidir. Değiştireceğiz **arama** sorgu dizeleri için filtre ölçütlerini daha sonra öğreticide kullanmak üzere düğmesi.

## <a name="add-paging-to-the-students-index-page"></a>Disk belleği Öğrenciler dizin sayfasına ekleme

Disk belleği Öğrenciler dizin sayfasına eklemek için yükleyerek başlayacaksınız **PagedList.Mvc** NuGet paketi. Ek değişiklikler hale getireceğiz sonra `Index` yöntemi ve disk belleği bağlantılar ekleyebilir `Index` görünümü. **PagedList.Mvc** pek çok iyi disk belleği ve ASP.NET MVC için paketler sıralama biridir ve kendi burada yalnızca bir örnek olarak, onun için bir öneri diğer seçenekleri üzerinden olarak değil kullanılmaya yöneliktir. Aşağıdaki çizimde, disk belleği bağlantılarını gösterir.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet paketini yükleyin

NuGet **PagedList.Mvc** paketi otomatik olarak yükler **PagedList** paketi bir bağımlılık olarak. **PagedList** paketini yükler bir `PagedList` için koleksiyon türü ve uzantı yöntemleri `IQueryable` ve `IEnumerable` koleksiyonları. Genişletme yöntemleri verilerde tek sayfalık oluşturma bir `PagedList` dışı koleksiyonu, `IQueryable` veya `IEnumerable`ve `PagedList` koleksiyon çeşitli özellikleri ve disk belleği kolaylaştırmak yöntemleri sağlar. **PagedList.Mvc** paketi, disk belleği düğmeleri görüntüler sayfalama bir yardımcı yükler.

Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

İçinde **Paket Yöneticisi Konsolu** penceresinde emin olun **paket kaynağı** olan **nuget.org** ve **varsayılan proje** olan**ContosoUniversity**ve ardından aşağıdaki komutu girin:

`Install-Package PagedList.Mvc`

![PagedList.Mvc yükleyin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Projeyi oluşturun. 

### <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin

İçinde *Controllers\StudentController.cs*, ekleme bir `using` bildirimi `PagedList` ad alanı:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Değiştir `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Bu kod ekleyen bir `page` parametre, geçerli bir sıralama sipariş parametresi ve yöntem imzası geçerli filtre parametresi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Kullanıcı bir disk belleği veya bağlantı sıralama kurmadı tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir. Disk belleği bağlantıya tıkladıysanız `page` değişkeni görüntülemek için sayfa numarasını içerir.

A `ViewBag` özelliği, bu disk belleği bağlantıları sıralama sırasında disk belleği aynı tutmak için dahil edilmesi için geçerli sıralama düzenini görünümüyle sağlar:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Başka bir özellik `ViewBag.CurrentFilter`, görünümü ile geçerli filtre dizesini sağlar. Bu değer, disk belleği bağlantıları sırasında disk belleği filtre ayarlarını korumak için eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir. Arama dizesi sırasında disk belleği değiştirdiyseniz, 1'e sıfırlamak için sayfanın yeni filtre görüntülemek için farklı veri kaybına neden çünkü gerekir. Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir. Bu durumda, `searchString` parametresi null değil.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Yöntemi, sonunda `ToPagedList` genişletme yöntemi Öğrenciler üzerinde `IQueryable` nesnesi Öğrenci sorgu disk belleği destekleyen bir koleksiyon türü öğrencinin tek sayfalık dönüştürür. Bu sayfada Öğrenciler daha sonra görünümüne geçirilir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Yöntemi bir sayfa numarasını alır. İki soru işaretleri temsil [null birleşim işlecinin](https://msdn.microsoft.com/en-us/library/ms173224.aspx). Null birleşim işleci, null atanabilir bir tür için varsayılan bir değer tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` değerine sahip veya 1 döndürür, `page` null.

### <a name="add-paging-links-to-the-student-index-view"></a>Disk belleği bağlantılar Öğrenci dizin görünümüne ekleyin

İçinde *Views\Student\Index.cshtml*, var olan kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

`@model` Sayfanın üst kısmındaki deyimi belirtir görünümü şimdi alır bir `PagedList` yerine Nesne bir `List` nesnesi.

`using` Bildirimi `PagedList.Mvc` erişimi verir MVC Yardımcısı için disk belleği düğmeler.

Kod bir aşırı yüklemesini kullanır [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) belirtmek için sağlayan [FormMethod.Get](https://msdn.microsoft.com/en-us/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Varsayılan [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) parametreleri HTTP ileti gövdesi yer alan ve URL sorgu dizeleri geçirilir, yani bir POST ile form verileri gönderir. HTTP GET belirttiğinizde, form verilerini URL'de sorgu dizeleri kullanıcıların URL yer işareti sağlayan geçirilir. [HTTP GET kullanımı için W3C yönergeleri](http://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirmede sonuçlanmaz zaman GET kullanması gerektiğini öneririz.

Yeni bir sayfa tıklattığınızda geçerli arama dizesi görebilmeniz için metin kutusunda geçerli arama dizesiyle başlatıldı.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Sütun başlığı bağlantıları sorgu dizesi kullanıcı içinde filtre sonuçlarını sıralayabilmesi geçerli arama dizesi denetleyiciye geçirmek için kullanın:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Sayfa geçerli sayfa ve toplam sayısı görüntülenir.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Görüntülenecek sayfa varsa, "Sayfa 0 0" gösterilir. (Bu durumda sayfa numarası sayfa sayısından büyük olduğundan `Model.PageNumber` 1 ' dir ve `Model.PageCount` 0'dır.)

Disk belleği düğmeleri tarafından görüntülenen `PagedListPager` Yardımcısı:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Yardımcısı, çeşitli stil oluşturma ve URL'leri dahil özelleştirebilirsiniz seçenekler sağlar. Daha fazla bilgi için bkz: [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub sitesinde.

Sayfayı çalıştırın.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Disk belleği çalıştığından emin olmak için farklı sıralamalar disk belleği bağlantıları tıklatın. Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği deneyin.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Oluşturma bir öğrenci istatistiklerini gösterir sayfa hakkında

Contoso University Web sitesinin için sayfa hakkında kaç tane Öğrenciler her kayıt tarihi için kayıtlı olan görüntülersiniz. Bu grupları gruplandırma ve basit hesaplamaları gerektirir. Bunu gerçekleştirmek için şunları yapmanız:

- Görünüme iletmek için gereken verileri için bir görünüm model sınıfı oluşturun.
- Değiştirme `About` yönteminde `Home` denetleyicisi.
- Değiştirme `About` görünümü.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturma

Oluşturma bir *ViewModels* proje klasöründe. Bu klasörde sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Ev denetleyicisi değiştirme

İçinde *HomeController.cs*, aşağıdakileri ekleyin `using` dosyanın en üstüne deyimlerini:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Veritabanı bağlamı için bir sınıf değişkeni parantezinden sınıfı için hemen sonra ekleyin:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Değiştir `About` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ ifadesi Öğrenci varlıklar kayıt tarihe göre gruplar, her grup içindeki varlıkların sayısı hesaplar ve sonuçları bir koleksiyondaki depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.

Ekleme bir `Dispose` yöntemi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Değiştirme Görünüm hakkında

Kodla *Views\Home\About.cshtml* aşağıdaki kod ile dosya:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Uygulamayı çalıştırın ve tıklatın **hakkında** bağlantı. Öğrenciler için her kayıt tarihi sayısı bir tabloda görüntülenir.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Özet

Bu öğreticide bir veri modeli oluşturmak ve temel CRUD, sıralama, filtreleme, disk belleği ve gruplandırma işlevi uygulamak öğrendiniz. Sonraki öğreticide veri modelini genişleterek daha gelişmiş konuları arayan başlarsınız.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Önceki](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[sonraki](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
