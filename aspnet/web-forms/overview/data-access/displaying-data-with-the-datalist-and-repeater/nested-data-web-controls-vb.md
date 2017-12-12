---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: "İç içe veri Web denetimleri (VB) | Microsoft Docs"
author: rick-anderson
description: "Biz inceleyeceksiniz Bu öğreticide bir yineleyici kullanma başka bir yineleyici içinde iç içe geçmiş. Örnekler, her iki d iç yineleyici doldurmak nasıl çalışılacağını..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 944f208d6fe4f9fde13b530fb236ecc69ff5e9cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="nested-data-web-controls-vb"></a>İç içe veri Web denetimleri (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) veya [PDF indirin](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> Biz inceleyeceksiniz Bu öğreticide bir yineleyici kullanma başka bir yineleyici içinde iç içe geçmiş. Örnekler bildirimli olarak ve program aracılığıyla iç yineleyici doldurmak nasıl çalışılacağını.


## <a name="introduction"></a>Giriş

Statik HTML ve Veri bağlamada sözdizimi ek olarak, şablonları Web denetimleri ve kullanıcı denetimleri de içerebilir. Bu Web denetimlerin özelliklerini olabilir tanımlayıcı, veri bağlama sözdizimi atanan veya uygun sunucu tarafında olay işleyicilerine programlı olarak erişilebilir.

Bir şablonu içinde denetimleri gömerek Görünüm ve kullanıcı deneyimi özelleştirilebilir ve bağlı geliştirilmiş. Örneğin, [kullanarak TemplateFields GridView denetiminde](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) öğretici, bir Takvim denetimi ekleyerek bir çalışan s işe alma tarihi göstermek için bir TemplateField; GridView s görüntüsünü özelleştirmek nasıl gördüğümüz [ekleme Doğrulama denetimleri arabirimleri ekleme ve düzenleme için](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) ve [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) öğreticileri, gördüğümüz düzenleme özelleştirmeyi ve ekleme arabirimleri doğrulama ekleme denetimleri, metin kutuları, DropDownLists ve diğer Web denetler.

Şablonları diğer verileri Web denetimler de içerebilir. Diğer bir deyişle, biz kendi şablonlarını içinde başka bir DataList (veya yineleyici veya GridView veya DetailsView vb.) içeren bir DataList olabilir. Sınama böyle bir arabirim ile ilgili veri iç veri Web denetimi bağlama değil. ObjectDataSource programlı olanlara kullanarak bildirim temelli seçenekleri arasında değişen kullanılabilir birkaç farklı yaklaşım vardır.

Biz inceleyeceksiniz Bu öğreticide bir yineleyici kullanma başka bir yineleyici içinde iç içe geçmiş. Dış yineleyici veritabanında, her kategori için bir öğe kategori s adını ve açıklamasını görüntüleme içerir. Her kategori öğesi s iç yineleyici bu kategoriye ait her ürün için bilgi görüntülenir (bkz: Şekil 1) madde işaretli listede. Örneklerde bildirimli olarak ve program aracılığıyla iç yineleyici doldurmak nasıl çalışılacağını.


[![Kendi ürünlerinin yanı sıra, her kategoriye listelenir](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Şekil 1**: kendi ürünlerinin yanı sıra her kategori listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>1. adım: kategori listesi oluşturma

İç içe veri Web denetimleri kullanan bir sayfa oluşturma, ı tasarım yararlı, oluşturun ve hatta iç iç içe geçmiş denetim hakkında endişelenmeden en dıştaki veri Web denetimi ilk olarak, test. Bu nedenle, ad ve açıklama her kategori için listeler sayfası bir yineleyici eklemek gereken adımları adım adım ilerlemenizi sağlayarak Başlat s olanak tanır.

Başlangıç açarak `NestedControls.aspx` sayfasındaki `DataListRepeaterBasics` klasörü ve yineleyici denetimi ayarı sayfasına ekleyin, `ID` özelliğine `CategoryList`. Yineleyici s akıllı etiketten adlı yeni bir ObjectDataSource oluşturmak için seçtiğiniz `CategoriesDataSource`.


[![Yeni ObjectDataSource CategoriesDataSource adı](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Şekil 2**: yeni ObjectDataSource ad `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-data-web-controls-vb/_static/image6.png))


Böylece kendi veri çeker ObjectDataSource yapılandırma `CategoriesBLL` s sınıfı `GetCategories` yöntemi.


[![ObjectDataSource s CategoriesBLL sınıfı GetCategories yöntemi kullanmak üzere yapılandırma](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` s sınıfı `GetCategories` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-data-web-controls-vb/_static/image9.png))


Yineleyici s şablonu belirtmek için içerik biz kaynak görünümüne gidin ve bildirim temelli söz dizimini el ile girmeniz gerekir. Ekleme bir `ItemTemplate` kategori s adını görüntüleyen bir `<h4>` öğesi ve bir paragraf öğesini kategori s açıklamasında (`<p>`). Ayrıca, let s ayrı her kategori yatay bir kuralla (`<hr>`). Bu değişiklikleri yaptıktan sonra sayfanızı, aşağıdakine benzer ObjectDataSource ve yineleyici için tanımlayıcı sözdizimi içermelidir:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Şekil 4'te bir tarayıcıdan görüntülendiğinde bizim ilerleme durumunu gösterir.


[![Her kategori s ad ve açıklama, yatay bir kural tarafından ayrılmış listelenir](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Şekil 4**: her kategori s ad ve açıklama listelenir, yatay bir kural tarafından ayrılmış ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>2. adım: iç içe geçmiş ürün yineleyici ekleme

Tam listeleme kategorisiyle bizim sonraki görev için bir yineleyici eklemektir `CategoryList` s `ItemTemplate` uygun kategoriye ait bu ürünleri hakkında daha fazla bilgi görüntüler. Çeşitli yollarla verileri ikisi size kısa süre içinde ele alacağız bu iç yineleyici için alabilir vardır. Şimdilik, let s yalnızca yineleyici ürünleri oluşturmanız içinde `CategoryList` yineleyici s `ItemTemplate`. Özellikle, yineleyici görünen her ürünle madde işaretli listede her liste öğesi s ürün adı ve fiyat de dahil olmak üzere ürüne sahip s olanak tanır.

İhtiyacımız iç yineleyici s bildirim temelli söz dizimi ve şablonlara el ile girmek için bu yineleyici oluşturmak için `CategoryList` s `ItemTemplate`. İçinde aşağıdaki biçimlendirmeleri eklemek `CategoryList` yineleyici s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>3. adım: kategoriye özel ürünler ProductsByCategoryList yineleyici bağlama

Bu noktada sayfası bir tarayıcı aracılığıyla ziyaret ederseniz ekranınızı Şekil 4 ile aynı olduğundan şuna biz henüz herhangi bir veri yineleyici bağlamak için Taşı. Size uygun ürün kayıtları alın ve böylelikle diğerlerinden bazı daha verimli yineleyici bağlamak birkaç yolu vardır. En önemli güçlük burada belirtilen kategori için uygun ürün geri alamazsınız.

İç yineleyici denetimi bağlamak için verileri ya da bildirimli olarak, bir ObjectDataSource aracılığıyla erişilebilen `CategoryList` yineleyici s `ItemTemplate`, veya program aracılığıyla, ASP.NET sayfası s arka plan kod sayfası. Benzer şekilde, bu veriler iç yineleyici ya da bildirimli olarak - s iç yineleyici bağlanabilecek `DataSourceID` özelliği veya yoluyla bildirimsel veri bağlamayı sözdizimi veya program aracılığıyla iç Yineleyicideki başvuran `CategoryList` yineleyici s `ItemDataBound` programlı olarak ayarlama olay işleyicisi, kendi `DataSource` özelliği ve arama kendi `DataBind()` yöntemi. Bu yaklaşımların her birinin keşfedin s olanak tanır.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>ObjectDataSource Denetimi ile bildirimli olarak verilere erişilirken ve`ItemDataBound`olay işleyicisi

Bu yana ve Bu öğretici serisi, bu örnek ile ObjectDataSource takılıyor için verilere erişmek için en iyi seçenek genelinde yaygın ObjectDataSource kullandık. `ProductsBLL` Sınıfına sahip bir `GetProductsByCategoryID(categoryID)` belirtilen ait bu ürünlerle ilgili bilgileri döndürür yöntemi  *`categoryID`* . Bu nedenle, bir ObjectDataSource ekleyebiliriz `CategoryList` yineleyici s `ItemTemplate` ve bu sınıf s yönteminden kendi verilerine erişim yapılandırın.

Ne yazık ki, yineleyici içermiyor t Biz bu ObjectDataSource Denetimi bildirim temelli söz dizimi el ile eklemeniz gerekir böylece Tasarım görünümü düzenlenmesi için kendi şablonlarını verin. Aşağıdaki sözdizimi görülmektedir `CategoryList` yineleyici s `ItemTemplate` bu yeni ObjectDataSource ekledikten sonra (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

ObjectDataSource yaklaşım kullanırken ayarlamak ihtiyacımız `ProductsByCategoryList` yineleyici s `DataSourceID` özelliğine `ID` ObjectDataSource, (`ProductsByCategoryDataSource`). Ayrıca, bizim ObjectDataSource sahip bildirimi bir `<asp:Parameter>` belirtir öğesi  *`categoryID`*  içine geçirilen değer `GetProductsByCategoryID(categoryID)` yöntemi. Ancak Biz bu değerin nasıl belirtmezseniz? İdeal olarak, d biz yalnızca ayarlayabilmek `DefaultValue` özelliği `<asp:Parameter>` databinding sözdizimini kullanarak öğesi sözlüğüdür:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Ne yazık ki, veri bağlama sözdizimi yalnızca sahip denetimlerinde geçerli bir `DataBinding` olay. `Parameter` Sınıfı gibi bir olaya sahip değil ve bu nedenle yukarıdaki söz dizimi geçersiz ve bir çalışma zamanı hatası ile sonuçlanır.

Bu değeri ayarlamak için bir olay işleyicisi oluşturmak ihtiyacımız `CategoryList` yineleyici s `ItemDataBound` olay. Sözcüğünün `ItemDataBound` olay harekete kez yineleyici bağlı her bir öğe için. Bu nedenle, bu olay harekete dış yineleyici için her zaman biz geçerli atayabilirsiniz `CategoryID` değeri `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametresi.

İçin bir olay işleyicisi oluşturun `CategoryList` yineleyici s `ItemDataBound` aşağıdaki kod ile olay:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Bu olay işleyicisi biz veri postalarla re yerine üst bilgi, alt bilgi veya ayırıcı öğesi öğesi sağlayarak başlatır. Ardından, biz gerçek başvuru `CategoriesRow` geçerli bir yalnızca bağlı örneği `RepeaterItem`. Son olarak, biz ObjectDataSource başvuru `ItemTemplate` ve ata kendi `CategoryID` parametre değerini `CategoryID` geçerli `RepeaterItem`.

Bu olay işleyicisi ile `ProductsByCategoryList` yineleyici her `RepeaterItem` bu ürünlerinde bağlı `RepeaterItem` s kategorisi. Şekil 5 sonuçta çıktı ekran görüntüsü gösterilmektedir.


[![Dış yineleyici her kategori listeler. İç bir kategori ürünleri listeler](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Şekil 5**: dış yineleyici listeler her kategori; iç bir listeleri o kategorideki ürünleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Ürün kategorisi verileri tarafından program aracılığıyla erişme

Geçerli kategorideki ürünleri almak için bir ObjectDataSource kullanmak yerine, bir yöntem bizim ASP.NET sayfası s arka plan kodu sınıfında oluşturuyoruz (veya `App_Code` klasör veya ayrı bir sınıf kitaplığı Proje) uygun kümesini döndürür içinde geçirildiğinde ürünleri bir `CategoryID`. Biz bu tür bir yöntem bizim ASP.NET sayfası s arka plan kodu sınıfında var ve bu taşıyordu düşünün `GetProductsInCategory(categoryID)`. Bu yöntemle yerinde biz ürünler geçerli kategori için aşağıdaki bildirim temelli söz dizimini kullanarak iç yineleyici bağlayabilirsiniz:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Yineleyici s `DataSource` özelliği verilerini alanından gelir belirtmek için veri bağlama sözdizimini kullanan `GetProductsInCategory(categoryID)` yöntemi. Bu yana `Eval("CategoryID")` türünde bir değer döndürür `Object`, biz nesnesine cast bir `Integer` içine geçirmeden önce `GetProductsInCategory(categoryID)` yöntemi. Unutmayın `CategoryID` erişilen veri bağlama söz dizimi şöyledir `CategoryID` içinde *dış* yineleyici (`CategoryList`), o s bir bağlı kayıtları için `Categories` tablo. Biliyoruz bu nedenle, `CategoryID` bir veritabanı olamaz `NULL` biz doğrudan çevirebilirsiniz neden olan değeri, `Eval` varsa denetlemeden yöntemi biz postalarla re bir `DBNull`.

Bu yaklaşımda, oluşturmamız gerekir `GetProductsInCategory(categoryID)` yöntemi ve sağlanan verilen ürünleri uygun kümesini almak  *`categoryID`* . Bu basitçe döndürerek yapabiliriz `ProductsDataTable` tarafından döndürülen `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi. Oluşturma s izin `GetProductsInCategory(categoryID)` arka plandaki kod sınıfı yönteminde bizim `NestedControls.aspx` sayfası. Aşağıdaki kodu kullanarak bunu yapın:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Bu yöntem yalnızca bir örneğini oluşturur `ProductsBLL` yöntemi ve sonuçlarını döndürür `GetProductsByCategoryID(categoryID)` yöntemi. Yöntem işaretlenmelidir Not `Public` veya `Protected`; yöntemi işaretlenmişse `Private`, ASP.NET sayfası s bildirim temelli biçimlendirmeden erişilebilir olmaz.

Bu yeni teknik kullanmak için bu değişiklikleri yaptıktan sonra bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. ObjectDataSource kullanırken çıkışı Çıkış aynı olmalıdır ve `ItemDataBound` olay işleyicisi yaklaşım (ekran görmek için Şekil 5'e yeniden bakın).

> [!NOTE]
> Oluşturmak için busywork gibi görünebilir `GetProductsInCategory(categoryID)` ASP.NET sayfası s arka plandaki kod sınıfı yöntemi. Bu yöntem yalnızca bir örneğini oluşturur sonra tüm `ProductsBLL` sınıfı ve sonuçlarını döndürür, `GetProductsByCategoryID(categoryID)` yöntemi. Neden yalnızca bu yöntemi doğrudan iç yineleyici databinding sözdiziminde gibi çağırmanıza: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Bu sözdiziminin geçerli bizim uygulaması ile t çalışma won rağmen `ProductsBLL` sınıfı (bu yana `GetProductsByCategoryID(categoryID)` yöntemdir örnek yöntemi), değiştirebilir `ProductsBLL` statik içerecek şekilde `GetProductsByCategoryID(categoryID)` yöntemi veya statik dahilsınıfı`Instance()` yeni bir örneğini döndürmek için yöntemini `ProductsBLL` sınıfı.


Tür değişiklikler gereksinimini ortadan kaldırır ancak `GetProductsInCategory(categoryID)` ASP.NET sayfası s arka plandaki kod sınıfı yönteminde, arka plandaki kod sınıfı yöntemi verir bize kısa süre içinde anlatıldığı gibi alınan, verileri ile çalışma daha fazla esneklik.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Aynı anda tüm ürün bilgiler alınıyor

İki denetlerler teknikleri biz incelenmesi ve yapılan bir çağrı yaparak bu ürünler için geçerli kategori yakalayın `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi (ilk yaklaşım bunu bir ObjectDataSource aracılığıyla aracılığıyla ikinci vermedi `GetProductsInCategory(categoryID)` yöntemi arka plandaki kod sınıfı). Bu yöntem çağrıldığında, her zaman veri erişim katmanı için iş mantığı katmanı çağrıları, hangi sorgular satırları döndüren bir SQL deyimi veritabanıyla `Products` özelliği tablo `CategoryID` alan sağlanan giriş parametresi ile eşleşir.

Verilen *N* kategoriler sisteminde, bu yaklaşım belirlenir *N* + tüm kategorilerini almak için veritabanı bir veritabanı sorgusu 1 çağrıları ve ardından *N* ürünleri almak için çağrıları Her kategori özgüdür. Ancak, yalnızca iki veritabanı çağrıları tek aramada tüm kategorileri ve tüm ürünleri almak için başka bir almak için tüm gerekli veri alıyoruz olabilir. Biz ürünlerin tamamı olduktan sonra Biz bu ürünlerden şekilde filtreleyebilirsiniz yalnızca geçerli eşleşen ürünleri `CategoryID` bu kategoriye s bağlı iç yineleyici.

Bu işlevselliği sağlayacak şekilde yalnızca küçük bir değişiklik yapmak ihtiyacımız `GetProductsInCategory(categoryID)` bizim ASP.NET sayfası s arka plandaki kod sınıfı yöntemi. Doğrudan sonuçları döndüren yerine `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi, kuruluşunuzun yöneticisiyle bunun yerine ilk erişim *tüm* ürünlerin (bunlar t başlattıysanız, edilmiş zaten erişilen) ve ardından yalnızca filtre uygulanmış görünümüne dönün ürünlere dayalı geçilen üzerinde `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Sayfa düzeyi değişken eklenmesi Not `allProducts`. Bu, tüm ürünleri ile ilgili bilgileri tutar ve ilk kez doldurulur `GetProductsInCategory(categoryID)` yöntemi çağrılır. Olduktan `allProducts` nesnesi oluşturulur ve doldurulur, yalnızca satırları şekilde özelliği yöntemi DataTable s sonuçları filtreler `CategoryID` belirtilen eşleşen `CategoryID` erişilebilir. Bu yaklaşım veritabanı erişilen sayısını azaltır *N* + 1 iki aşağı kaydırın.

Bu geliştirme, sayfanın oluşturulan biçimlendirmenin herhangi bir değişiklik sunmaz veya başka bir yaklaşım'den daha az kayıtları dışı bırakmaz. Yalnızca veritabanı çağrılarının sayısını da azaltır.

> [!NOTE]
> Bir sezgisel veritabanı erişimlerine sayısının azaltılması assuredly performansı olduğunu neden. Ancak, bu durumda olmayabilir. Çok sayıda ürünleri varsa, `CategoryID` olan `NULL`, örnek sonra çağrısı için `GetProducts` yöntemi hiçbir zaman görüntülenen ürünleri bir sayı döndürür. Ayrıca, tüm ürünleri döndürüyor olabilir kayıp varsa, yalnızca bir alt durum disk belleği uyguladıysanız olabilen kategorilerinin gösteren re.


Her zaman, iki teknikleri performansını analiz etme geldiğinde, yalnızca surefire ölçü uygulama s ortak servis talebi senaryolarınız için uyarlanmış denetimli testleri çalıştırmak için aynıdır.

## <a name="summary"></a>Özet

Bu öğreticide bir veri Web denetimi başka içinde iç içe nasıl özellikle bir dış yineleyici madde işaretli listede her kategori için ürünleri listeleyen bir iç yineleyici her kategori için bir öğe görüntülemek nasıl inceleniyor gördük. Bir iç içe geçmiş kullanıcı arabirimi oluşturmanın en önemli güçlük erişme ve iç veri Web denetimine doğru veri bağlama arasındadır. İkisi biz Bu öğreticide incelenmesi teknikleri, çeşitli vardır. Dış veri Web denetimi s bir ObjectDataSource incelenmesi ilk yaklaşım kullanılan `ItemTemplate` iç veri Web denetimi için bağlıydı kendi `DataSourceID` özelliği. İkinci teknik verileri ASP.NET sayfası s arka plan kodu sınıfında bir yöntem aracılığıyla erişilir. Bu yöntem iç veri Web denetimi s sonra bağlanabilir `DataSource` özelliği üzerinden databinding sözdizimi.

Bu öğreticide incelenmesi iç içe geçmiş kullanıcı arabirimi yineleyici içinde iç içe geçmiş bir yineleyici kullanılan olsa da, bu teknikler diğer veri Web denetimleri için genişletilebilir. Yineleyici GridView veya bir DataList içinde GridView içinde iç içe ve benzeri.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Zack Can ve Liz Shulok yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
