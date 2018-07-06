---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: (C#) bir Ayrıntılar DataList'i ile madde işaretli ana kayıt listesi kullanan ana/ayrıntı | Microsoft Docs
author: rick-anderson
description: Bu öğreticide biz iki sayfalık ana/ayrıntı rapor önceki öğreticinin tek bir sayfada, madde işaretli kategori adları listesini gösteren t sıkıştırmak...
ms.author: aspnetcontent
ms.date: 10/17/2006
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 0be6a03b2fbbf71a1f1d1e13bd4b9118981d0715
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841069"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>(C#) bir Ayrıntılar DataList'i ile madde işaretli ana kayıt listesi kullanan ana/ayrıntı
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) veya [PDF olarak indirin](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> Bu öğreticide size iki sayfalık ana/ayrıntı rapor önceki öğreticinin tek bir sayfada, ekranın sağ seçili kategorinin ürünler ve ekranın sol tarafındaki bir madde işaretli liste kategori adları gösteren sıkıştırın.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](master-detail-filtering-acess-two-pages-datalist-cs.md) iki sayfada ana/ayrıntı raporu nasıl inceledik. Ana sayfada bir madde işaretli liste kategorilerin işlemek için Repeater denetimiyle kullanılır. Her kategori adı olan bir köprü tıklandığında, Al iki sütunlu DataList bu ürünlerin burada gösterilen Ayrıntıları sayfası, kullanıcıya ait, seçilen kategoriye.

Bu öğreticide size iki sayfalık öğretici tek bir sayfada, bir LinkButton işlenen her kategori adı ile ekranın sol tarafında bir madde işaretli liste kategori adları gösteren sıkıştırın. Kategori adı LinkButtons birine tıklayarak bir geri gönderme sevk ve Seçili kategoriyi s ürünleri ekranın sağ iki sütunlu DataList'te bağlar. Her kategori s adı görüntülenmesinin yanı sıra soldaki Repeater var. kaç toplam ürünleri için belirli bir kategori gösterilir (bkz. Şekil 1).


[![Kategori adı s ve ürünleri toplam bir sayı soldaki bölmede görüntülenir](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Şekil 1**: Kategori s adı ve toplam sayı ürünleri soldaki bölmede görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>1. adım: bir yineleyici ekranın sol bölümünde görüntüleme

Bu öğretici için seçilen kategori s ürünlerinin sol içerisinde madde işaretli liste kategorilerin sağlamak ihtiyacımız var. Bir web sayfası içeriği konumlandırılmış standart HTML öğelerini paragraf etiketleri bölünemez boşluklar kullanarak `<table>` s ve benzeri veya geçişli stil sayfası (CSS) teknikleri aracılığıyla. Konumlandırma için öğreticilerimizi tüm CSS teknikleri şimdiye kadarki kullandınız. Ne zaman oluşturduk Gezinti kullanıcı arabirimi bizim ana sayfasında [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-cs.md) kullandık öğretici *mutlak konumlandırma*, gezinme için kesin piksel uzaklığını belirten Liste ve ana içerik. Alternatif olarak, CSS sağa veya sola başka bir öğeye konumlandırmak için kullanılabilir *kayan*. DataList solundaki Repeater kayan göre seçilen kategori s ürünlerinin sol içerisinde madde işaretli liste kategorilerin sahibiz

Açık `CategoriesAndProducts.aspx` gelen sayfasında `DataListRepeaterFiltering` klasör ve bir tekrarlayan ve DataList sayfaya ekleyin. Yineleyici s ayarlamak `ID` için `Categories` ve DataList s için `CategoryProducts`. Kaynak görünümüne gidin ve kendi içindeki tekrarlayan ve DataList denetimler yerleştirme `<div>` öğeleri. Diğer bir deyişle, içinde Yineleyicinin içine bir `<div>` öğesi ilk ve daha sonra kendi içinde DataList `<div>` hemen sonra yineleyici öğesi. Bu noktada, biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

DataList solundaki Repeater kaydırmak için kullanılacak ihtiyacımız `float` CSS stil özniteliği şu şekilde:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

`float: left;` İlk gezinen `<div>` sol tarafındaki ikinci öğe. `width` Ve `padding-right` ayarları belirtmek ilk `<div>` s `width` ve ne kadar doldurma arasında eklenir `<div>` s öğe içeriği ve sağ alt kenar boşluğu. Öğeleri CSS kayan hakkında daha fazla bilgi için kullanıma [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Doğrudan ilk aracılığıyla da style ayarını belirtmek yerine `<p>` öğe s `style` özniteliği, bunun yerine, yeni bir CSS sınıfı oluşturma s versin `Styles.css` adlı `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

Yerini alabilecek sonra `<div>` ile `<div class="FloatLeft">`.

CSS sınıfının ekleme ve biçimlendirme içinde yapılandırma sonrasında `CategoriesAndProducts.aspx` sayfasında, tasarımcıya gidin. DataList solunda (sağ artık hem de yalnızca görünse ve henüz kendi veri kaynakları veya şablonları yapılandırmak için Biz bu yana kutuları gri olarak) kayan Repeater görmeniz gerekir.


[![Yineleyici DataList solunda Yüzdürülür](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Şekil 2**: yineleyici DataList solunda kaydırıldı ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>2. adım: her kategori için ürün sayısını belirleme

Biçimlendirme tamamlandı çevreleyen tekrarlayan ve DataList s ile biz yineleyici için bir kategori veri bağlamak için hazır yeniden denetleyin. Şekil 1'deki kategorilerin madde işaretli listede gösterildiği gibi ancak ek olarak her kategori adı da kategorisiyle ilişkili ürünleri sayısını görüntülemek ihtiyacımız var. Bu bilgilere erişmek için biz şunlardan birini yapabilirsiniz:

- **ASP.NET sayfası s arka plan kod sınıfı bu bilgiyi belirler.** Belirli bir verilen *`categoryID`* ilişkili ürün sayısı çağırarak belirleyebiliriz `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi. Bu yöntem döndürür bir `ProductsDataTable` nesnesi `Count` özelliği gösterir kaç `ProductsRow` s yoksa, hangi ürünler için belirtilen sayısıdır *`categoryID`*. Oluşturabiliriz bir `ItemDataBound` çağıran, yineleyici için bağlı her kategori için bir yineleyici için olay işleyicisi `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ve Çıkışta sayımına içerir.
- **Güncelleştirme `CategoriesDataTable` eklemek Typed DataSet içinde bir `NumberOfProducts` sütun.** Biz sonra güncelleştirebilirsiniz `GetCategories()` yönteminde `CategoriesDataTable` bu bilgiyi içer veya alternatif olarak, çıkmak için `GetCategories()` olarak-olan ve yeni bir `CategoriesDataTable` adlı bir yöntem `GetCategoriesAndNumberOfProducts()`.

Her iki tekniğin keşfedin s olanak tanır. Biz t veri erişim katmanı güncelleştirme gerek ki bu yana uygulamak bir ilk yaklaşım basittir; Ancak, veritabanı ile daha fazla iletişim gerektirir. Çağrı `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yönteminde `ItemDataBound` Yineleyicideki görüntülenen her kategori için bir ek veritabanı çağrısı olay işleyicisi ekler. Bu teknikte vardır *N* + 1 veritabanı çağrısı, burada *N* Yineleyicideki görüntülenen kategorileri sayısıdır. İkinci yaklaşımda her kategori hakkında bilgi içeren ürün sayısı döndürülür `CategoriesBLL` s sınıfı `GetCategories()` (veya `GetCategoriesAndNumberOfProducts()`) yöntemi, böylece yalnızca bir dönüş içinde veritabanına kaynaklanan.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Ürünleri ItemDataBound olay işleyicisinde sayısını belirleme

Her kategorideki s yineleyicideki ürünleri sayısını belirleme `ItemDataBound` olay işleyicisi, bizim mevcut veri erişim katmanı herhangi bir değişiklik gerektirmez. Tüm değişiklikler, doğrudan içinde yapılabilir `CategoriesAndProducts.aspx` sayfası. Başlangıç adlı yeni bir ObjectDataSource ekleyerek `CategoriesDataSource` aracılığıyla Repeater s akıllı etiket. Ardından, yapılandırma `CategoriesDataSource` BT'nin, verileri alır. Bu nedenle ObjectDataSource `CategoriesBLL` s sınıfı `GetCategories()` yöntemi.


[![ObjectDataSource CategoriesBLL sınıfı s GetCategories() yöntemi kullanmak için yapılandırma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` s sınıfı `GetCategories()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Her öğe `Categories` Repeater tıklanabilir ve tıklandığında neden için gereksinim duyduğu `CategoryProducts` DataList, seçilen kategori için bu ürünlerin görüntülenecek. Bu aynı bu sayfaya bağlanarak köprü, her kategori yaparak gerçekleştirilebilir (`CategoriesAndProducts.aspx`), ancak geçen `CategoryID` önceki öğreticide çok gördüğümüz gibi querystring aracılığıyla. Bu yaklaşımın avantajı, belirli kategori s ürünleri görüntüleyen bir sayfa kullanılabilir bozulmasına ve bir arama motoru tarafından dizine emin olmanızdır.

Alternatif olarak, her kategoride bir LinkButton, Bu öğretici için kullanacağız yaklaşımdır yapabiliyoruz. Köprü olarak kullanıcı s tarayıcıda oluşturur ancak tıklandığında, bir geri gönderme sevk LinkButton; geri gönderme üzerinde DataList s ObjectDataSource Seçili kategoriye ait olan bu ürünleri görüntülemek üzere yenilenmesi gerekiyor. Bu öğreticide, bir köprü kullanarak bir LinkButton kullanmaktan daha anlamlı olur; Ancak, bir LinkButton kullanarak daha avantajlı olduğu diğer senaryolar olabilir. Bu örneğin köprü yaklaşım İdealden sırada LinkButton kullanarak keşfedin s olanak tanır. Anlatıldığı gibi bir LinkButton kullanarak bir kısmı köprü ile ortaya değil bazı zorluklara neden olur. Bu nedenle, bu öğreticide bir LinkButton kullanarak bu zorluklar vurgulayın ve köprü yerine bir LinkButton kullanmak istediğimiz, bu senaryoları için çözümler sağlanmasına yardımcı olur.

> [!NOTE]
> Bir köprü denetimini kullanarak bu öğreticinin yineleyin denetlemeleri veya `<a>` LinkButton yerine öğesi.


Aşağıdaki biçimlendirme Yineleyici ve ObjectDataSource için bildirim temelli söz dizimini gösterir. Not: yineleyici s şablonları bir LinkButton olarak her bir öğesi ile bir madde işaretli liste oluşturma


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> Bu öğretici için bir yineleyici görünüm durumunu etkin olması gerekir (Not Java'daki `EnableViewState="False"` Repeater s bildirim temelli söz). 3. adımda size bir olay işleyicisi s yineleyici için oluşturursunuz `ItemCommand` , biz güncelleştiriyor s ObjectDataSource s DataList olay `SelectParameters` koleksiyonu. Yineleyici s `ItemCommand`, ancak görünüm durumu devre dışı bırakılmışsa t yangın kazandı. Bkz: [A zorlu bir ASP.NET soru](http://scottonwriting.net/sowblog/posts/1263.aspx) ve [çözümünün](http://scottonwriting.net/sowBlog/posts/1268.aspx) neden hakkında daha fazla bilgi için bir yineleyici s görünüm durumu etkinleştirilmelidir `ItemCommand` olayının ateşlenmesine neden.


Linkbutton'a `ID` özelliği değerinin `ViewCategory` sahip değil, `Text` özellik kümesi. Yeni kategori adını görüntüler istedik, metin özelliğini bildirimli olarak, veri bağlama söz dizimi aracılığıyla ayarlarız şu şekilde:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Ancak, kategori s adını göstermek istiyoruz *ve* bu kategoriye ait olan ürün sayısı. Bu bilgiler s Repeater alınabilir `ItemDataBound` çağrısı yaparak olay işleyicisi `ProductBLL` s sınıfı `GetCategoriesByProductID(categoryID)` yöntemi ve ortaya çıkan kaç kayıtlar döndürülür belirleme `ProductsDataTable`, aşağıdaki kodu gösterilmektedir:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Biz, sağlayarak başlar biz sahip veri öğesi çalışma re (bir, `ItemType` olduğu `Item` veya `AlternatingItem`) ve ardından başvuru `CategoriesRow` geçerli bir yalnızca bağlı örnek `RepeaterItem`. Ardından, bu kategori için ürün sayısı bir örneğini oluşturarak belirleriz `ProductsBLL` çağırma, sınıf kendi `GetCategoriesByProductID(categoryID)` yöntemi ve ile döndürülen kayıt sayısını belirleyen `Count` özelliği. Son olarak, `ViewCategory` LinkButton ItemTemplate içindeki başvurular olduğu ve onun `Text` özelliği *CategoryName* (*NumberOfProductsInCategory*), burada  *NumberOfProductsInCategory* sıfır ondalık bir sayı olarak biçimlendirilmiş.

> [!NOTE]
> Alternatif olarak, ekledik bir *işlevi biçimlendirme* bir kategori s kabul eden ASP.NET sayfası s arka plan kod sınıfı için `CategoryName` ve `CategoryID` döndürür ve değerleri `CategoryName` sayısı ile birleştirilmiş ürün kategorisinde (çağırarak belirlendiği `GetCategoriesByProductID(categoryID)` yöntemi). Biçimlendirme bir işlevinin sonuçlarını LinkButton s metin özelliğini değiştirerek gereksinimini bildirimli olarak atanabilir `ItemDataBound` olay işleyicisi. Başvurmak [GridView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) veya [DataList ve Repeater göre verileri üzerine biçimlendirme](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) öğreticileri biçimlendirme işlevlerini kullanma hakkında daha fazla bilgi için.


Bu olay işleyici ekledikten sonra bir tarayıcı aracılığıyla sayfada test etmek için bir dakikanızı ayırın. Her kategorisi kategori s adı ve kategori ile ilişkili ürün sayısı görüntüleyen bir madde işaretli liste nasıl Listeleneceği unutmayın (bkz: Şekil 4).


[![Her kategori s adı ve numarası, ürünleri görüntülenir](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Şekil 4**: her kategori s adı ve numarası, ürünleri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Güncelleştirme`CategoriesDataTable`ve`CategoriesTableAdapter`ürün sayısı için her bir kategori eklemek için

S bağlı olarak her kategori için ürün sayısını belirleme yerine yineleyici için biz ayarlayarak bu işlemi kolaylaştırmak `CategoriesDataTable` ve `CategoriesTableAdapter` bu bilgileri yerel olarak eklemek için veri erişim katmanında. Bunu başarmak için size yeni bir sütun eklemeniz gerekir `CategoriesDataTable` ilişkili ürün sayısı tutacak. Bir DataTable öğesine yeni bir sütun eklemek için türü belirtilmiş veri kümesi açın (`App_Code\DAL\Northwind.xsd`) değiştirileceğini DataTable sağ tıklayın ve Ekle'yi seçin / sütun. Yeni bir sütun ekleyin `CategoriesDataTable` (bkz: Şekil 5).


[![Yeni bir sütun için CategoriesDataSource Ekle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Şekil 5**: yeni bir sütun ekleyin `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Bu adlı yeni bir sütun ekler `Column1`, farklı bir ad yazarak değiştirebilirsiniz. Bu yeni bir sütun yeniden adlandırma `NumberOfProducts`. Ardından, biz bu sütun s özelliklerini yapılandırmanız gerekir. Yeni bir sütun üzerinde tıklayın ve Özellikler penceresine gidin. Değiştirme s sütunu `DataType` özelliğinden `System.String` için `System.Int32` ve `ReadOnly` özelliğini `True`Şekil 6'da gösterildiği gibi.


![Veri türü ve yeni bir sütun salt okunur özelliklerini ayarlama](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Şekil 6**: ayarlayın `DataType` ve `ReadOnly` yeni bir sütun özellikleri


Sırada `CategoriesDataTable` artık bir `NumberOfProducts` sütun değerine ayarlı değil herhangi bir karşılık gelen TableAdapter s sorgular tarafından. Biz güncelleştirebilirsiniz `GetCategories()` yöntemi gibi bilgiler istiyoruz, bu bilgileri döndürmek için döndürülen her zaman kategori bilgileri alınır. Ancak, yalnızca ilişkili ürünleri için kategorileri nadir örnekler vardır (örneğin olarak yalnızca Bu öğretici için) sayısını almak ihtiyacımız durumunda size bırakabilirsiniz, `GetCategories()` olarak-olduğu ve bu bilgileri döndüren yeni bir yöntem oluşturun. Let s adlı yeni bir yöntem oluşturma, ikinci bu yaklaşımı kullanmak `GetCategoriesAndNumberOfProducts()`.

Bu yeni eklemek için `GetCategoriesAndNumberOfProducts()` yöntemi, sağ `CategoriesTableAdapter` ve yeni sorguyu seçin. Bu sayıda önceki öğreticilerde kullanılan ve TableAdapter sorgu Yapılandırma Sihirbazı, hangi biz yukarı getirir. Bu yöntem için sorgu satırlar döndüren bir geçici SQL deyimini kullanır belirterek Sihirbazı başlatın.


[![Geçici SQL deyimi kullanarak yöntemi oluşturma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Şekil 7**: yöntemini kullanarak bir geçici SQL ifadesi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![SQL deyimi satırları döndürür.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Şekil 8**: SQL deyimi satırları döndürür ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


Sonraki sihirbaz ekranında bizim için kullanılacak sorguyu ister. Her kategori s döndürülecek `CategoryID`, `CategoryName`, ve `Description` alanlar, kategori ile ilişkili ürün sayısı ile birlikte kullanmak aşağıdaki `SELECT` deyimi:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Kullanılacak bir sorgu belirtin](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Şekil 9**: kullanılacak bir sorgu belirtin ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Diğer adlı olarak hesaplar kategorisiyle ilişkili ürün sayısı alt sorgu unutmayın `NumberOfProducts`. Bu adlandırma eşleşme ile ilişkili için bu alt sorgu tarafından döndürülen değer neden `CategoriesDataTable` s `NumberOfProducts` sütun.

Bu sorgu girdikten sonra yeni yöntemin adı için son adımı seçmektir. Kullanım `FillWithNumberOfProducts` ve `GetCategoriesAndNumberOfProducts` dolgu bir DataTable ve dönüş DataTable, sırasıyla desen.


[![Yeni bir TableAdapter s yöntemleri FillWithNumberOfProducts adı ve GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Şekil 10**: yeni bir TableAdapter s yöntemleri adında `FillWithNumberOfProducts` ve `GetCategoriesAndNumberOfProducts` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


Bu noktada veri erişim katmanı Kategori başına ürün sayısı içerecek şekilde genişletilmiştir. Ayrı iş mantığı katmanı aracılığıyla DAL için tüm çağrıları bizim sunu katmanı yönlendiren beri karşılık gelen eklemek ihtiyacımız `GetCategoriesAndNumberOfProducts` yönteme `CategoriesBLL` sınıfı:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

DAL ve BLL tam biz re hazır bu verilere bağlamak için `Categories` Yineleyicideki `CategoriesAndProducts.aspx`! Önceden belirleme gelen yineleyici için bir ObjectDataSource zaten oluşturduysanız, bu, ürünler, `ItemDataBound` olay işleyicisi bölümünde, bu ObjectDataSource silin ve yineleyici s kaldırmak `DataSourceID` özelliği ayarı; ayrıca unwire Yineleyici s `ItemDataBound` kaldırarak olay işleyici olaydan `Handles Categories.OnItemDataBound` ASP.NET arka plan kod sınıfı sözdizimi.

Adlı yeni bir ObjectDataSource özgün durumuna geri içinde Repeater ile ekleme `CategoriesDataSource` aracılığıyla Repeater s akıllı etiket. ObjectDataSource kullanmak için yapılandırma `CategoriesBLL` sınıfı, ancak bunu kullanmak yerine `GetCategories()` yöntemine sahip kullanmak `GetCategoriesAndNumberOfProducts()` yerine (bkz. Şekil 11).


[![ObjectDataSource GetCategoriesAndNumberOfProducts yöntemi kullanmak üzere yapılandırma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `GetCategoriesAndNumberOfProducts` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


Ardından, güncelleştirme `ItemTemplate` böylece LinkButton s `Text` özelliği, veri bağlama söz dizimini kullanarak bildirimli olarak atanır ve her ikisi de içerir `CategoryName` ve `NumberOfProducts` veri alanları. Yineleyici için tam bildirim temelli biçimlendirme ve `CategoriesDataSource` ObjectDataSource izleyin:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

DAL içerecek şekilde güncelleştirerek işlenen çıkışı bir `NumberOfProducts` kullanarak aynı olduğu sütun `ItemDataBound` olay işleyicisi yaklaşım (ekran görmek için Şekil 4'e yeniden bakın kategori adları ve ürün sayısını gösteren bir yineleyici görüntüsü).

## <a name="step-3-displaying-the-selected-category-s-products"></a>3. adım:, seçilen kategori s ürünleri görüntüleme

Bu noktada sahibiz `Categories` her bir kategorideki ürün sayısını kategori listesi görüntüleme yineleyici. Yineleyicinin bir LinkButton tıklandığında, nedenleri, geri gönderme bir işaret etmesini, biz, her kategori için kullanır. Bu seçilen kategori ürünlerin görüntülemeniz `CategoryProducts` DataList.

Bize e yönelik bir güçlük, seçilen kategori için yalnızca bu ürünlerin görüntüleme DataList sahip şeklidir. İçinde [ana/Ayrıntılar DetailsView seçilebilir bir ana GridView kullanan Detail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) satırları GridView oluşturma gördüğümüz öğretici seçilmesi, seçili satırı bir DetailsView aynı sayfada görüntülenmesini s ayrıntıları. GridView s kullanarak tüm ürünlerle ilgili bilgileri ObjectDataSource döndürülen `ProductsBLL` s `GetProducts()` yöntemi DetailsView s ObjectDataSource çalışırken alınan seçili ürün kullanma hakkında bilgi `GetProductsByProductID(productID)` yöntemi. *`productID`* Parametre değeri sağlandı bildirimli olarak GridView s değeri ile ilişkilendirerek `SelectedValue` özelliği. Ne yazık ki, yineleyici bulunmayan bir `SelectedValue` özelliği ve parametre kaynağı olarak hizmet veremez.

> [!NOTE]
> Repeater'da LinkButton kullanırken görünen bu zorluklardan biri budur. Köprü olarak geçirilecek kullandık vardı `CategoryID` sorgu dizesi bunun yerine, bu sorgu dizesi alanı kaynağı olarak parametre s değeri için kullanabiliriz.


Biz eksikliği hakkında endişe önce bir `SelectedValue` yineleyici özelliği ilk DataList bağlamak için bir ObjectDataSource ve belirtin, izin kendi `ItemTemplate`.

DataList s akıllı etiketten adlı yeni bir ObjectDataSource eklemek için iyileştirilmiş `CategoryProductsDataSource` ve kullanacak şekilde yapılandırma `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi. Bu öğreticide DataList salt okunur bir arabirim sunar. bu yana, INSERT, UPDATE, açılan listeler ve sekmeleri (hiçbiri) silme çekinmeyin.


[![ObjectDataSource ProductsBLL sınıfı s GetProductsByCategoryID(categoryID) yöntemi kullanmak üzere yapılandırma](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Bu yana `GetProductsByCategoryID(categoryID)` yöntemi giriş parametresi bekliyor (*`categoryID`*), veri kaynağı Yapılandırma Sihirbazı'nı parametresi s kaynağını belirtmek sağlıyor. Kategoriler listelenen GridView veya bir DataList, d parametresi kaynak aşağı açılan liste denetimi ve ControlId için ayarladık `ID` veri Web denetimi. Ancak, yineleyici oturumda bu yana bir `SelectedValue` , kullanılamaz bir parametre kaynağı özelliği. İşaretlerseniz, ControlId açılır listede yalnızca bir denetimi içerdiğini göreceksiniz `ID``CategoryProducts`, `ID` DataList.

Şimdilik, parametre kaynak açılır listede yok olarak ayarlayın. Yineleyicideki LinkButton tıklandığında kategori olduğunda bu parametreyi programlı olarak atama yukarı elde edersiniz.


[![' % S'CategoryID parametresi için bir parametre kaynağı yapmak belirtme](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Şekil 13**: bir parametrenin kaynağı için belirtmeyin *`categoryID`* parametre ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik-s DataList oluşturur `ItemTemplate`. Bu varsayılanı değiştirmek `ItemTemplate` şablonuyla biz önceki öğreticide kullanılan; Ayrıca, s DataList Ayarla `RepeatColumns` özelliği 2. Bu değişiklikleri yaptıktan sonra bildirim temelli biçimlendirme DataList ve onun ilişkili ObjectDataSource aşağıdaki gibi görünmelidir:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

Şu anda `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* parametresi hiçbir zaman ayarlanırsa, ürün sayfayı görüntülerken görüntülenecek şekilde. Bu parametre değer temel alınarak ayarlanmış yapmak ihtiyacımız olan `CategoryID` yineleyicideki tıklandı kategorisi. Bu iki zorluklara neden olur: ilk olarak nasıl belirleriz bir LinkButton olduğunda s yineleyicideki `ItemTemplate` tıklandı; ve ikinci, nasıl biz belirleyebilir `CategoryID` karşılık gelen kategorisi, Linkbutton'a tıkladı?

Düğme ve ImageButton denetimleri gibi LinkButton sahip bir `Click` olay ve [ `Command` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Olay, yalnızca Linkbutton'a tıklandı unutmayın için tasarlanmıştır. Bazen, ancak Linkbutton'a tıklandı belirtmeye yanı sıra ayrıca bazı ek bilgiler olay işleyicisine geçirilecek ihtiyacımız var. Bu durumda, LinkButton s ise [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) ve [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) özellikleri ek bu bilgiler atanabilir. Ardından LinkButton tıklandığında, `Command` olay harekete geçirilir (yerine kendi `Click` olay) ve olay işleyicisi değerlerini geçirilir `CommandName` ve `CommandArgument` özellikleri.

Olduğunda bir `Command` olayı oluşturulur gelen yineleyicideki Repeater s şablonu içindeki [ `ItemCommand` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) tetikler ve geçirilen `CommandName` ve `CommandArgument` tıklandı LinkButton değerlerini (veya düğmesini veya ImageButton). Bu nedenle, bir kategori yineleyicideki LinkButton tıklandığında belirlemek için biz aşağıdakileri yapmanız gerekir:

1. Ayarlayın `CommandName` s yineleyicideki LinkButton özelliği `ItemTemplate` bazı değerine (Ben ve kullanılan ListProducts). Bu ayarlandığında `CommandName` değeri LinkButton s `Command` LinkButton tıklandığında olayı tetikler.
2. LinkButton s ayarlamak `CommandArgument` s geçerli öğesinin değer özelliğini `CategoryID`.
3. S yineleyici için bir olay işleyicisi oluşturun `ItemCommand` olay. Olay işleyicisi, ayarlama `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametre değerine geçilen `CommandArgument`.

Aşağıdaki `ItemTemplate` biçimlendirme kategorileri yineleyici için 1 ve 2. adımları uygular. Not nasıl `CommandArgument` değeri veri öğesi s atandığı `CategoryID` veri bağlama söz dizimini kullanarak:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

Oluşturma her bir `ItemCommand` gelen her zaman ilk denetlenecek akıllıca olduğu olay işleyicisi `CommandName` çünkü değer *herhangi* `Command` olayı tarafından *herhangi* düğme, LinkButton, veya Yineleyici içinde ImageButton neden `ItemCommand` olayının ateşlenmesine neden. Biz şu anda yalnızca bir LinkButton artık olsa da, gelecekte biz (veya başka bir geliştirici ekibimiz) düğme Web denetimi için bir yineleyici ekleyebilirsiniz, tıklandığında, aynı başlatır `ItemCommand` olay işleyicisi. Bu nedenle, bu her zaman kontrol emin olmak en iyi s `CommandName` özelliği ve yalnızca beklenen değeri eşleşmesi durumunda, programlama mantığı ile devam edin.

Geçilen olduktan sonra `CommandName` değere eşit ListProducts, olay işleyicisi sonra atar `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametre değerine geçilen `CommandArgument`. Bu değişikliği ObjectDataSource s `SelectParameters` otomatik olarak yeni seçilen kategori ürünleri gösteren veri kaynağına, kendisi yeniden bağlamaya DataList neden olur.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

Bu eklemeleriyle öğreticimize tamamlandı! Bir tarayıcıda test etmek için bir dakikanızı ayırın. Şekil 14 ilk sayfasını ziyaret ederek ekranı gösterilir. Bir kategori henüz seçilmiş olması gerektiğinden, ürün görüntülenir. Örneğin, bir kategori tıklayarak görüntüler bu ürünlerin ürün kategorisinde bulunan iki sütunlu bir görünüm (bkz. Şekil 15).


[![Görüntülenen zaman ilk ziyaret sayfası olan hiçbir ürünler](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Şekil 14**: No ürünler olan görüntülenen zaman ilk ziyaret sayfa ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Ürün kategorisi listeleri eşleşen ürünleri sağ tıklayarak](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Şekil 15**: ürün kategorisi tıklayarak sağa eşleşen ürünleri listeler ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Özet

Bu öğretici ve önceki bir gördüğümüz gibi ana/ayrıntı raporlar, iki sayfada dağılmış olabilir veya birinde birleştirilir. Tek bir sayfada ana/Ayrıntılar rapor görüntüleme, ancak bazı zorluklar konusunda en iyi Düzen asıl ve Ayrıntılar kayıt sayfasında tanıtır. İçinde *ana/Ayrıntılar DetailsView seçilebilir bir ana GridView kullanan Detail* ana kayıtları görünür ayrıntıları kayıtları vardı; Bu öğreticide CSS teknikleri ana kayıtları kayan nokta sağlamak için kullandığımız Öğreticisi sol tarafındaki ayrıntıları.

Ana/Ayrıntılar raporları görüntüleme yanı sıra, ayrıca nasıl sunucu tarafı mantık LinkButton (veya düğme veya ImageButton olduğunda) gerçekleştirmek için içinden tıklandığında nasıl her kategori ile ilişkili ürünleri sayısını almak keşfetmek için Fırsat vardı bir Yineleyici.

Bu öğreticide, bizim DataList ve Repeater ile ana/ayrıntı raporları incelenmesi tamamlar. Sonraki kümemizdeki öğretici düzenleme ve silme olanağı DataList denetimi ekleme gösterecektir.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) CSS CSS öğelerle kayan bir öğretici
- [CSS konumlandırma](http://www.brainjar.com/css/positioning/) CSS ile öğeleri konumlandırma hakkında daha fazla bilgi
- [Yerleştirme kullanıma içerik HTML](http://www.w3schools.com/html/html_layout.asp) kullanarak `<table>` s ve diğer HTML öğeleri için konumlandırma

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Zack Jones oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [İleri](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
