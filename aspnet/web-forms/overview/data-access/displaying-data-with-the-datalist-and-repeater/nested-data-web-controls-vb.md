---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: İç içe veri Web denetimleri (VB) | Microsoft Docs
author: rick-anderson
description: Biz inceleyeceksiniz Bu öğreticide, içinde başka bir yineleyici Repeater'da kullanmayı iç içe. Örnekler, hem d iç Repeater doldurmak nasıl konacaktır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f7980d22d6ebc15a033cca321644a2bf1d4e3bb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390045"
---
<a name="nested-data-web-controls-vb"></a>İç içe veri Web denetimleri (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) veya [PDF olarak indirin](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> Biz inceleyeceksiniz Bu öğreticide, içinde başka bir yineleyici Repeater'da kullanmayı iç içe. Örnekler, iç Repeater bildirimli ve programlı olarak doldurmak nasıl çalışılacağını.


## <a name="introduction"></a>Giriş

Statik HTML ve veri bağlama söz dizimi ek olarak, şablonları Web denetimleri ve kullanıcı denetimleri de içerebilir. Bu Web denetimlerin özelliklerini olabilir bildirime dayalı, veri bağlama söz dizimi atanan veya uygun sunucu tarafında olay işleyicilerine programlı olarak erişilebilir.

Şablonu içindeki denetimler ekleyerek Görünüm ve kullanıcı deneyimi özelleştirilebilir ve üzerinde geliştirildi. Örneğin, [GridView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) öğretici, bir Takvim denetimi ekleyerek bir çalışan s alım tarihi göstermek için bir TemplateField; GridView s görüntüsünü özelleştirmek nasıl gördüğümüz [ekleme Doğrulama denetimleri düzenleme ve ekleme arabirimleri](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) ve [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) öğreticiler, gördüğümüz nasıl özelleştirileceğini düzenleme ve ekleme arabirimleri tarafından doğrulama ekleme denetimleri, metin kutuları, DropDownList ve diğer Web denetimleri.

Şablonlar, diğer veri Web denetimleri de içerebilir. Diğer bir deyişle, biz kendi şablonlarında başka bir DataList (veya yineleyici veya GridView veya DetailsView vb.) içeren bir DataList olabilir. Sınama böyle bir arabirim ile seçeneği, iç veri Web denetimi için uygun veri bağlama. Bildirim temelli seçeneklerden programlı olanlara ObjectDataSource kullanma arasında değişen kullanılabilir birkaç farklı yaklaşım vardır.

Biz inceleyeceksiniz Bu öğreticide, içinde başka bir yineleyici Repeater'da kullanmayı iç içe. Dış Repeater kategori s ad ve açıklama görüntüleme veritabanındaki her kategori için bir öğe içerir. Her kategori öğesi s iç Repeater bu kategoriye ait her ürün için bilgi görüntülenir (bkz. Şekil 1) bir madde işaretli liste. Örneklerimizde iç Repeater bildirimli ve programlı olarak doldurmak nasıl çalışılacağını.


[![Kendi ürünlerinin yanı sıra, her kategoriye listelenir](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Şekil 1**: yanı sıra kendi ürünleri her kategori listelenir ([tam boyutlu görüntüyü görmek için tıklatın](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>1. adım: kategori listesi oluşturma

Ben tasarlarken yararlı iç içe veri Web denetimleri kullanan bir sayfa oluşturmak, oluşturun ve en dıştaki veri Web denetimi, ilk olarak, hatta iç iç içe geçmiş denetim hakkında endişelenmeden test. Bu nedenle, bir yineleyici ad ve açıklama her kategori için listeler sayfasına eklemek gerekli adımları tarafından dolaşmaya başlayalım s olanak tanır.

Başlangıç açarak `NestedControls.aspx` sayfasını `DataListRepeaterBasics` klasörü ve Repeater denetimiyle ayarı sayfasına ekleyin, `ID` özelliğini `CategoryList`. Yineleyici s akıllı etiketten adlı yeni bir ObjectDataSource oluşturmayı tercih `CategoriesDataSource`.


[![Yeni ObjectDataSource CategoriesDataSource adı](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Şekil 2**: yeni ObjectDataSource ad `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](nested-data-web-controls-vb/_static/image6.png))


ObjectDataSource yapılandırın, veri çeker `CategoriesBLL` s sınıfı `GetCategories` yöntemi.


[![ObjectDataSource s CategoriesBLL sınıfı GetCategories yöntemi kullanmak üzere yapılandırma](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` s sınıfı `GetCategories` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](nested-data-web-controls-vb/_static/image9.png))


Yineleyici s şablonu belirtmek için içerik kaynak görünümüne gidin ve bildirim temelli söz dizimi el ile girmek ihtiyacımız var. Ekleme bir `ItemTemplate` s kategori adını görüntüler bir `<h4>` öğesi ve bir paragraf öğesini s kategori tanımı (`<p>`). Ayrıca, let s ayrı her kategori yatay bir kuralla (`<hr>`). Bu değişiklikleri yaptıktan sonra sayfanız, aşağıdakine benzer ObjectDataSource ve yineleyici için bildirim temelli söz dizimi içermelidir:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Şekil 4'te bir tarayıcıdan görüntülendiğinde ilerleme gösterir.


[![Her kategori s adı ve açıklaması, yatay bir kural tarafından ayrılmış listelenir](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Şekil 4**: her kategori s adı ve açıklaması listelenir, yatay bir kural tarafından ayrılmış ([tam boyutlu görüntüyü görmek için tıklatın](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>2. adım: iç içe geçmiş ürün Repeater ekleme

Tam listeleme kategorisiyle bizim sonraki görev için bir yineleyici eklemektir `CategoryList` s `ItemTemplate` uygun kategoriye ait bu ürünlerle ilgili bilgileri görüntüler. Çeşitli yollarla veri ikisi şunları kısa bir süre içinde keşfedeceğiz bu iç yineleyici için alabilirsiniz vardır. Şimdilik, let s yalnızca yineleyici ürünler oluşturun içinde `CategoryList` Repeater s `ItemTemplate`. Özellikle, yineleyici görünen bir madde işaretli listedeki her bir ürün her liste öğesi s ürün adı ve fiyat gibi ürün s olanak tanır.

İhtiyacımız iç Repeater s bildirim temelli söz dizimi ve şablonlara el ile girmek için bu Repeater oluşturmak için `CategoryList` s `ItemTemplate`. İçinde aşağıdaki işaretlemeyi ekleyin `CategoryList` Repeater s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>3. adım: Kategori özel ürünler için ProductsByCategoryList Repeater bağlama

Bu noktada tarayıcısından sayfayı ziyaret ederse, ekran Şekil 4 ile aynı olduğundan görünür ediyoruz ve tüm veriler için bir yineleyici henüz bağlamak için. Size uygun ürün kayıtları alın ve böylelikle diğerlerine göre biraz daha verimli Repeater bağlamak birkaç yolu vardır. Burada temel zorluk, belirtilen kategori için uygun ürün geri alamazsınız.

İç Repeater denetimine bağlamak veri ya da bildirimli olarak, bir ObjectDataSource içinde erişilebilir `CategoryList` Repeater s `ItemTemplate`, veya programlama yoluyla, ASP.NET sayfalarının arka plan kod sayfası. Benzer şekilde, bu verileri için iç yineleyici ya da bildirimli olarak - s iç Repeater ile bağlanabilir `DataSourceID` özelliği veya bildirim temelli veri bağlama söz dizimi aracılığıyla veya programlama aracılığıyla iç Yineleyicideki başvuran `CategoryList` Repeater s `ItemDataBound` programlı olarak ayarlama, olay işleyicisi, `DataSource` özelliği ve arama kendi `DataBind()` yöntemi. Bu yaklaşımların her birinin keşfedin s olanak tanır.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Bildirimli olarak ObjectDataSource Denetimi ile verilere erişme ve`ItemDataBound`olay işleyicisi

Bu yana ve yaygın olarak Bu öğretici serisinde, bu örnek ile ObjectDataSource takılıyor için verilerine erişmek için en iyi seçenek boyunca ObjectDataSource kullandık. `ProductsBLL` Sınıfında bir `GetProductsByCategoryID(categoryID)` belirtilen ait bu ürünleri hakkında bilgi döndüren yöntem *`categoryID`*. Bu nedenle, bir ObjectDataSource için ekleyebiliriz `CategoryList` Repeater s `ItemTemplate` ve bu sınıf s yöntemi kendi verilerine erişim yapılandırın.

Ne yazık ki, yineleyici eklenmemişse t, kendi şablonları yüzden bu ObjectDataSource denetimi için bildirim temelli söz dizimi el ile eklemek Tasarım görünümü düzenlenmesine izin verin. Aşağıdaki sözdizimi gösterildiği `CategoryList` Repeater s `ItemTemplate` bu yeni ObjectDataSource ekledikten sonra (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

ObjectDataSource yaklaşım kullanırken ayarlamak ihtiyacımız `ProductsByCategoryList` Repeater s `DataSourceID` özelliğini `ID` ObjectDataSource'nın (`ProductsByCategoryDataSource`). Ayrıca, bizim ObjectDataSource sahip bildirimi bir `<asp:Parameter>` belirten öğesi *`categoryID`* yöntemlere geçirilen değer `GetProductsByCategoryID(categoryID)` yöntemi. Ancak, bu değeri nasıl belirttiğimiz? İdeal olarak, d biz yalnızca ayarlayabilirsiniz `DefaultValue` özelliği `<asp:Parameter>` veri bağlama söz dizimini kullanarak öğesini şu şekilde:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Ne yazık ki, veri bağlama söz dizimi yalnızca sahip denetimleri geçerli bir `DataBinding` olay. `Parameter` Sınıfı bu tür bir olaya sahip değil ve bu nedenle, yukarıdaki söz dizimi geçersiz ve bir çalışma zamanı hatasına neden.

Bu değeri ayarlamak için bir olay işleyicisi oluşturmak ihtiyacımız `CategoryList` Repeater s `ItemDataBound` olay. Bu geri çağırma `ItemDataBound` olayı tetikler kez yineleyici için bağlı her bir öğe için. Bu nedenle, bu olay harekete dış yineleyici için her zaman size geçerli atayabilirsiniz `CategoryID` değerini `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametresi.

İçin bir olay işleyicisi oluşturun `CategoryList` Repeater s `ItemDataBound` aşağıdaki kod ile olay:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Bu olay işleyicisi, biz veri uğraşmanızı re yerine üst bilgi, alt bilgi veya ayırıcı öğesi öğe sağlayarak başlatır. Ardından, biz gerçek başvuru `CategoriesRow` geçerli bir yalnızca bağlı örnek `RepeaterItem`. Son olarak, biz de ObjectDataSource başvuru `ItemTemplate` ve atama kendi `CategoryID` parametre değerine `CategoryID` geçerli `RepeaterItem`.

Bu olay işleyicisi ile `ProductsByCategoryList` her yineleyici `RepeaterItem` bu ürünlerin içinde bağlı `RepeaterItem` s kategorisi. Şekil 5, sonuçta elde edilen çıktının bir ekran görüntüsü gösterilmektedir.


[![Dış Repeater her kategori listeler. İç bir kategori olduğu ürünleri listeler.](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Şekil 5**: dış Repeater listeler her kategorisi; ürünler o kategorinin iç bir listeleri ([tam boyutlu görüntüyü görmek için tıklatın](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Kategori veri ürünler program aracılığıyla erişme

Geçerli kategorisi için bir ürün almak için bir ObjectDataSource kullanmak yerine, bir yöntem bizim ASP.NET sayfalarının arka plan kod sınıfında oluşturabilir (veya `App_Code` klasör veya ayrı bir sınıf kitaplığı projesinde) uygun kümesini döndürür geçirilen zaman ürünleri bir `CategoryID`. Biz bu tür bir yöntem bizim ASP.NET sayfalarının arka plan kod sınıfında sahipti ve adlı Imagine `GetProductsInCategory(categoryID)`. Bu yöntemle yerinde biz ürünler için geçerli kategori aşağıdaki bildirim temelli söz dizimini kullanarak iç Repeater bağlayabilirsiniz:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Yineleyici s `DataSource` özelliği alanından gelir verilerini göstermek için veri bağlama söz dizimi kullanan `GetProductsInCategory(categoryID)` yöntemi. Bu yana `Eval("CategoryID")` türünde bir değer döndürür `Object`, biz nesneye dönüştürme bir `Integer` içine geçirmeden önce `GetProductsInCategory(categoryID)` yöntemi. Unutmayın `CategoryID` erişilen veri bağlama söz dizimi şöyledir `CategoryID` içinde *dış* yineleyici (`CategoryList`), o s bir bağlı kayıtlara `Categories` tablo. Bu nedenle, biliyoruz `CategoryID` bir veritabanı `NULL` biz körüne çevirebilirsiniz neden olan değer `Eval` olmadığını denetlemeden yöntemi biz uğraşmanızı re bir `DBNull`.

Bu yaklaşımda, oluşturmamız gerekir `GetProductsInCategory(categoryID)` yöntemi ve sağlanan verilen ürünleri uygun kümesini almak *`categoryID`*. Bu basitçe döndürerek yapabileceğimiz `ProductsDataTable` tarafından döndürülen `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi. Oluşturma s izin `GetProductsInCategory(categoryID)` için arka plan kod sınıfı yönteminde bizim `NestedControls.aspx` sayfası. Aşağıdaki kodu kullanarak bunu yapın:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Bu yöntem yalnızca bir örneğini oluşturur `ProductsBLL` yöntemi ve sonuçlarını döndürür `GetProductsByCategoryID(categoryID)` yöntemi. Yöntemi işaretlenmelidir Not `Public` veya `Protected`; yöntem işaretlenmişse `Private`, ASP.NET sayfası s bildirim temelli biçimlendirmeden erişilebilir olmayacaktır.

Bu yeni teknik kullanmak için bu değişiklikleri yaptıktan sonra bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. ObjectDataSource kullanırken çıktısı çıktıya benzer olmalıdır ve `ItemDataBound` olay işleyicisi yaklaşım (ekran görüntüsünü görmek için Şekil 5'e yeniden bakın).

> [!NOTE]
> Oluşturulacak iş karmaşıklığının gibi görünebilir `GetProductsInCategory(categoryID)` ASP.NET sayfalarının arka plan kod sınıfı yöntemi. Bu yöntem yalnızca bir örneğini oluşturur, `ProductsBLL` sınıfı ve sonuçlarını döndürür, `GetProductsByCategoryID(categoryID)` yöntemi. Neden bu yöntem doğrudan iç yineleyicideki veri bağlama sözdiziminden gibi de çağırabilirsiniz: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Bu söz dizimi geçerli kararlılığımızın t çalışma kazanılan rağmen `ProductsBLL` sınıfı (bu yana `GetProductsByCategoryID(categoryID)` yöntemi bir örnek yöntemi olduğundan), değiştirebilir `ProductsBLL` statik içerecek şekilde `GetProductsByCategoryID(categoryID)` yöntemi veya statik bir dahilsınıfı`Instance()` yöntemi yeni bir örneğini döndürülecek `ProductsBLL` sınıfı.


Bu tür değişiklikler gereksinimini ortadan kaldırır ancak `GetProductsInCategory(categoryID)` ASP.NET sayfalarının arka plan kod sınıfı yönteminde, arka plan kod sınıfı yöntemi sağladığı kısa bir süre içinde anlatıldığı gibi alınan, verilerle daha fazla esneklik.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Tüm tek seferde ürün bilgileri alınıyor

İki denetlerler teknikleri biz incelenir ve çağrısı yaparak bu ürünler için geçerli kategori alın `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi (bir ilk yaklaşım bunu bir ObjectDataSource aracılığıyla ikinci yaptığınız `GetProductsInCategory(categoryID)` yöntemi arka plan kod sınıfı). Bu yöntem çağrıldığında, her zaman veri erişim katmanına, iş mantığı katmanı çağrıları, sorgular veritabanı tablosundan satırları döndüren bir SQL deyimi ile `Products` ayarlanmış tablo `CategoryID` sağlanan giriş parametresi alan eşleşir.

Verilen *N* kategoriler sisteminde, bu yaklaşım netleştirir *N* + kategorilerin tümünü almak için veritabanını tek veritabanı sorgusu 1 çağrıları ve ardından *N* ürünleri almak için çağrıları Her kategori için belirli. Ancak, yalnızca iki veritabanı çağrıları tek aramada tüm kategorileri ve diğer tüm ürünleri almak için almak için gerekli tüm verileri alıyoruz olabilir. Biz tüm ürünleri aldıktan sonra Biz bu ürünlerin şekilde filtreleyebilirsiniz yalnızca geçerli eşleşen ürünleri `CategoryID` bu kategoriye s ilişkili iç yineleyici.

Bu işlevselliği sağlayacak şekilde, yalnızca küçük bir değişiklik yapmak ihtiyacımız `GetProductsInCategory(categoryID)` bizim ASP.NET sayfalarının arka plan kod sınıfı yöntemi. Farkında olmadan sonuçlarını döndürmek yerine `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemini çözmeye çalışacağız bunun yerine ilk erişim *tüm* ürünlerin (bunlar t başlattıysanız, bırakıldı zaten erişilen) ve ardından yalnızca filtrelenen görünümünü dönün ürünleri tabanlı geçirilen açma üzerinde `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Sayfa düzeyi değişkenin eklenmesi Not `allProducts`. Bu, tüm ürünleri ile ilgili bilgileri tutar ve ilk kez doldurulur `GetProductsInCategory(categoryID)` yöntemi çağrılır. Olduktan sonra `allProducts` nesne oluşturulur ve doldurulur, yalnızca satır olacak şekilde ayarlanmış yöntemi DataTable s sonuçları filtreleyen `CategoryID` belirtilen eşleşen `CategoryID` erişilebilir. Bu yaklaşım veritabanı erişilen sayısını azaltır *N* + 1 iki gösteriyor.

Bu geliştirme biçimlendirmenin sayfanın için herhangi bir değişiklik sunmaz ve diğer bir yaklaşım daha az kayıt neden olmaz. Yalnızca veritabanı çağrılarının sayısını da azaltır.

> [!NOTE]
> Bir kolayca veritabanı erişimlerine sayısını azaltmayı assuredly performansını, neden. Ancak, bu durumda olmayabilir. Çok sayıda ürün varsa, `CategoryID` olduğu `NULL`, örnek ve ardından çağrısı `GetProducts` yöntemi hiçbir zaman görüntülenen ürünleri sayısını döndürür. Ayrıca, tüm ürünleri döndürmek kısıp varsa, yalnızca durum sayfalama uyguladıysanız olabilen kategorileri kümesini gösteren re.


Her zaman iki teknik performansını analiz etme geldiğinde, yalnızca surefire ölçü uygulama s ortak örneği senaryolarınızı için uyarlanmış denetimli testleri çalıştırmak için aynıdır.

## <a name="summary"></a>Özet

Bu öğreticide Web denetimi içindeki başka bir veri içe nasıl özellikle bir dış Repeater listeleme ürünleri madde işaretli listede her kategori için bir iç Repeater ile her kategori için bir öğe görüntülemek nasıl İnceleme gördük. Bir iç içe geçmiş kullanıcı arabirimi oluşturmanın temel zorluk erişme ve iç veri Web denetimi için doğru veri bağlama arasındadır. Biz bu öğreticide incelenirken ikisi teknikleri, çeşitli vardır. İncelenirken bir ilk yaklaşım bir ObjectDataSource dış veri s Web denetimi için kullanılan `ItemTemplate` aracılığıyla iç veri Web denetimine bağlıydı, `DataSourceID` özelliği. İkinci yöntem, verileri ASP.NET sayfası s arka plan kod sınıfı bir yöntem aracılığıyla erişilir. Bu yöntem iç veri Web denetimi s ardından bağlanabilir `DataSource` veri bağlama söz dizimi aracılığıyla özelliği.

Bu öğreticide incelenirken iç içe geçmiş kullanıcı arabirimi Repeater'da içinde iç içe geçmiş bir yineleyici kullanılabilir. ancak, bu teknikler diğer veri Web denetimleri için genişletilebilir. Repeater'da GridView veya bir DataList içinde GridView içinde iç içe ve benzeri.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Zack Jones ve Liz Shulok yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
