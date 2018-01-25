---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: "Özel biçimlendirme (C#) verilerine dayalı | Microsoft Docs"
author: rick-anderson
description: "GridView, DetailsView veya ona veriye bağlı FormView biçimini ayarlama birden çok yolla gerçekleştirilebilir. Bu öğreticide l gerekir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 606721b01fae34a7bce85d497a442cb110f1b51e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="custom-formatting-based-upon-data-c"></a>Özel biçimlendirme (C#) verilerine göre
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) veya [PDF indirin](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> GridView, DetailsView veya ona veriye bağlı FormView biçimini ayarlama birden çok yolla gerçekleştirilebilir. Bu öğreticide bağlı veri kullanımı ile veriye bağlı ve RowDataBound olay işleyicileri biçimlendirme nasıl inceleyeceğiz.


## <a name="introduction"></a>Giriş

GridView, DetailsView ve FormView denetimlerin görünümünü stili ilgili özellikler çok sayıda özelleştirilebilir. Gibi özellikleri `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, ve `Height`, diğerleriyle birlikte, işlenen denetimi genel görünümünü dikte. Özellikler de dahil olmak üzere `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, ve diğerleri belirli bölümlerine uygulanacak aynı bu stilini ayarlar sağlar. Benzer şekilde, bu stilini ayarlar alan düzeyinde uygulanabilir.

Birçok senaryoda, yine de görüntülenen verileri değeri biçimlendirme gereksinimleri bağlıdır. Örneğin, dışı ürünler hisse senedi dikkat çekmek için ürün bilgileri listeleyen bir rapor, bu ürünler için sarı için arka plan rengi ayarlayabilir `UnitsInStock` ve `UnitsOnOrder` alanlardır her ikisi de 0'a eşit. Daha pahalı ürünleri vurgulamak için biz birden fazla $75.00 kalın yazı tipiyle maliyeti bu ürünlerden fiyatları görüntülemek isteyebilirsiniz.

GridView, DetailsView veya ona veriye bağlı FormView biçimini ayarlama birden çok yolla gerçekleştirilebilir. Bu öğreticide veriye kullanım yoluyla biçimlendirme nasıl inceleyeceğiz `DataBound` ve `RowDataBound` olay işleyicileri. Sonraki öğreticide biz alternatif bir yaklaşım ele alacağız.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>DetailsView denetimin kullanarak`DataBound`olay işleyicisi

Ne zaman veri bağlı bir DetailsView için veri kaynağı denetimi veya program aracılığıyla veri denetimin atama yoluyla `DataSource` özelliği ve arama kendi `DataBind()` yöntemi, sırasıyla aşağıdaki adımlardan oluşur:

1. Verinin Web denetimi `DataBinding` olay etkinleşir.
2. Verileri veri Web denetimi bağlıdır.
3. Verinin Web denetimi `DataBound` olay etkinleşir.

Özel mantık, adım 1 ve 3 olay işleyicisi aracılığıyla hemen sonra yerleştirilebilir. Bir olay işleyicisi oluşturarak `DataBound` olay program aracılığıyla belirleriz daha eski verileri veri Web denetime bağlı ve gerektiği gibi biçimlendirme ayarlayın. Bunu şimdi göstermek için bir ürün hakkında genel bilgiler listelenir, ancak görüntüleyecek bir DetailsView oluşturma `UnitPrice` değeri bir ***kalın, italik yazı tipi*** $75.00 aşarsa.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>1. adım: bir DetailsView'da ürün bilgilerini görüntüleme

Açık `CustomColors.aspx` sayfasındaki `CustomFormatting` klasörü, DetailsView denetimini tasarımcıya araç çubuğuna sürükleyin, Ayarla kendi `ID` özellik değerine `ExpensiveProductsPriceInBoldItalic`ve çağıran yeni bir ObjectDataSource denetimi bağlamak `ProductsBLL` sınıfının `GetProducts()` yöntemi. Biz bunları önceki eğitimlerine ayrıntılı olarak incelenmesi olduğundan bu işlemi gerçekleştirmek için ayrıntılı adımlar burada okumanızdır göz ardı edilir.

ObjectDataSource DetailsView'a bağladınız sonra alan listesini değiştirmek için bir dakikanızı ayırın. Kaldırmak için tercih `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, ve `Discontinued` BoundFields ve yeniden adlandırılamaz ve kalan BoundFields yeniden biçimlendirilen. I ayrıca temizlenmiş `Width` ve `Height` ayarlar. Yalnızca tek bir kaydını DetailsView görüntüler olduğundan, tüm ürünleri görüntülemek son kullanıcı izin vermek üzere ICollection gerekir. DetailsView'un akıllı etiket disk belleği etkinleştir onay kutusunu işaretleyerek bunu.


[![DetailsView'un akıllı etiket etkinleştirme disk belleği onay kutusunu işaretleyin](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Şekil 1**: etkinleştirmek disk belleği DetailsView'un akıllı etiketinde onay ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image3.png))


Bu değişikliklerden sonra DetailsView biçimlendirme olacaktır:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Bu sayfayı tarayıcınızda çıkışı test etmek için bir dakikanızı ayırın.


[![DetailsView denetimi her seferinde bir ürün görüntüler](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Şekil 2**: DetailsView denetimi görüntüler bir ürün birer birer ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>2. adım: Program aracılığıyla veriye bağlı olay işleyicisi verilerde değerini belirleme

Kalın, italik yazı tipi bu ürünler için fiyat görüntülemek için `UnitPrice` değeri aşıyor $75.00, ilk programlı olarak belirlemek mümkün olması için ihtiyacımız `UnitPrice` değeri. DetailsView için bu de gerçekleştirilebilir `DataBound` olay işleyicisi. Olay oluşturmak için işleyici DetailsView Tasarımcısı'nda tıklatın ardından Properties penceresine gidin. Görünür ya da Görünüm menüsünde gidin değilse, Getir için F4 tuşuna basın ve Özellikler penceresini menü seçeneğini belirleyin. Özellikler penceresinden DetailsView'un olayları listelemek için Şimşek Cıvata simgesine tıklayın. Ardından, ya da çift `DataBound` olay ya da oluşturmak istediğiniz olay işleyicisi adını yazın.


![Veriye bağlı olayı için bir olay işleyicisi oluşturun](custom-formatting-based-upon-data-cs/_static/image7.png)

**Şekil 3**: için bir olay işleyicisi oluşturun `DataBound` olayı


Bunun yapılması otomatik olay işleyicisi oluşturur ve burada eklendi kod bölümüne alın. Bu noktada görürsünüz:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Veri DetailsView'a bağlı aracılığıyla erişilen `DataItem` özelliği. Biz bizim denetimleri kesin türü belirtilmiş DataRow örneklerinin bir koleksiyonunu oluşan kesin türü belirtilmiş DataTable tablosuna, bağlama geri çağırma. DataTable DetailsView'a bağlandı, ilk DataRow DataTable tablosundaki DetailsView'un atanan `DataItem` özelliği. Özellikle, `DataItem` özelliği atandığında bir `DataRowView` nesnesi. Biz kullanabilirsiniz `DataRowView`'s `Row` olduğu temel alınan DataRow nesnesi erişmek için özellik gerçekte bir `ProductsRow` örneği. Biz bunu aldıktan sonra `ProductsRow` biz yapabilir bizim karar nesnenin özellik değerlerini inceleyerek örneği.

Aşağıdaki kodu nasıl belirleneceğini göstermektedir olup olmadığını `UnitPrice` denetimini bağlı değerdir 75.00 büyük:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Bu yana `UnitPrice` olabilir bir `NULL` değeri veritabanındaki ilk denetleyin biz ile ilgilenen değil olduğundan emin olmak için bir `NULL` erişmeden önce değer `ProductsRow`'s `UnitPrice` özelliği. Bu onay önemlidir çünkü erişmeye çalışırsanız `UnitPrice` olduğunda bu özellik bir `NULL` değeri `ProductsRow` nesnesi oluşturur bir [StrongTypingException özel durum](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>3. adım: DetailsView'da UnitPrice değeri biçimlendirme

Bu noktada belirleriz olup olmadığını `UnitPrice` DetailsView'a bağlı değer $75.00 aşan değerine sahip, ancak DetailsView programlı olarak ayarlama hakkında bilgi için uygun şekilde formatını henüz biz yaptık. Tüm bir satır DetailsView'da biçimlendirme değiştirmek için program aracılığıyla satır kullanarak erişim `DetailsViewID.Rows[index]`; belirli bir hücre değiştirmek, kullandığı erişimi `DetailsViewID.Rows[index].Cells[index]`. Satır veya hücre bir başvuru sahibiz sonra stili ilgili özellikleri ayarlayarak biz sonra görünümünü ayarlayabilirsiniz.

Bir satır program aracılığıyla erişme 0'da başlar sıranın dizini bilmeniz gerekir. `UnitPrice` Satır DetailsView'da 4 dizini vermiş ve programlı olarak erişilebilir hale getirme, beşinci satırdır kullanarak `ExpensiveProductsPriceInBoldItalic.Rows[4]`. Bu noktada biz aşağıdaki kodu kullanarak kalın, italik yazı tipiyle gösterilen tüm sıranın içeriğe sahip:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Ancak, bu yapacak *her ikisi de* (Fiyat) etiketi ve kalın ve italik değeri. Biz yalnızca bu uygulamak için aşağıdaki kullanılarak gerçekleştirilebilir satır ikinci hücreye biçimlendirme ihtiyacımız değer kalın ve italik yapmak istiyorsanız:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Öğreticilerimizi bugüne kadarki stil işlenmiş biçimlendirme ve stil ilgili bilgiler arasında temiz birbirinden ayırmaya kullanmış olduğundan, yukarıda şimdi bunun yerine gösterildiği gibi belirli stil özelliklerini ayarlama yerine bir CSS sınıfı kullanın. Açık `Styles.css` stil ve adlı yeni bir CSS sınıfı ekleme `ExpensivePriceEmphasis` aşağıdaki tanımıyla:


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

Ardından `DataBound` olay işleyici ayarlamayı hücrenin `CssClass` özelliğine `ExpensivePriceEmphasis`. Aşağıdaki kodda gösterildiği `DataBound` okumalıdır olay işleyicisi:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Küçüktür $75.00 maliyetleri, Chai görüntülerken fiyat normal bir yazı tipiyle görüntülenir (Şekil 4'e bakın). Ancak, $97.00 fiyatı olan Mishi Kobe Niku görüntülerken fiyat kalın, italik yazı tipiyle görüntülenir (bkz. Şekil 5).


[![Normal yazı tipiyle $75.00 sayısından az fiyatlar görüntülenir](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Şekil 4**: Normal yazı tipiyle $75.00 sayısından az fiyatlar görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image10.png))


[![Pahalı ürünlerin fiyatları bir kalın, italik yazı tipi görüntülenir](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Şekil 5**: pahalı ürünlerin fiyatları bir kalın, italik yazı tipi görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>FormView denetimin kullanarak`DataBound`olay işleyicisi

Bir DetailsView oluşturmak için temel alınan veri bir FormView bağlı belirlemek için adımları aynıdır bir `DataBound` olay işleyicisi cast `DataItem` uygun nesne türü özelliğine denetime bağlı ve devam etmek nasıl belirleyin. FormView ve DetailsView, Bununla birlikte, kendi kullanıcı arabiriminin görünümünü nasıl güncelleştirildiğini farklı.

FormView herhangi BoundFields içermiyor ve bu nedenle eksik `Rows` koleksiyonu. Bir FormView statik HTML bir karışımını içerebilir, şablonları bunun yerine, oluşan Web denetimleri ve Veri bağlamada sözdizimi. Genellikle bir FormView stilini ayarlama, bir veya daha fazla Web denetimleri FormView'ın şablonlar içindeki stilini ayarlama içerir.

Bunu göstermek için şimdi FormView listesi ürün için önceki örnekte, ancak bu kez şimdi gibi kullanın yalnızca ürün adı ve birimler 10 eşit veya daha az ise, kırmızı bir yazı tipiyle gösterilen stoktaki birimleri ile stoktaki.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>4. adım: bir FormView ürün bilgilerini görüntüleme

FormView için ekleme `CustomColors.aspx` sayfa DetailsView ve kümesi altında kendi `ID` özelliğine `LowStockedProductsInRed`. Önceki adımda oluşturulan ObjectDataSource Denetimi FormView bağlar. Bu oluşturacak bir `ItemTemplate`, `EditItemTemplate`, ve `InsertItemTemplate` FormView için. Kaldırma `EditItemTemplate` ve `InsertItemTemplate` ve basitleştirmek `ItemTemplate` dahil etmek için yalnızca `ProductName` ve `UnitsInStock` değerleri, her kendi uygun şekilde adlı etiket denetimleri. Önceki örnekten DetailsView ile aynı zamanda etkinleştirme disk belleği FormView'ın akıllı etiketinde onay gibi.

Sonra bu düzenlemeler FormView'ın biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Unutmayın `ItemTemplate` içerir:

- **Statik HTML** metni "Ürün:" ve "stoktaki birimler:" ile birlikte `<br />` ve `<b>` öğeleri.
- **Web denetimleri** iki etiket denetimleri `ProductNameLabel` ve `UnitsInStockLabel`.
- **Veri bağlama sözdizimi** `<%# Bind("ProductName") %>` ve `<%# Bind("UnitsInStock") %>` değerleri bu alanlardan etiket denetimleri için atar sözdizimi `Text` özellikleri.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>5. adım: Program aracılığıyla veriye bağlı olay işleyicisi verilerde değerini belirleme

Tam FormView'ın biçimlendirme ile programlı olarak belirlemek için sonraki adım olan `UnitsInStock` değer olan 10 az veya bu değere eşit. DetailsView haliyle FormView ile tam aynı şekilde gerçekleştirilir. Başlangıç FormView için 's olay işleyicisi oluşturarak `DataBound` olay.


![Veriye bağlı olay işleyicisi oluşturun](custom-formatting-based-upon-data-cs/_static/image14.png)

**Şekil 6**: oluşturmak `DataBound` olay işleyicisi


Olay işleyici FormView's cast `DataItem` özelliğine bir `ProductsRow` örneği ve belirlemek olup olmadığını `UnitsInPrice` değerdir sağlayacak şekilde kırmızı bir yazı tipiyle görüntüle gerekir.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>6. adım: FormView'ın ItemTemplate UnitsInStockLabel etiket denetiminde biçimlendirme

Görüntülenen biçimlendirmek için son adımdır `UnitsInStock` değer 10 veya daha az ise kırmızı bir yazı tipinde değeri. Bunu programlı olarak erişmek için ihtiyacımız gerçekleştirmek için `UnitsInStockLabel` denetim `ItemTemplate` ve böylece kırmızı olarak görüntülenen kendi metin stili özelliklerini ayarlayın. Bir şablon Web denetiminde erişmek için `FindControl("controlID")` yöntemi şuna benzer:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

Bir etiket erişim istediğimizi Bizim örneğimizde, Denetim `ID` değer `UnitsInStockLabel`, biz kullanırsınız:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Programlama aracılığıyla Web denetimi yapılan başvuruyu sahibiz sonra biz stili ilgili özelliklerini gerektiği gibi değiştirebilirsiniz. Önceki örnekle t bir CSS sınıfı oluşturmuş olduğunuz gibi `Styles.css` adlı `LowUnitsInStockEmphasis`. Bu stili etiket Web denetimi uygulamak üzere ayarla kendi `CssClass` özelliği uygun şekilde.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> Program aracılığıyla Web denetimi kullanarak erişen bir şablon biçimlendirme sözdizimi `FindControl("controlID")` ve stille ilgili özelliklerini ayarlayarak da kullanılabilir kullanırken [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView ya da GridView denetler. Biz TemplateFields bizim sonraki öğreticide inceleyeceğiz.


Şekil 7, bir ürün görüntülerken FormView gösterir, `UnitsInStock` Şekil 8'de ürünün değerini 10'dan sahipken değer 10'dan büyük.


[![Ürünleri ile bir yeterince büyük stoktaki için özel biçimlendirme yok uygulanır](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Şekil 7**: için ürünleri ile bir yeterince büyük stoktaki, özel biçimlendirme yok uygulanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image17.png))


[![Hisse senedi numarası birimlerinde bu ürünleri ile değerleri için 10 veya daha az kırmızı olarak gösterilir](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Şekil 8**: birimleri hisse senedi sayısı bu ürünleri ile değerleri için 10 veya daha az kırmızı olarak gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>GridView ile 's biçimlendirme`RowDataBound`olayı

Daha önce şu adımları DetailsView dizisi incelenmesi ve FormView veri bağlama sırasında ilerleme durumu denetimleri. Üzerinde aşağıdaki adımları yine Yenileyici bakalım.

1. Verinin Web denetimi `DataBinding` olay etkinleşir.
2. Verileri veri Web denetimi bağlıdır.
3. Verinin Web denetimi `DataBound` olay etkinleşir.

Yalnızca tek bir kaydını görüntülemek için bu üç basit adım DetailsView ve FormView için yeterlidir. GridView, görüntüleyen *tüm* bağlı kayıtları kendisine (yalnızca ilk), 2. adım biraz daha karmaşıktır.

GridView veri kaynağı numaralandırır ve her bir kayıt oluşturur 2. adımda bir `GridViewRow` örneği ve geçerli kayıt kendisine bağlar. Her `GridViewRow` GridView eklenen, iki olaylar oluşturulur:

- **`RowCreated`**sonra ateşlenir `GridViewRow` oluşturuldu
- **`RowDataBound`**Geçerli kayıt için bağlı sonra ateşlenir `GridViewRow`.

GridView, daha sonra veri bağlama daha doğru bir şekilde aşağıdaki adımlar dizisini açıklanmıştır:

1. GridView's `DataBinding` olay etkinleşir.
2. Veri GridView bağlıdır.   
  
 Veri kaynağındaki her kayıt için 

    1. Oluşturma bir `GridViewRow` nesnesi
    2. Yangın `RowCreated` olayı
    3. Kayda bağlama`GridViewRow`
    4. Yangın `RowDataBound` olayı
    5. Ekleme `GridViewRow` için `Rows` koleksiyonu
3. GridView's `DataBound` olay etkinleşir.

GridView'ın tek tek kayıtları biçimini özelleştirmek için daha sonra bir olay işleyicisi oluşturmak ihtiyacımız `RowDataBound` olay. Bunu göstermek için bir GridView ekleyelim `CustomColors.aspx` ad, kategori ve bu ürünler, fiyatıdır değerinden $10,00 sarı arka plan rengiyle vurgulama, her ürün için fiyat listeleri sayfası.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>7. adım: Bir GridView ürün bilgilerini görüntüleme

FormView altındaki GridView önceki örnekten ekleyip ayarlayın, `ID` özelliğine `HighlightCheapProducts`. Biz sayfasında tüm ürünleri verir bir ObjectDataSource zaten sahip olduğundan, GridView olarak bağlayın. Son olarak, GridView'ın BoundFields yalnızca ürünlerin adlarını, kategoriler ve fiyatlarını içerecek şekilde düzenleyin. Sonra bu düzenlemeler GridView'ın biçimlendirme gibi görünmelidir:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

Şekil 9 bir tarayıcıdan görüntülendiğinde bu noktaya bizim ilerleme durumunu gösterir.


[![GridView ad, kategori ve her ürün için fiyat listeleri](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Şekil 9**: GridView ad, kategori ve her ürün için fiyat listeleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>8. adım: Program aracılığıyla RowDataBound olay işleyicisi verilerde değerini belirleme

Zaman `ProductsDataTable` GridView bağlı kendi `ProductsRow` örnekleri numaralandırılmış ve her biri için `ProductsRow` bir `GridViewRow` oluşturulur. `GridViewRow`'S `DataItem` özelliği, belirli atandığında `ProductRow`, sonrasında GridView's `RowDataBound` olay işleyicisi oluşturulur. Belirlemek için `UnitPrice` her ürün için değer bağlı GridView, böylece olay işleyicisi GridView için 's oluşturmak için ihtiyacımız `RowDataBound` olay. Bu olay işleyicisi biz inceleyebilirsiniz `UnitPrice` geçerli değeri `GridViewRow` ve biçimlendirme kararı ilgili satır.

Bu olay işleyicisi FormView ve DetailsView adımlardan aynı serisi kullanılarak oluşturulabilir.


![GridView's RowDataBound olayı için bir olay işleyicisi oluşturun](custom-formatting-based-upon-data-cs/_static/image24.png)

**Şekil 10**: olay işleyicisi GridView için 's oluşturmak `RowDataBound` olayı


Bu şekilde olay işleyicisi oluşturma, ASP.NET sayfa kod bölümüne otomatik olarak eklenmesi için aşağıdaki kodu neden olur:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Zaman `RowDataBound` olay ateşlenir olay işleyicisine geçirilir, ikinci parametre olarak türünde bir nesne `GridViewRowEventArgs`, adında bir özellik olan `Row`. Bu özellik için bir başvuru döndürür `GridViewRow` yalnızca veriye oluştu. Erişim için `ProductsRow` örneği bağlı `GridViewRow` kullanırız `DataItem` özellik sözlüğüdür:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

İle çalışırken `RowDataBound` GridView satırları farklı türdeki oluşur ve bu olay için ateşlenir göz önünde bulundurmanız önemlidir olay işleyicisi *tüm* satır türleri. A `GridViewRow`'s türü tarafından belirlenebilir kendi `RowType` özelliği ve olası değerlerden biri olabilir:

- `DataRow`bir kayda GridView bilgisayarın bağlı olduğu bir satır`DataSource`
- `EmptyDataRow`görüntülenen satır GridView's `DataSource` boş
- `Footer`altbilgi satırı; gösterilen IF GridView's `ShowFooter` özelliği ayarlanmış`true`
- `Header`Üstbilgi satırındaki; GridView'ın ShowHeader özelliği ayarlanmışsa gösterilen `true` (varsayılan)
- `Pager`GridView için kullanıcının, disk belleği, disk belleği arabirimini görüntüler satır uygulayın.
- `Separator`GridView için kullanılmaz, ancak tarafından kullanılan `RowType` DataList ve yineleyici özelliklerini denetimleri, iki veri aşağıdakiler ele gelecekte öğreticileri Web denetimleri

Bu yana `EmptyDataRow`, `Header`, `Footer`, ve `Pager` satırları ile ilişkili olmayan bir `DataSource` kayıt, bunlar her zaman olacaktır bir `null` değerini kendi `DataItem` özelliği. Geçerli çalışma denemeden önce bu nedenle, `GridViewRow`'s `DataItem` özelliği, biz ilk biz ile ilgilenen olduğundan emin olmalısınız bir `DataRow`. Bu denetleyerek gerçekleştirilebilir `GridViewRow`'s `RowType` özellik sözlüğüdür:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>9. adım: satır sarı olduğunda UnitPrice değerinin vurgulama değerinden $10,00

Program aracılığıyla tüm vurgulamak için son adımdır `GridViewRow` varsa `UnitPrice` satır değerinden $10,00 değeri. GridView'ın satır veya hücre erişmek için söz dizimi gibi DetailsView ile aynıdır `GridViewID.Rows[index]` tüm satırı erişmek için `GridViewID.Rows[index].Cells[index]` belirli bir hücre erişmek için. Ancak, ne zaman `RowDataBound` olay işleyicisi harekete veri bağlı `GridViewRow` henüz GridView'in eklenmesi gerekiyor `Rows` koleksiyonu. Geçerli etkinleştirilemez `GridViewRow` gelen örnek `RowDataBound` satır koleksiyonunu kullanarak olay işleyicisi.

Yerine `GridViewID.Rows[index]`, geçerli başvurusu yapabilir `GridViewRow` örneğini `RowDataBound` olay işleyicisi kullanarak `e.Row`. Geçerli vurgulamak için diğer bir deyişle, giriş sırası `GridViewRow` gelen örnek `RowDataBound` olay işleyicisi kullanın:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

Yerine ayarlamak `GridViewRow`'s `BackColor` özelliği doğrudan şimdi CSS sınıfları kullanarak da bağlı kalın. Adlı bir CSS sınıfı oluşturmuş olduğunuz `AffordablePriceEmphasis` sarı arka plan rengini ayarlar. Tamamlanan `RowDataBound` olay işleyicisi izler:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![En uygun maliyetli ürünleri vurgulanmış sarı](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Şekil 11**: en uygun maliyetli ürünleri olan vurgulanmış sarı ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>Özet

Bu öğreticide GridView, DetailsView ve denetime bağlı verileri temel alan FormView biçimlendirme gördük. Bu olay işleyicisi için oluşturduğumuz gerçekleştirmek için `DataBound` veya `RowDataBound` olaylar, burada temel alınan veri incelenmesi bir biçimlendirme değişikliği birlikte gerekiyorsa. DetailsView veya FormView bağlı verilere erişmek için kullanırız `DataItem` özelliğinde `DataBound` olay işleyicisi için bir GridView; her `GridViewRow` örneğinin `DataItem` özelliği kullanılabilir,satırbağlıverileriiçerir`RowDataBound` olay işleyicisi.

Program aracılığıyla veri Web denetimin biçimlendirme ayarlama sözdizimi Web denetimi ve verilerin biçimlendirilmesi için nasıl görüntüleneceğini bağlıdır. DetailsView ve GridView denetimleri, satır ve hücre bir sıra dizini tarafından erişilebilir. Şablonlar kullanır, FormView için `FindControl("controlID")` yöntemi şablonu içindeki bir Web denetimi bulmak için yaygın olarak kullanılır.

Sonraki öğreticide GridView ve DetailsView şablonları kullanma inceleyeceğiz. Ayrıca, temel alınan verileri temel alan biçimlendirme özelleştirmek için başka bir teknik göreceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için E.R. gözden geçirenler sağlama Gilmore, Dennis Patterson ve Dan Jagers. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](using-templatefields-in-the-gridview-control-cs.md)
