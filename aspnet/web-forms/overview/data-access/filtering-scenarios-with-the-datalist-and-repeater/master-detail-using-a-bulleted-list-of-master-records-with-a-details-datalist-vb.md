---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Ana/ayrıntı ayrıntılar DataList (VB) ile ana kayıtları madde işaretli bir listesini kullanarak | Microsoft Docs
author: rick-anderson
description: Bu öğreticide şu iki sayfalık ana/ayrıntı raporu önceki öğreticinin tek bir sayfada, t madde işaretli kategori adları listesini gösteren Sıkıştır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d87dc7f4fb00e96d9eb2653e6fbc1efb8bb656c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889026"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Ana/ayrıntı madde işaretli ana kayıtların listesini ayrıntıları DataList (VB) ile kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) veya [PDF indirin](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> Bu öğreticide şu iki sayfalık ana/ayrıntı raporu önceki öğreticinin tek bir sayfada, ekran ve seçili kategorinin ürünleri ekranın sağ taraftaki sol tarafındaki madde işaretli kategori adları listesini gösteren sıkıştırın.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](master-detail-filtering-acess-two-pages-datalist-vb.md) ana/ayrıntı raporu iki sayfalara ayırmak nasıl inceledik. Ana sayfada madde işaretli kategorilerin listesini oluşturmak için bir yineleyici denetimi kullanılır. Köprü her kategori adı olduğu tıklatıldığında Al burada iki sütun DataList gösterdi bu ürünlerden Ayrıntıları sayfası, kullanıcıya ait, seçilen kategoriye.

Bu öğreticide şu iki sayfalık öğretici tek bir sayfada, LinkButton işlenen her kategori adı ile ekranın sol tarafındaki madde işaretli kategori adları listesini gösteren sıkıştırın. Kategori adı LinkButtons birini tıklatarak geri gönderimin uygulanmasını ve seçilen kategori s ürünleri iki sütun DataList ekranın sağ taraftaki bağlar. Her kategori s adı görüntüleme ek olarak, sol taraftaki yineleyici var. kaç toplam ürünleri için belirli bir kategori gösterilir (bkz: Şekil 1).


[![Kategori adı s ve ürünleri toplam sayı soldaki bölmede görüntülenir](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Şekil 1**: Kategori s adı ve ürünleri toplam sayı soldaki bölmede görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>1. adım: bir yineleyici ekranın sol bölümünde görüntüleme

Bu öğretici için size kategorileri Madde işaretli listede seçilen kategori s ürünleri solunda görünür olması gerekir. İçinde bir web sayfası içeriği konumlandırılmış standart HTML öğelerini paragraf etiketleri bölünemez boşluklar kullanarak `<table>` s ve benzeri veya geçişli stil sayfası (CSS) teknikleri aracılığıyla. Konumlandırma için öğreticilerimizi tümünün CSS teknikleri bugüne kadarki kullandınız. Ne zaman oluşturduğumuz Gezinti kullanıcı arabirimi ana sayfamızı içinde [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) kullandık öğretici *mutlak konumlandırma*, gezinti için kesin piksel uzaklık belirten Liste ve ana içerik. Alternatif olarak, CSS bir öğeye sağa veya sola başka bir konuma kullanılabilir *kayan*. Yineleyici DataList solundaki kayan tarafından seçilen kategori s ürünleri solunda görüntülenir kategorileri Madde işaretli listede sahip olabilir

Açık `CategoriesAndProducts.aspx` gelen sayfa `DataListRepeaterFiltering` klasör ve bir yineleyici ve DataList sayfasına ekleyin. Yineleyici s ayarlamak `ID` için `Categories` ve için s DataList `CategoryProducts`. Kaynak görünümüne gidin ve kendi içinde Yineleyici ve DataList denetimleri yerleştirme `<div>` öğeleri. Diğer bir deyişle, içinde yineleyici içine bir `<div>` öğesi ilk ve daha sonra kendi içinde DataList `<div>` yineleyici hemen sonra öğesi. Bu noktada, biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Yineleyici DataList solundaki float için kullanılacak ihtiyacımız `float` CSS stil özniteliği şu şekilde:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

`float: left;` İlk gezinen `<div>` ikincisi solundaki öğesi. `width` Ve `padding-right` ayarları belirtmek ilk `<div>` s `width` ve ne kadar doldurma arasında eklenir `<div>` öğe s içeriği ve sağ alt kenar boşluğu. CSS öğeleri kayan hakkında daha fazla bilgi için kullanıma [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

İlk aracılığıyla doğrudan stil ayarı belirtmek yerine `<p>` öğesi s `style` özniteliği, bunun yerine, yeni bir CSS sınıfı oluşturmak s izin `Styles.css` adlı `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Biz değiştirebilirsiniz sonra `<div>` ile `<div class="FloatLeft">`.

CSS sınıfı ekleme ve biçimlendirme yapılandırma sonra `CategoriesAndProducts.aspx` sayfasında, Designer'a gidin. (Sağ şimdi her ikisi de yalnızca görünse henüz kendi veri kaynakları veya şablonları yapılandırmak için ve biz bu yana kutuları gri olarak) DataList soluna kayan yineleyici görmeniz gerekir.


[![Yineleyici DataList soluna Yüzdürülen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Şekil 2**: yineleyici DataList soluna kaydırılmış ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>2. adım: her kategori için ürünleri sayısını belirleme

İşaretleme tam çevreleyen Yineleyici ve DataList s ile biz yineleyici kategori veri bağlamak için hazır re kontrol eder. Madde işaretli kategori listesi, Şekil 1'de gösterildiği gibi ancak, her kategori s adı yanı sıra biz de kategoriyle ilişkili ürünlerin sayısını görüntülemek gerekir. Bu bilgilere erişmek için şu şunlardan birini yapabilirsiniz:

- **ASP.NET sayfası s arka plandaki kod sınıfı bu bilgiyi belirler.** Belirli bir verilen *`categoryID`* çağırarak biz ilişkili ürünlerin sayısını belirleyebilirsiniz `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi. Bu yöntem bir `ProductsDataTable` nesnesindeki `Count` özelliği gösterir kaç `ProductsRow` s olduğunu, hangi ürünler için belirtilen sayısıdır *`categoryID`*. Oluşturabiliriz bir `ItemDataBound` , yineleyici için bağlı her kategori için çağıran yineleyici için olay işleyicisini `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ve çıktıda sayımına içerir.
- **Güncelleştirme `CategoriesDataTable` dahil etmek için yazılan kümesindeki bir `NumberOfProducts` sütun.** Biz sonra güncelleştirebilirsiniz `GetCategories()` yönteminde `CategoriesDataTable` bu bilgiyi içer veya alternatif olarak, bırakın için `GetCategories()` olarak-ise ve yeni bir `CategoriesDataTable` adlı bir yöntem `GetCategoriesAndNumberOfProducts()`.

Bu teknikler her ikisi de keşfedin s olanak tanır. İlk yaklaşım, biz t veri erişim katmanı güncelleştirme gerek güncelleştireceğinizi bu yana uygulama kolaydır; Ancak, veritabanı ile daha fazla iletişim gerektirir. Çağrı `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yönteminde `ItemDataBound` ek veritabanı çağrısı Yineleyicideki görüntülenen her kategori için olay işleyicisini ekler. Bu teknik ile vardır *N* + 1 veritabanı çağrısı, burada *N* Yineleyicideki görüntülenen kategorileri sayısıdır. İkinci yaklaşımda, ürün sayısı her kategoriden hakkındaki bilgilerle döndürülen `CategoriesBLL` s sınıfı `GetCategories()` (veya `GetCategoriesAndNumberOfProducts()`) içinde yalnızca bir seyahat veritabanına böylece kaynaklanan yöntemi.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>ItemDataBound olay işleyicisi ürünlerinde sayısını belirleme

Yineleyicideki s her kategori için ürünleri sayısını belirleme `ItemDataBound` olay işleyicisi bizim mevcut veri erişim katmanı için herhangi bir değişiklik gerektirmez. Tüm değişiklikleri doğrudan içinde yapılan `CategoriesAndProducts.aspx` sayfası. Adlı yeni bir ObjectDataSource eklemeye başlayın `CategoriesDataSource` yineleyici s akıllı etiket aracılığıyla. Ardından, yapılandırma `CategoriesDataSource` şekilde kendi veri alır şekilde ObjectDataSource `CategoriesBLL` s sınıfı `GetCategories()` yöntemi.


[![ObjectDataSource CategoriesBLL sınıf s GetCategories() yöntemi kullanacak şekilde yapılandırma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` s sınıfı `GetCategories()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Her öğe `Categories` yineleyici tıklanabilir ve tıklatıldığında neden için gereksinim duyduğu `CategoryProducts` DataList seçilen kategori için bu ürünleri görüntüler. Bu aynı bu sayfaya bağlama köprü, her kategori yaparak gerçekleştirilebilir (`CategoriesAndProducts.aspx`), ancak geçen `CategoryID` önceki öğreticide çok gördüğümüz gibi querystring aracılığıyla. Bu yaklaşımın avantajı, belirli kategori s ürünleri görüntüleyen bir sayfa kullanılabilir işareti ve bir arama motoru tarafından dizine emin olan.

Alternatif olarak, biz Bu öğreticide kullanacağız yaklaşım olduğu bir LinkButton her kategori yapabilirsiniz. LinkButton köprü olarak kullanıcı s tarayıcıda işler ancak tıklatıldığında geri gönderimin uygulanmasını; geri göndermede, DataList s ObjectDataSource seçili kategorisine ait bu ürünlerden görüntülenecek yenilenmesi gerekiyor. Bu öğretici için köprü kullanarak bir LinkButton kullanmaktan daha anlamlı olur; Ancak, bir LinkButton kullanarak daha avantajlı olduğu diğer senaryolar olabilir. Köprü yaklaşımı Bu örnek için ideal olsa da, bunun yerine LinkButton kullanarak araştırmaya s olanak tanır. Anlatıldığı gibi bir LinkButton kullanarak sahip bir köprüyü ortaya olmayan bazı zorluklar getirir. Bu nedenle, bu öğreticide bir LinkButton kullanarak bu zorluklar vurgulayın ve burada size bir LinkButton yerine bir köprü kullanmak isteyebilirsiniz bu senaryoları için çözümler sağlamaya yardımcı.

> [!NOTE]
> Köprü denetim kullanarak bu öğreticinin yineleyin önerilir veya `<a>` LinkButton yerine öğesi.


Aşağıdaki biçimlendirmede Yineleyici ve ObjectDataSource için bildirim temelli söz dizimini gösterir. Yineleyici s şablonlarını listeyi bir LinkButton olarak her bir öğesiyle işle dikkat edin:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Bu öğretici için etkin görünüm durumunu yineleyici olmalıdır (Java'daki Not `EnableViewState="False"` yineleyici s bildirim temelli sözdiziminden). 3. adımda size bir olay işleyicisi yineleyici s oluşturursunuz `ItemCommand` , biz güncelleştiriyor s ObjectDataSource s DataList olay `SelectParameters` koleksiyonu. Yineleyici s `ItemCommand`, ancak, Görünüm durumu devre dışı bırakılmışsa t yangın won. Bkz: [A zorlu bir ASP.NET soru](http://scottonwriting.net/sowblog/posts/1263.aspx) ve [çözümünün](http://scottonwriting.net/sowBlog/posts/1268.aspx) neden hakkında daha fazla bilgi için Görünüm durumunun yineleyici s için etkin `ItemCommand` tetiklenecek olay.


İle LinkButton `ID` özellik değerinin `ViewCategory` sahip değil, `Text` özellik kümesi. Biz yalnızca kategori adı görüntülenecek istediyseniz, metin özelliğini bildirimli olarak, veri bağlama söz dizimi ayarlarız sözlüğüdür:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Ancak, kategori s adlarını göstermek istiyoruz *ve* bu kategoriye ait ürünleri sayısı. Bu bilgiler yineleyici s alınabilir `ItemDataBound` yapılan bir çağrı yaparak olay işleyicisi `ProductBLL` s sınıfı `GetCategoriesByProductID(categoryID)` yöntemi ve kaç tane kayıtlar kaynaklanan döndürülür belirleme `ProductsDataTable`, aşağıdaki kod olarak gösterilmektedir:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Biz, sağlayarak başlar biz sahip veri öğesi çalışma re (bir olan `ItemType` olan `Item` veya `AlternatingItem`) ve ardından başvuru `CategoriesRow` geçerli bir yalnızca bağlı örneği `RepeaterItem`. Ardından, bu kategorideki ürünleri sayısı örneğini oluşturarak belirleriz `ProductsBLL` çağrılırken, sınıf kendi `GetCategoriesByProductID(categoryID)` yöntemi ve kullanarak döndürülen kayıt sayısını belirleme `Count` özelliği. Son olarak, `ViewCategory` ItemTemplate LinkButton olan başvuruları ve kendi `Text` özelliği ayarlanmış *CategoryName* (*NumberOfProductsInCategory*), burada  *NumberOfProductsInCategory* sıfır ondalık basamaklı bir sayı olarak biçimlendirilmiş.

> [!NOTE]
> Alternatif olarak, ekledik bir *işlevi biçimlendirme* kategori s kabul ASP.NET sayfası s arka plandaki kod sınıfı için `CategoryName` ve `CategoryID` değerleri ve döndürür `CategoryName` sayısı ile birleştirilmiş Kategori ürünlerinde (çağırarak belirlenen `GetCategoriesByProductID(categoryID)` yöntemi). Bu tür bir biçimlendirme işlevi sonuçlarını LinkButton s gereksinimini değiştirme metin özelliği bildirimli olarak atanabilir `ItemDataBound` olay işleyicisi. Başvurmak [kullanarak TemplateFields GridView denetiminde](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) veya [DataList ve bağlı yineleyici göre verileri biçimlendirme](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) öğreticileri biçimlendirme işlevlerini kullanma hakkında daha fazla bilgi için.


Bu olay işleyicisi eklendikten sonra bir tarayıcı aracılığıyla bir sayfayı test etmek için bir dakikanızı ayırın. Kategori s adı ve kategori ile ilişkili ürün sayısına görüntüleme madde işaretli listede, her kategoriye nasıl listelenen unutmayın (Şekil 4'e bakın).


[![Her kategori s adı ve numarası, ürünleri görüntülenir](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Şekil 4**: her kategori s adı ve numarası, ürünleri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Güncelleştirme`CategoriesDataTable`ve`CategoriesTableAdapter`her kategori için ürün sayısına eklemek için

S bağlı olarak her kategori için ürünleri sayısını belirleme yerine yineleyici için ayarlayarak Biz bu işlem kolaylaştırabilirsiniz `CategoriesDataTable` ve `CategoriesTableAdapter` bu bilgileri yerel olarak dahil etmek için veri erişim katmanı içinde. Bunu başarmak için size yeni bir sütun eklemelisiniz `CategoriesDataTable` ilişkili ürünlerin sayısını tutmak için. DataTable tablosuna yeni bir sütun eklemek için yazılan veri kümesi'ni açın (`App_Code\DAL\Northwind.xsd`) değiştirmek için DataTable sağ tıklayın ve Ekle'yi seçin / sütun. Yeni bir sütun ekleyin `CategoriesDataTable` (bkz. Şekil 5).


[![CategoriesDataSource için yeni bir sütun ekleyin](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Şekil 5**: yeni bir sütun ekleyin `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Bu adlı yeni bir sütun ekler `Column1`, farklı bir ad yazarak değiştirebilirsiniz. Bu yeni bir sütun için yeniden adlandırma `NumberOfProducts`. Ardından, biz bu sütun s özelliklerini yapılandırmanız gerekir. Yeni bir sütun üzerinde tıklatın ve Özellikler penceresine gidin. S değiştirmek `DataType` özelliğinden `System.String` için `System.Int32` ve `ReadOnly` özelliğine `True`Şekil 6'da gösterildiği gibi.


![Veri türü ve yeni bir sütun salt okunur özelliklerini ayarlama](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Şekil 6**: ayarlamak `DataType` ve `ReadOnly` yeni bir sütun özellikleri


Sırada `CategoriesDataTable` artık bir `NumberOfProducts` sütun değeri ayarlı değil herhangi bir karşılık gelen TableAdapter s sorgular tarafından. Biz güncelleştirebilirsiniz `GetCategories()` gibi bilgileri istiyoruz, bu bilgileri döndürmek için yöntemi döndürülen edildiğinde kategori bilgileri alınır. Ancak, yalnızca ender (örneğin olarak yalnızca Bu öğretici için) kategorileri için ilişkili ürün sayısına şablonlarınızdan ihtiyacımız durumunda biz bırakabilirsiniz, `GetCategories()` olarak-olduğu ve bu bilgileri döndürür yeni bir yöntem oluşturun. Let s adlı yeni bir yöntem oluşturma, ikincisi bu yaklaşımı kullanın `GetCategoriesAndNumberOfProducts()`.

Bu yeni eklemek için `GetCategoriesAndNumberOfProducts()` yöntemi, üzerinde sağ `CategoriesTableAdapter` ve yeni bir sorgu seçin. TableAdapter sorgu Yapılandırma Sihirbazı'nı hangi biz sayıda önceki eğitimlerine kullanılan ve yukarı getirir. Bu yöntem için sorgu satırlar döndüren bir geçici SQL deyimini kullanan belirterek Sihirbazı başlatın.


[![Geçici SQL deyimi kullanarak yöntemi oluşturma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Şekil 7**: yöntemini kullanarak bir geçici SQL deyimi oluşturmak ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![SQL deyimi satırları döndürür](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Şekil 8**: SQL deyimi döndürür satırları ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


Sonraki sihirbaz ekranında bize kullanmak bir sorgu için ister. Her kategori s döndürülecek `CategoryID`, `CategoryName`, ve `Description` alanları, kategori ile ilişkili ürünleri ile birlikte kullanmak aşağıdaki `SELECT` deyimi:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Kullanılacak sorgusu belirtin](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Şekil 9**: kullanılacak sorgusu belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Kategori ile ilişkili ürün sayısına hesaplar alt sorgu diğer adı olarak olduğuna dikkat edin `NumberOfProducts`. Bu adlandırma eşleşme ile ilişkili için bu alt sorgu tarafından döndürülen değer neden `CategoriesDataTable` s `NumberOfProducts` sütun.

Bu sorgu girdikten sonra son adımı yeni yöntemin adını seçmektir. Kullanım `FillWithNumberOfProducts` ve `GetCategoriesAndNumberOfProducts` dolgu DataTable ve Return DataTable, sırasıyla düzenleri.


[![Ad yeni TableAdapter s yöntemleri FillWithNumberOfProducts ve GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Şekil 10**: yeni TableAdapter s yöntemleri ad `FillWithNumberOfProducts` ve `GetCategoriesAndNumberOfProducts` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


Bu noktada veri erişim katmanı ürün kategorisi başına sayısına içerecek şekilde genişletilmiştir. Bizim sunu katmanı ayrı bir iş mantığı katmanı aracılığıyla DAL tüm çağrıları yönlendirir beri karşılık gelen eklemek ihtiyacımız `GetCategoriesAndNumberOfProducts` yönteme `CategoriesBLL` sınıfı:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

DAL ve BLL tam biz re hazır bu verileri bağlamak için `Categories` Yineleyicideki `CategoriesAndProducts.aspx`! Önceden belirleme gelen yineleyici için bir ObjectDataSource zaten oluşturduysanız, sayı, ürünlerinde `ItemDataBound` olay işleyicisi bölümünde, bu ObjectDataSource silme ve s yineleyici kaldırma `DataSourceID` özelliği ayarlama; ayrıca unwire Yineleyici s `ItemDataBound` kaldırarak olay işleyicisi olayından `Handles Categories.OnItemDataBound` ASP.NET arka plandaki kod sınıfı sözdiziminde.

Özgün durumuna geri içinde yineleyici ile adlı yeni bir ObjectDataSource ekleme `CategoriesDataSource` yineleyici s akıllı etiket aracılığıyla. ObjectDataSource kullanmak için yapılandırma `CategoriesBLL` sınıfı, ancak bunu kullanmak yerine `GetCategories()` yöntemi, sahip kullanmak `GetCategoriesAndNumberOfProducts()` yerine (bkz. Şekil 11).


[![ObjectDataSource GetCategoriesAndNumberOfProducts yöntemi kullanmak üzere yapılandırma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `GetCategoriesAndNumberOfProducts` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


Ardından, güncelleştirme `ItemTemplate` böylece LinkButton s `Text` özelliği databinding sözdizimini kullanarak bildirimli olarak atanır ve her ikisi de içeren `CategoryName` ve `NumberOfProducts` veri alanları. Yineleyici için tam bildirim temelli biçimlendirme ve `CategoriesDataSource` ObjectDataSource izleyin:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

Eklenecek DAL güncelleştirerek çizilir çıktı bir `NumberOfProducts` sütundur kullanarak aynı `ItemDataBound` olay işleyicisi yaklaşım (geri ekran görmek için Şekil 4'e bakın kategori adları ve ürünleri sayısını gösteren yineleyici görüntüsü).

## <a name="step-3-displaying-the-selected-category-s-products"></a>3. adım: seçilen kategori s ürünleri görüntüleme

Bu noktada sahibiz `Categories` yineleyici ürünleri ile birlikte kategori listesi, her kategoride görüntüleme. Yineleyici bir LinkButton tıklatıldığında nedenler kesiştiği geri gönderme bir işaret etmesini, biz her kategori için kullanır. Bu ürünler için seçilen kategorideki görüntülemeniz `CategoryProducts` DataList.

Bize bakan bir yalnızca seçilen kategori için ürünleri görüntüler DataList nasıl iştir. İçinde [ana/Ayrıntılar DetailsView ile seçilebilir ana GridView kullanarak ayrıntı](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) GridView, satırları nasıl oluşturulacağını gördüğümüz öğretici seçilmesi, seçilen satırı DetailsView aynı sayfada görüntülenen s ayrıntıları. GridView s kullanarak tüm ürünlerle ilgili bilgileri ObjectDataSource döndürülen `ProductsBLL` s `GetProducts()` yöntemi DetailsView s ObjectDataSource çalışırken alınan seçili ürün kullanma hakkında bilgi `GetProductsByProductID(productID)` yöntemi. *`productID`* Parametre değeri sağlanmadığından bildirimli olarak GridView s değeriyle ilişkilendirerek `SelectedValue` özelliği. Ne yazık ki, yineleyici sahip olmayan bir `SelectedValue` özelliği ve parametre kaynağı olarak sunulamıyor.

> [!NOTE]
> Bu, LinkButton bir yineleyici kullanılırken görünür bu zorluklar biridir. Köprü olarak geçirmek için kullandık vardı `CategoryID` querystring bunun yerine, biz QueryString alan kaynak olarak için parametre s değeri kullanabilirsiniz.


Biz eksikliği hakkında endişelenmeniz önce bir `SelectedValue` özelliği yineleyici için önce bir ObjectDataSource DataList bağlayın ve belirtin s yine de izin kendi `ItemTemplate`.

Adlı yeni bir ObjectDataSource eklemek için DataList s akıllı etiketten opt `CategoryProductsDataSource` ve kullanacak şekilde yapılandırın `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi. Bu öğreticide DataList salt okunur bir arabirim sunar olduğundan, INSERT, UPDATE, açılan listeleri ayarlamak ve sekmeleri (hiçbiri) silmek çekinmeyin.


[![ObjectDataSource ProductsBLL sınıfı s GetProductsByCategoryID(categoryID) yöntemi kullanmak üzere yapılandırma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Bu yana `GetProductsByCategoryID(categoryID)` yöntemi giriş parametresi bekliyor (*`categoryID`*), veri kaynağı Yapılandırma Sihirbazı'nı parametre s kaynak olanak tanır. Kategorileri listelenen GridView veya bir DataList, d parametre kaynak açılır liste denetimi ve ControlId ayarlarız `ID` Web denetimi veri. Ancak, yineleyici oturumda bu yana bir `SelectedValue` özelliği bir parametre kaynağı olarak kullanılamaz. İşaretlerseniz, ControlId aşağı açılan listesinde yalnızca bir denetim içerdiğini bulacaksınız `ID``CategoryProducts`, `ID` DataList.

Şimdilik, parametre kaynak aşağı açılan listesi yok olarak ayarlayın. Bu parametre değeri LinkButton Yineleyicideki tıklandığında kategori olduğunda program aracılığıyla atama yukarı elde edersiniz.


[![Adlı kullanıcı, Categoryıd'si parametresi için bir parametre kaynağı yapmak belirtme](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Şekil 13**: için bir parametre kaynak belirtmeyin *`categoryID`* parametre ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik-s DataList oluşturur `ItemTemplate`. Bu varsayılanı değiştirmek `ItemTemplate` şablonuyla biz önceki öğreticide kullanılan; ayrıca s DataList ayarlayın `RepeatColumns` özelliğini 2. Bu değişiklikleri yaptıktan sonra bildirim temelli biçimlendirme DataList ve onun ilişkili ObjectDataSource için aşağıdaki gibi görünmelidir:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Şu anda `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* parametresi hiçbir zaman ayarlanırsa, hiç ürün sayfasını görüntülerken görüntülenecek şekilde. Bu parametre değer temel alınarak ayarlanmış yapmak ihtiyacımız olan `CategoryID` yineleyicideki tıklatılan kategorisi. Bu iki zorluklar getirir: ilk olarak, nasıl belirleriz bir LinkButton zaman s yineleyicideki `ItemTemplate` tıklatılan; ve ikinci, nasıl biz belirleyebilir `CategoryID` , LinkButton tıklattınız karşılık gelen kategorisinin?

Düğmesini ve ImageButton denetimleri gibi LinkButton sahip bir `Click` olay ve [ `Command` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Olay yalnızca LinkButton tıklatıldıktan not etmek için tasarlanmıştır. Bazen, ancak LinkButton tıklatıldıktan belirtmeye ek olarak biz de bazı ek bilgi için olay işleyicisini geçirmek gerekir. Bu durumda, LinkButton s ise [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) ve [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) özellikler bu ek bilgiler atanabilir. Ardından LinkButton tıklandığında, kendi `Command` olay ateşlenir (yerine kendi `Click` olay) ve olay işleyicisi değerlerini geçirilen `CommandName` ve `CommandArgument` özellikleri.

Zaman bir `Command` olayı gelen yineleyici s yineleyici şablonunda içinde [ `ItemCommand` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) başlatılır ve geçirilen `CommandName` ve `CommandArgument` tıklatılan LinkButton değerlerini (veya düğmesini veya ImageButton). Bu nedenle, bir kategori yineleyicideki LinkButton tıklandığında belirlemek için aşağıdakileri yapmak ihtiyacımız var:

1. Ayarlama `CommandName` s yineleyicideki LinkButton özelliğinin `ItemTemplate` bazı değerine (t ve kullanılan ListProducts). Bu ayarlayarak `CommandName` değeri, LinkButton s `Command` LinkButton tıklatıldığında olay tetikler.
2. LinkButton s ayarlamak `CommandArgument` özelliğinin değeri geçerli öğenin s `CategoryID`.
3. Yineleyici s için bir olay işleyicisi oluşturun `ItemCommand` olay. Olay işleyicisi ayarlamak `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametre değerine geçilen `CommandArgument`.

Aşağıdaki `ItemTemplate` biçimlendirmesi kategorileri yineleyici için adım 1 ve 2 uygular. Not nasıl `CommandArgument` değeri s veri öğesi atanan `CategoryID` databinding sözdizimini kullanarak:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Oluşturma her bir `ItemCommand` gelen her zaman ilk denetlenecek akıllıca olduğu olay işleyicisi `CommandName` çünkü değer *herhangi* `Command` olayı tarafından *herhangi* düğmesi, LinkButton, veya Yineleyici içinde ImageButton neden olacak `ItemCommand` tetiklenecek olay. Biz şu anda yalnızca bir LinkButton şimdi olmakla birlikte, gelecekte biz (veya başka bir geliştirici ekibimiz üzerinde) ek düğmesi Web denetimleri yineleyici ekleyebilir, tıklatıldığında, aynı başlatır `ItemCommand` olay işleyicisi. Bu nedenle, bu her zaman, kontrol emin olmak en iyi s `CommandName` özelliği ve beklenen değeri eşleştiğini yalnızca programlı mantığı ile devam edin.

Geçilen olduktan `CommandName` değerine eşit ListProducts, olay işleyicisi sonra atar `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametre değerine geçilen `CommandArgument`. Bu değişikliği ObjectDataSource s `SelectParameters` otomatik olarak kendisini yeni seçilen kategori ürünleri gösteren veri kaynağına rebind DataList neden olur.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Bu eklemeleri ile öğreticimizi tamamlandı! Bir tarayıcıda çıkışı test etmek için bir dakikanızı ayırın. Şekil 14 ekran ilk sitesini ziyaret ettiğinde gösterir. Bir kategori henüz seçilmesi olduğundan hiç ürün görüntülenir. Üretim gibi bir kategori tıklayarak görüntüler bu ürünlerden iki sütun görünümünde ürün kategorisi (bkz. Şekil 15).


[![Görüntülenen zaman ilk ziyaret sayfa hiçbir ürün değil](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Şekil 14**: Hayır ürünleri olan görüntülenen zaman ilk ziyaret sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Üretim kategori listelerinde eşleşen ürünleri sağ tıklayarak](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Şekil 15**: ürün kategorisi tıklatarak sağa eşleşen ürünleri listeler ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Özet

Bu öğretici ve önceki bir gördüğümüz gibi ana/ayrıntı raporları iki sayfalara dağılmış olabilir veya bir birleştirilmiş. Ana/ayrıntı raporu tek bir sayfada görüntülemek, ancak bazı zorluklar nasıl en iyi düzeni asıl ve ayrıntıları kayıtları sayfasında tanıtır. İçinde *ana/Ayrıntılar DetailsView ile seçilebilir ana GridView kullanarak ayrıntı* biz ana kayıtları görünür ayrıntıları kayıtları içeriyor; Bu öğreticide CSS teknikleri master kayıtları float olmasını kullandık Öğreticisi sol tarafındaki ayrıntıları.

Ana/ayrıntı raporları görüntüleme yanı sıra, biz de sunucu tarafı mantığı LinkButton (veya düğme veya ImageButton olduğunda) gerçekleştirmek için içinden tıklandığında nasıl her kategoriyle ilişkili ürünleri sayısını almak üzere nasıl keşfetmek için Fırsat sahip bir Yineleyici.

Bu öğretici ana/ayrıntı raporları bizim incelenmesi DataList ve yineleyici tamamlar. Bizim sonraki öğreticileri kümesini düzenleme ve silme DataList denetimi yetenekleri ekleme gösterecektir.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) CSS CSS öğeleriyle kayan üzerinde bir öğretici
- [CSS konumlandırma](http://www.brainjar.com/css/positioning/) CSS ile öğeleri konumlandırma hakkında daha fazla bilgi
- [Yerleştirme çıkışı içerik HTML ile](http://www.w3schools.com/html/html_layout.asp) kullanarak `<table>` s ve diğer HTML öğeleri konumlandırma için

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Zack CAN oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-filtering-acess-two-pages-datalist-vb.md)
