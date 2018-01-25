---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Sıralama, filtreleme ve (3 10) ASP.NET MVC uygulamasındaki Entity Framework disk belleği | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f9b68abeba19561a327bad5ee4be80d79af1a550
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sıralama, filtreleme ve (3 10) ASP.NET MVC uygulamasındaki Entity Framework disk belleği
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

Bir yöntem [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) göre sıralamak için sütun belirtmek için. Kod oluşturur bir [Iqueryable](https://msdn.microsoft.com/library/bb351562.aspx) önce değişken `switch` deyimi içinde değiştirir `switch` deyimi ve çağrıları `ToList` sonra yöntemi `switch` deyimi. Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir. Dönüştürülünceye kadar sorgu yürütülmedi `IQueryable` gibi bir yöntemini çağırarak bir koleksiyon nesnesine `ToList`. Bu nedenle, bu kod kadar yürütülmedi tek bir sorgu sonuçları `return View` deyimi.

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

Eklediğiniz bir `searchString` parametresi `Index` yöntemi. LINQ ifadesi de eklediğiniz bir `where` clausethat yalnızca, ad ve Soyadı arama dizesini içeren Öğrenciler seçer. Bir metin kutusundan dizin görünümüne ekleyeceksiniz arama dizesi değeri alındı. Ekler deyimi [burada](https://msdn.microsoft.com/library/bb535040.aspx) yan tümcesi yalnızca arama için bir değer ise gerçekleştirilir.

> [!NOTE]
> Çoğu durumda aynı yöntemi bir Entity Framework varlık kümesi veya bir bellek içi koleksiyonda bir genişletme yöntemi olarak çağırabilirsiniz. Sonuçları normalde aynıdır, ancak bazı durumlarda farklı olabilir. Örneğin, .NET Framework uygulamasını `Contains` yöntem boş bir dizeyi geçirmek, ancak SQL Server Compact 4.0 için Entity Framework sağlayıcısı boş dizeler için sıfır satır döndürür tüm satırları döndürür. Bu nedenle örnek kodda (koyma `Where` deyimi içinde bir `if` deyimi) SQL Server'ın tüm sürümleri için aynı sonucu elde emin olur. Ayrıca, .NET Framework uygulamasını `Contains` yöntemi, varsayılan olarak büyük küçük harfe duyarlı karşılaştırma gerçekleştirir, ancak Entity Framework SQL Server sağlayıcıları varsayılan olarak büyük küçük harf duyarlı karşılaştırmalar gerçekleştirin. Bu nedenle, çağırma `ToUpper` test açıkça büyük küçük harf duyarsız yapmak için yöntem sağlar, döndürülecek bir depo daha sonra kullanmak için kodu değiştirdiğinizde sonuçları değiştirmeyin bir `IEnumerable` koleksiyon yerine bir `IQueryable` nesnesi. (Çağırdığınızda `Contains` yöntemi bir `IEnumerable` koleksiyonu, .NET Framework uygulamasını alın; çağırdığınızda, üzerinde bir `IQueryable` nesnesi, veritabanı sağlayıcısı uygulaması alın.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Bir arama kutusu Öğrenci dizin görünümüne ekleyin

İçinde *Views\Student\Index.cshtml*, açmadan önce hemen vurgulanmış kodu ekleyin `table` resim yazısı, bir metin kutusu oluşturmak için etiket ve **arama** düğmesi.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Sayfayı çalıştırın, bir arama dizesi girin ve tıklayın **arama** filtreleme çalıştığını doğrulayın.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

URL içermiyor. Bu sayfaya yer işareti varsa, yer işareti kullandığınızda, filtrelenmiş liste vermeyecektir, yani "bir", arama dizesi dikkat edin. Değiştireceğiz **arama** sorgu dizeleri için filtre ölçütlerini daha sonra öğreticide kullanmak üzere düğmesi.

## <a name="add-paging-to-the-students-index-page"></a>Disk belleği Öğrenciler dizin sayfasına ekleme

Disk belleği Öğrenciler dizin sayfasına eklemek için yükleyerek başlayacaksınız **PagedList.Mvc** NuGet paketi. Ek değişiklikler hale getireceğiz sonra `Index` yöntemi ve disk belleği bağlantılar ekleyebilir `Index` görünümü. **PagedList.Mvc** pek çok iyi disk belleği ve ASP.NET MVC için paketler sıralama biridir ve kendi burada yalnızca bir örnek olarak, onun için bir öneri diğer seçenekleri üzerinden olarak değil kullanılmaya yöneliktir. Aşağıdaki çizimde, disk belleği bağlantılarını gösterir.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet paketini yükleyin

NuGet **PagedList.Mvc** paketi otomatik olarak yükler **PagedList** paketi bir bağımlılık olarak. **PagedList** paketini yükler bir `PagedList` için koleksiyon türü ve uzantı yöntemleri `IQueryable` ve `IEnumerable` koleksiyonları. Genişletme yöntemleri verilerde tek sayfalık oluşturma bir `PagedList` dışı koleksiyonu, `IQueryable` veya `IEnumerable`ve `PagedList` koleksiyon çeşitli özellikleri ve disk belleği kolaylaştırmak yöntemleri sağlar. **PagedList.Mvc** paketi, disk belleği düğmeleri görüntüler sayfalama bir yardımcı yükler.

Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi** ve ardından **çözüm için NuGet paketlerini Yönet**.

İçinde **NuGet paketlerini Yönet** iletişim kutusu, tıklatın **çevrimiçi** sol tarafta sekmesini ve ardından arama kutusuna "disk belleğine alınan" girin. Gördüğünüzde **PagedList.Mvc** paketini, tıklatın **yükleme**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

İçinde **projeleri seçin** kutusunda, **Tamam**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin

İçinde *Controllers\StudentController.cs*, ekleme bir `using` bildirimi `PagedList` ad alanı:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Değiştir `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Bu kod ekleyen bir `page` parametre, geçerli bir sıralama sipariş parametresi ve aşağıda gösterildiği gibi yöntemi imzası geçerli bir filtre parametresi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Kullanıcı bir disk belleği veya bağlantı sıralama kurmadı tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir. Disk belleği bağlantıya tıkladıysanız `page` değişkeni görüntülemek için sayfa numarasını içerir.

`A ViewBag`Bu disk belleği bağlantıları sıralama sırasında disk belleği aynı tutmak için dahil edilmesi için geçerli sıralama düzenini görünümüyle özelliği sağlar:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Başka bir özellik `ViewBag.CurrentFilter`, görünümü ile geçerli filtre dizesini sağlar. Bu değer, disk belleği bağlantıları sırasında disk belleği filtre ayarlarını korumak için eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir. Arama dizesi sırasında disk belleği değiştirdiyseniz, 1'e sıfırlamak için sayfanın yeni filtre görüntülemek için farklı veri kaybına neden çünkü gerekir. Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir. Bu durumda, `searchString` parametresi null değil.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Yöntemi, sonunda `ToPagedList` genişletme yöntemi Öğrenciler üzerinde `IQueryable` nesnesi Öğrenci sorgu disk belleği destekleyen bir koleksiyon türü öğrencinin tek sayfalık dönüştürür. Bu sayfada Öğrenciler daha sonra görünümüne geçirilir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Yöntemi bir sayfa numarasını alır. İki soru işaretleri temsil [null birleşim işlecinin](https://msdn.microsoft.com/library/ms173224.aspx). Null birleşim işleci, null atanabilir bir tür için varsayılan bir değer tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` değerine sahip veya 1 döndürür, `page` null.

### <a name="add-paging-links-to-the-student-index-view"></a>Disk belleği bağlantılar Öğrenci dizin görünümüne ekleyin

İçinde *Views\Student\Index.cshtml*, var olan kodu aşağıdaki kodla değiştirin:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

`@model` Sayfanın üst kısmındaki deyimi belirtir görünümü şimdi alır bir `PagedList` yerine Nesne bir `List` nesnesi.

`using` Bildirimi `PagedList.Mvc` erişimi verir MVC Yardımcısı için disk belleği düğmeler.

Kod bir aşırı yüklemesini kullanır [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) belirtmek için sağlayan [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Varsayılan [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) parametreleri HTTP ileti gövdesi yer alan ve URL sorgu dizeleri geçirilir, yani bir POST ile form verileri gönderir. HTTP GET belirttiğinizde, form verilerini URL'de sorgu dizeleri kullanıcıların URL yer işareti sağlayan geçirilir. [HTTP GET kullanımı için W3C yönergeleri](http://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirmede sonuçlanmaz zaman GET kullanması gerektiğini belirtin.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Disk belleği çalıştığından emin olmak için farklı sıralamalar disk belleği bağlantıları tıklatın. Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği deneyin.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Oluşturma bir öğrenci istatistiklerini gösterir sayfa hakkında

Contoso University Web sitesinin için sayfa hakkında kaç tane Öğrenciler her kayıt tarihi için kayıtlı olan görüntülersiniz. Bu grupları gruplandırma ve basit hesaplamaları gerektirir. Bunu gerçekleştirmek için şunları yapmanız:

- Görünüme iletmek için gereken verileri için bir görünüm model sınıfı oluşturun.
- Değiştirme `About` yönteminde `Home` denetleyicisi.
- Değiştirme `About` görünümü.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturma

Oluşturma bir *ViewModels* klasör. Bu klasörde sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve var olan kodu aşağıdaki kodla değiştirin:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>İsteğe bağlı: uygulama Windows Azure'a dağıtma

Şu ana kadar uygulamanızı yerel olarak IIS Express'te geliştirme bilgisayarınızda çalıştığı. Internet üzerinden kullanmak diğer kişiler için kullanılabilir hale getirmek için bir web barındırma sağlayıcısına dağıtmak zorunda. Öğreticinin isteğe bağlı bu bölümde bir Windows Azure Web sitesi dağıtacaksınız.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Veritabanını dağıtmak için Code First geçişleri kullanma

Veritabanını dağıtmak için Code First Migrations kullanacaksınız. Visual Studio'dan dağıtmak için ayarları yapılandırmak için kullandığınız yayımlama profili oluşturduğunuzda, etiketli onay kutusu seçersiniz **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**. Bu ayar uygulamayı otomatik olarak yapılandırmak dağıtım işlemi neden *Web.config* Code First kullanmayacağından hedef sunucuda dosya `MigrateDatabaseToLatestVersion` Başlatıcı sınıfı.

Visual Studio dağıtım işlemi sırasında veritabanı ile herhangi bir şey yapmaz. Dağıtılmış uygulamanın veritabanı dağıtımdan sonra ilk kez eriştiğinde Code First otomatik olarak bir veritabanı oluşturur veya veritabanı şeması en son sürüme güncelleştirir. Uygulama bir geçişler uyguluyorsa `Seed` yöntemi, veritabanı oluşturulur veya şema güncelleştirildikten sonra yöntemi çalışır.

Geçişler `Seed` yöntemi test verilerini ekler. Bir üretim ortamına dağıtılmadan, değiştirmeniz gerekecektir `Seed` şekilde yalnızca üretim veritabanınıza eklenmesini istediğiniz veri ekleyen şekilde yöntemi. Örneğin, geçerli veri modelinizi gerçek kurslar ancak kurgusal Öğrenciler geliştirme veritabanında olan isteyebilirsiniz. Yazma bir `Seed` hem de geliştirme yüklenemedi ve üretim için dağıtmadan önce kurgusal Öğrenciler açıklama yöntemi. Veya yazabilirsiniz bir `Seed` yalnızca kurslar yüklenip kurgusal Öğrenciler uygulamanın kullanıcı arabirimini kullanarak test veritabanında el ile girin. yöntemi.

### <a name="get-a-windows-azure-account"></a>Bir Windows Azure hesabı edinin

Windows Azure hesabınızın olması gerekir. Zaten yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Windows Azure ücretsiz deneme](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Windows Azure'da bir web sitesi ve bir SQL veritabanı oluşturma

Windows Azure Web sitenizi, diğer Windows Azure istemcilerle paylaşılan sanal makineler (VM'ler) üzerinde çalışan anlamına gelir paylaşılan bir barındırma ortamında çalıştırılır. Paylaşılan bir barındırma ortamı, bulutta çalışmaya başlamak için bir düşük maliyetli yoludur. Daha sonra web trafiği artarsa, uygulama ayrılmış VM'ler üzerinde çalıştırılan tarafından gereksinimini karşılayacak şekilde ölçeklendirebilirsiniz. Daha karmaşık bir mimari gerekiyorsa, bir Windows Azure bulut hizmetine geçirebilirsiniz. Bulut Hizmetleri, sizin ihtiyaçlarınıza göre yapılandırabilirsiniz özel VM'ler çalıştırın.

Windows Azure SQL veritabanı, SQL Server teknolojilerini yerleşik olarak bulunan bir bulut tabanlı ilişkisel veritabanı hizmetidir. Araçları ve SQL Server ile çalışan uygulamalar da SQL veritabanı ile çalışır.

1. İçinde [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/), tıklatın **Web siteleri** sol sekmesini ve ardından **yeni**.

    ![Yönetim Portalı'nda yeni düğmesi](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Tıklatın **özel Oluştur**.

    ![Yönetim Portalı'nda veritabanı bağlantısı oluşturun](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

 **Yeni Web sitesi - özel Oluştur** Sihirbazı açılır.
3. İçinde **yeni Web sitesi** adım Sihirbazı'nın bir dize girin **URL** kutusunu uygulamanız için benzersiz bir URL olarak kullanın. Tam URL ne buraya girdiğiniz ve metin kutusunun yanında göreceğiniz sonekine oluşur. "ConU" çizimde gösterilmektedir, ancak farklı bir seçmek olması için bu URL'yi büyük olasılıkla alınır.

    ![Yönetim Portalı'nda veritabanı bağlantısı oluşturun](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. İçinde **bölge** aşağı açılan listesinde, yakın bir bölge seçin. Bu ayar, web sitenizi çalışacak hangi veri merkezi belirtir.
5. İçinde **veritabanı** aşağı açılan listesinde, seçin **ücretsiz 20 MB SQL veritabanı oluşturma**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. İçinde **DB bağlantı DİZESİ adı**, girin *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Sağ taraftaki kutusunun alt kısmındaki işaret oka tıklayın. İçin sihirbaz ilerler **veritabanı ayarlarını** adım.
8. İçinde **adı** kutusuna *ContosoUniversityDB*.
9. İçinde **Server** kutusunda **yeni SQL veritabanı sunucusu**. Alternatif olarak, daha önce bir sunucu oluşturduysanız, bu sunucu aşağı açılan listeden seçebilirsiniz.
10. Bir yönetici girin **oturum açma adı** ve **parola**. Seçtiyseniz **yeni SQL veritabanı sunucusu** mevcut bir ad ve burada parola girmezsiniz, yeni bir ad ve daha sonra veritabanına eriştiğinizde kullanmak üzere şimdi tanımlayacağınız parolayı girersiniz. Daha önce oluşturduğunuz bir sunucuyu seçtiyseniz, bu sunucu için kimlik bilgilerini girin. Bu öğretici için seçtiğiniz olmaz ***Gelişmiş*** onay kutusu. ***Gelişmiş*** seçeneklerini veritabanını ayarlamak etkinleştirme [harmanlama](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Aynı seçin **bölge** web sitesi için seçtiğiniz.
12. Tamamlanmış belirtmek üzere kutusunun sağ altındaki onay işaretine tıklayın.   
  
    ![Veritabanı ayarlarını adım Veritabanı Sihirbazı ile yeni Web sitesi - oluşturun](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

 Aşağıdaki resimde bir var olan SQL Server ve oturum açma kullanarak gösterir.   
  
    ![Veritabanı ayarlarını adım Veritabanı Sihirbazı ile yeni Web sitesi - oluşturun](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
 Yönetim Portalı Web siteleri sayfasına döndürür ve **durum** sütunda görüntülenir site oluşturulmaktadır. (Genellikle bir dakikadan az), bir süre sonra **durum** sütunda görüntülenir sitesi başarıyla oluşturuldu. Sol gezinti çubuğunda hesabınızda kullandığınız sitelerin sayısı görüntülenir **Web siteleri** simgesini ve veritabanı sayısı görünür yanına **SQL veritabanları** simgesi.

## <a name="deploy-the-application-to-windows-azure"></a>Windows Azure için uygulama dağıtma

1. Visual Studio'da'nde projeye sağ **Çözüm Gezgini** seçip **Yayımla** ve bağlam menüsünden.  
  
    ![Proje bağlam menüsünde Yayımla](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. İçinde **profil** sekmesinde **Web'i Yayımla** Sihirbazı'nı tıklatın **alma**.  
  
    ![Yayımlama ayarlarını İçeri Aktar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Visual Studio'da daha önce Windows Azure aboneliğiniz eklemediyseniz, aşağıdaki adımları gerçekleştirin. Aşağı açılan listeden altında Bu adımlarda, aboneliğiniz ekleyebilirsiniz, böylece **bir Windows Azure web sitesinden içeri aktarma** web sitenizi dahil edilir.

    a. İçinde **yayımlama profilini içeri aktarma** iletişim kutusu, tıklatın **bir Windows Azure web sitesinden içeri aktarma**ve ardından **Ekle Windows Azure abonelik**.

    ![Windows Azure Abonelik Ekle](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. İçinde **alma Windows Azure abonelikleri** iletişim kutusu, tıklatın **indirme abonelik dosyası**.

    ![Abonelik dosyasını indirin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Tarayıcı pencerenizde Kaydet *.publishsettings* dosya.

    ![.publishsettings dosyasını indirin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Güvenlik - *publishsettings* Dosya Hizmetleri ve Windows Azure Aboneliklerini yönetmek için kullanılan (kodlanmamış) kimlik bilgilerinizi içerir. Kaynak dizinlerinizi dışında geçici olarak depolamak için bu dosya için en iyi güvenlik uygulaması olduğundan (örneğin *Libraries\Documents* klasör) ve alma işlemi tamamlandıktan sonra silin. Erişim kazanır kötü niyetli bir kullanıcının `.publishsettings` dosyayı düzenleyebilir, oluşturmak ve Windows Azure hizmetlerinizi silebilirsiniz.

    d. İçinde **alma Windows Azure abonelikleri** iletişim kutusu, tıklatın **Gözat** gidin *.publishsettings* dosya.

    ![Sub indirin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. **İçeri aktar**'a tıklayın.

    ![içeri aktar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. İçinde **yayımlama profilini içeri aktarma** iletişim kutusunda **bir Windows Azure web sitesinden içeri aktarma**, aşağı açılan listeden, web sitenizi seçin ve ardından **Tamam**.  
  
    ![Yayımlama profilini İçeri Aktar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. İçinde **bağlantı** sekmesini tıklatın, **bağlantıyı doğrula** ayarlarının doğru olduğunu doğrulayın.  
  
    ![Bağlantı doğrula](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Bağlantı doğrulandığında yeşil bir onay işareti yanında gösterilen **bağlantıyı doğrula** düğmesi. **İleri**'ye tıklayın.  
  
    ![Başarıyla doğrulanan bağlantı](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Açık **uzak bağlantı dizesi** açılır listesi altında **SchoolContext** ve oluşturduğunuz veritabanı için bağlantı dizesi seçin.
8. Seçin **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**.
9. İşaretini **çalışma zamanında Bu bağlantı dizesini kullan** için **UserContext (DefaultConnection)**, bu uygulama, üyelik veritabanının kullanmadığından.   
  
    ![Ayarlar sekmesi](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. **İleri**'ye tıklayın.
11. İçinde **Önizleme** sekmesini tıklatın, **önizlemeyi Başlat**.  
  
    ![Önizleme sekmesinde StartPreview düğmesi](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
 Sekme sunucuya kopyalanacak dosyaların bir listesini görüntüler. Önizleme görüntüleme uygulamayı yayımlamak için gerekli değildir ancak farkında olması için kullanışlı bir işlevdir. Bu durumda, görüntülenen dosyaların listesi ile herhangi bir şey yapmanız gerekmez. Bu uygulamayı dağıtabilmek sonraki açışınızda değişen dosyaları bu listede yer alacaktır.  
  
    ![StartPreview dosya çıktısı](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Tıklatın **yayımlama**.  
 Visual Studio dosyaları Windows Azure sunucusuna kopyalama işlemi başlar.
13. **Çıkış** penceresinde hangi dağıtım eylemlerinin gerçekleştirilen gösterir ve dağıtımın başarılı şekilde tamamlandığını bildirir.  
  
    ![Çıktı penceresi başarılı dağıtım raporlama](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Başarılı dağıtım sırasında varsayılan tarayıcı otomatik olarak dağıtılan web sitesinin URL'sini açar.  
 Oluşturduğunuz uygulama bulutta şu anda çalışıyor. Öğrenciler sekmesini tıklatın.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

Bu noktada, *SchoolContext* veritabanı seçtiğiniz çünkü Windows Azure SQL veritabanı oluşturuldu **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**. *Web.config* web sitesi dağıtıldı dosyasında değiştirildi böylece [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Başlatıcı kodunuz okur veya veritabanına veri Yazar ilk kez çalıştırma (hangi Seçtiğiniz zaman oldu **Öğrenciler** sekmesi):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Dağıtım işlemi de oluşturulan yeni bir bağlantı dizesi *(SchoolContext\_DatabasePublish*) veritabanı şemasının güncelleştirme ve veritabanı dengeli dağıtımı için kullanılacak Code First geçişleri için.

![Database_Publish bağlantı dizesi](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection* (Bu öğreticide kullanmıyorsanız) üyelik veritabanı bağlantı dizesi içindir. *SchoolContext* bağlantı dizesi için ContosoUniversity veritabanıdır.

Web.config dosyasının kendi bilgisayara dağıtılmış sürümünde bulabilirsiniz *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dağıtılan erişim *Web.config* kendisini FTP kullanarak dosya. Yönergeler için bkz: [Visual Studio kullanarak ASP.NET Web Dağıtımı: bir kod güncelleştirme dağıtımı](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). İle başlayan yönergeleri izleyin "bir FTP aracı kullanmak için üç şey gerekir: FTP URL'si, kullanıcı adı ve parolanın."

> [!NOTE]
> URL bulur herkes veri değiştirebilmeniz için web uygulaması güvenlik uygulamaz. Web sitesini güvenli hale getirmek yönergeler için bkz: [üyelik, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC uygulaması Windows Azure Web sitesine dağıtmak](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Windows Azure Yönetim Portalı'nı kullanarak site kullanarak diğer kişilerin engelleyebilir veya **Sunucu Gezgini** siteyi durdurmak için Visual Studio'da.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Kod ilk başlatıcıları

Dağıtım bölümünde gördüğünüz [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) kullanılan Başlatıcısı. Kod ilk, dahil olmak üzere kullanabileceğiniz diğer başlatıcıları de sağlar [Createdatabaseıfnotexists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (varsayılan), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) ve [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Başlatıcı birim testleri için koşullar ayarlamak için yararlı olabilir. Ayrıca kendi başlatıcıları yazabilirsiniz ve uygulama okur veya veritabanına yazar beklemek istemiyorsanız, bir başlatıcı açıkça çağırabilir. Bölüm 6 defterinin başlatıcıları kapsamlı bir açıklama için bkz: [programlama Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman ve Rowan Mert.

## <a name="summary"></a>Özet

Bu öğreticide bir veri modeli oluşturmak ve temel CRUD, sıralama, filtreleme, disk belleği ve gruplandırma işlevi uygulamak öğrendiniz. Sonraki öğreticide veri modelini genişleterek daha gelişmiş konuları arayan başlarsınız.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Önceki](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[sonraki](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
