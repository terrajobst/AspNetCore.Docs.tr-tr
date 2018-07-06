---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: DataList ve Repeater biçimlendirme (VB) verilerine dayalı | Microsoft Docs
author: rick-anderson
description: Bu öğreticide biz size DataList ve Repeater denetimleri ile biçimlendirme işlevleri kullanılarak görünümünü biçimini örnekleri adım...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 428438b2bae062c09d13c002f4729c3c394975a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807716"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>DataList ve Repeater (VB) verileri temel alan biçimlendirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) veya [PDF olarak indirin](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> Bu öğreticide biz size DataList ve Repeater denetimleri, şablonlar içindeki biçimlendirme işlevleri veya DataBound olayı işleme görünümünü biçimini örnekleri adım.


## <a name="introduction"></a>Giriş

Önceki öğreticide gördüğümüz gibi DataList görünümünü etkileyen stil özellikleri sunar. Özellikle, varsayılan CSS sınıfları DataList s atama gördüğümüz `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, ve `SelectedItemStyle` özellikleri. Bu dört özelliklerine ek olarak, DataList diğer stil özellikleri gibi içerir `Font`, `ForeColor`, `BackColor`, ve `BorderWidth`, birkaçıdır. Repeater denetimiyle tüm stil özellikleri içermiyor. Bu tür bir stil ayarlarını doğrudan Repeater s şablonlarındaki biçimlendirme içinde yapılması gerekir.

Genellikle, verilerin nasıl biçimlendirileceğini veri bağlıdır. Örneğin, ürün listelerken biz ürün bilgilerini bir açık gri yazı tipi rengini kullanımdan kaldırılmıştır veya vurgulamak isteyebilirsiniz görüntülemek isteyebilirsiniz `UnitsInStock` sıfır ise, değer. Önceki öğreticilerde gördüğümüz gibi GridView DetailsView ve FormView kendi verilerini temel alarak görünümlerini biçimlendirmek için iki farklı yol sunar:

- **`DataBound` Olay** oluşturmak için uygun bir olay işleyicisi `DataBound` verilerin her öğesine bağlandığı sonra tetiklenen olayı, (Bu GridView için `RowDataBound` olay; DataList ve Repeater içindir.`ItemDataBound`olay). İlgili olay işleyicisi, bağlı veri yalnızca incelenir ve kararları biçimlendirme yapılır. Biz bu teknikte incelenirken [özel biçimlendirme sırasında verileri](../custom-formatting/custom-formatting-based-upon-data-vb.md) öğretici.
- **Biçimlendirme şablonlarındaki işlevleri** DetailsView veya GridView denetimleri ya da bir şablon FormView denetiminde TemplateField kullanma, bir biçimlendirme işlevi ASP.NET sayfalarının arka plan kod sınıfı, iş mantığı katmanı veya tüm ekleyebiliriz web uygulamasından erişilebilir diğer bir sınıf kitaplığı. Bu biçimlendirme işlevi tercihe bağlı sayıda giriş parametrelerini kabul edebilir, ancak şablonda oluşturulacak HTML döndürmelidir. Biçimlendirme işlevleri ilk olarak incelenir [GridView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) öğretici.

DataList ve Repeater denetimleri ile bu teknikler biçimlendirme her ikisi de kullanılabilir. Bu öğreticide size her iki tekniğin için her iki denetim kullanan örnekler adım.

## <a name="using-theitemdataboundevent-handler"></a>Kullanarak`ItemDataBound`olay işleyicisi

Ne zaman veri bağlı bir DataList için verileri kaynak denetiminden veya program aracılığıyla veri s denetimine atama aracılığıyla `DataSource` özelliği ve arama kendi `DataBind()` yöntemi, s DataList `DataBinding` olayı tetikler, numaralandırılan, veri kaynağı ve her veri kaydı için DataList bağlıdır. Veri kaynağındaki her bir kayıt için DataList oluşturur bir [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) daha sonra geçerli kayda bağlı olan nesne. Bu işlem sırasında DataList iki olay meydana getirir:

- **`ItemCreated`** sonra ateşlenir `DataListItem` oluşturuldu
- **`ItemDataBound`** Geçerli kayıt için bağlı sonra ateşlenir. `DataListItem`

Aşağıdaki adımlar, veri bağlama işlemini DataList denetimi için özetlemektedir.

1. DataList s [ `DataBinding` olay](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) etkinleşir
2. DataList için verileri bağlı  
  
   Her kayıt için veri kaynağı 

    1. Oluşturma bir `DataListItem` nesnesi
    2. Ateş [ `ItemCreated` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Kaydına bağlama `DataListItem`
    4. Ateş [ `ItemDataBound` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Ekleme `DataListItem` için `Items` koleksiyonu

Yineleyici denetimine veri bağlama sırasında tam aynı sırası adımlar boyunca ilerler. Tek fark, yerine `DataListItem` Repeater oluşturulan örnek, kullanır [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Kurnaz okuyucu hafif bir anomali GridView verilere bağlı olduğunda DataList ve Repeater verilere karşı bağlandığında niteler adımlar dizisini arasındaki fark etmiş olabilirsiniz. Veri bağlama işleminin tail sonunda, GridView başlatır `DataBound` olay; ancak böyle bir olayın ne DataList veya Repeater denetime sahip. DataList ve Repeater denetimleri öncesi ve sonrası düzeyi olay işleyicisi düzeni ortak pazarlanmasının önünde önce geri ASP.NET 1.x zaman çerçevesinde, oluşturulan olmasıdır.


GridView ile bir olay işleyicisi oluşturmak için bir seçenek verileri temel alan biçimlendirme gibi `ItemDataBound` olay. Bu olay işleyicisi yalnızca için bağlı veri incelemek `DataListItem` veya `RepeaterItem` ve denetimin biçimlendirmesini gereken şekilde etkiler.

DataList denetimi için biçimlendirme değişiklikleri tüm öğesi kullanılarak uygulanır için `DataListItem` standart içerme s stil özellikleri `Font`, `ForeColor`, `BackColor`, `CssClass`ve benzeri. DataList s şablonu içindeki belirli Web denetimleri biçimlendirme etkilemek için program aracılığıyla erişmek ve bu Web denetimleri stilini değiştirmek ihtiyacımız var. Bu arka planda gerçekleştirmek nasıl gördüğümüz *özel biçimlendirme sırasında verileri* öğretici. Repeater denetimiyle gibi `RepeaterItem` sınıfın hiç stil özellikleri vardır; bu nedenle, tüm stil yapılan değişiklikler bir `RepeaterItem` içinde `ItemDataBound` olay işleyicisi Bitti, program aracılığıyla erişerek ve Web denetimleri içinde güncelleştiriliyor Şablon.

Bu yana `ItemDataBound` DataList ve Repeater'ı örneğimizde odaklanmak DataList kullanarak neredeyse aynı için teknik biçimlendirme.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>1. adım: DataList ürün bilgilerini görüntüleme

Biçimlendirme hakkında endişe önce let s ilk oluşturun ürün bilgilerini görüntülemek için bir DataList kullanan bir sayfa. İçinde [önceki öğreticide](displaying-data-with-the-datalist-and-repeater-controls-vb.md) DataList oluşturduk, `ItemTemplate` görüntülenen her s ürün adı, kategori, tedarikçi, birim ve fiyat başına miktarı. Bu öğreticide bu işlevsellik burada yineleyin s olanak tanır. Bunu gerçekleştirmek için DataList ve sıfırdan kendi ObjectDataSource ya da yeniden oluşturabilirsiniz veya önceki öğreticide oluşturulan sayfasından bu denetimlerin üzerine kopyalayabilirsiniz (`Basics.aspx`) ve bunları Bu öğretici için sayfaya yapıştırın (`Formatting.aspx`).

DataList ve ObjectDataSource işlevinden çoğaltıldıktan sonra `Basics.aspx` içine `Formatting.aspx`, s DataList değiştirmek için birkaç dakikanızı `ID` özelliğinden `DataList1` daha açıklayıcı için `ItemDataBoundFormattingExample`. Ardından, bir tarayıcıda DataList görüntüleyin. Şekil 1 gösterildiği gibi yalnızca biçimlendirme arasındaki her ürün arka plan rengi, diğerleri farktır.


[![DataList denetimi ürünler listelenir](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Şekil 1**: ürünleri DataList denetimi listelenir ([tam boyutlu görüntüyü görmek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Bu öğreticide, s DataList ürünlerden 20,00 değerinden bir fiyatla her iki adı olacaktır ve birim fiyatı vurgulanan sarı şekilde biçimlendirme sağlar.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>2. adım: Program aracılığıyla veri ItemDataBound olay işleyicisinde değerini belirleme

Yalnızca bu ürünlerin 20,00 altında bir fiyat ile özel biçimlendirme uygulanmış olmalıdır, size her ürün s fiyatı belirlemek çalıştırılabilmesi gerekir. DataList için veri bağlama sırasında DataList kendi veri kaynağındaki kayıtları numaralandırır ve her bir kayıt oluşturur bir `DataListItem` örneği, veri kaynağı kaydı için bağlama `DataListItem`. Belirli bir kayıtla sonra s geçerli veri bağlandı `DataListItem` nesne, s DataList `ItemDataBound` olay tetiklenir. Geçerli veri değerlerini incelemek bu olay için bir olay işleyicisi oluşturabiliriz `DataListItem` ve bu değerleri alarak, tüm biçimlendirme gerekli değişiklikleri yapın.

Oluşturma bir `ItemDataBound` DataList olayı ve aşağıdaki kodu ekleyin:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

While kavram ve DataList s ardındaki semantik `ItemDataBound` olay işleyicisi, GridView s tarafından kullanılanlarla aynı `RowDataBound` olay işleyicisinde *özel biçimlendirme sırasında verileri* öğreticide söz diziminin farklı biraz. Zaman `ItemDataBound` olayı tetiklendiğinde, `DataListItem` yalnızca verilere bağlama ile ilgili olay işleyicisine geçirilir `e.Item` (yerine `e.Row`, GridView s olarak `RowDataBound` olay işleyicisi). DataList s `ItemDataBound` olay işleyicisi harekete için *her* üst bilgi satırları ve alt satır ayırıcı satırları dahil olmak üzere DataList için eklenen satır. Bununla birlikte, ürün bilgilerini yalnızca veri satırlarına bağlıdır. Bu nedenle, kullanırken `ItemDataBound` DataList için ilişkili olay verileri incelemek için önce sağlamak ihtiyacımız sahip veri öğesi çalışma re ediyoruz. Bu kontrol ederek gerçekleştirilebilir `DataListItem` s [ `ItemType` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), hangi birine sahip [aşağıdaki sekiz değerleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Her ikisi de `Item` ve `AlternatingItem``DataListItem` s düzenini DataList s veri öğeleri. Varsayılarak biz çalışmaya bir `Item` veya `AlternatingItem`, biz gerçek erişim `ProductsRow` geçerli bağlı değildi örneğe `DataListItem`. `DataListItem` s [ `DataItem` özelliği](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) başvuru içeren `DataRowView` nesnesi `Row` özelliği, gerçek bir başvuru sağlar `ProductsRow` nesne.

Ardından, biz denetleyin `ProductsRow` örneği s `UnitPrice` özelliği. Ürünleri tablo s beri `UnitPrice` alan verir `NULL` erişmeye çalışmadan önce değerleri `UnitPrice` özelliği biz öncelikle kontrol etmelidir olup olmadığını görmek için bir `NULL` kullanarak değer `IsUnitPriceNull()` yöntemi. Varsa `UnitPrice` değeri değil `NULL`, biz sonra onay olmadığını görmek için s 20,00 küçüktür. Bu gerçekten de altında 20,00 ise, ardından özel biçimlendirme uygulamak ihtiyacımız var.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>3. adım: Ürün adı ve fiyat vurgulama

Bir ürün s fiyatı kısa 20,00 olduğunu öğrendikten sonra kalan tek şey, ad ve fiyat vurgulamak için. Bunu yapmak için biz öncelikle programlı olarak etiket denetimleri başvurmalıdır `ItemTemplate` fiyat ve ürün s adını görüntüler. Ardından, sarı bir arka plan görüntülemek ihtiyacımız var. Bu biçimlendirme bilgileri doğrudan etiketleri değiştirerek uygulanabilir `BackColor` özellikleri (`LabelID.BackColor = Color.Yellow`); ideal olarak, görüntü ile ilgili tüm konularda verdiği geçişli stil sayfaları yine de ifade edilmelidir. İstenen tanımlanan biçimlendirme sağlayan bir stil sayfası zaten aslında sahibiz `Styles.css`  -  `AffordablePriceEmphasis`, oluşturulan ve ele *özel biçimlendirme sırasında verileri* öğretici.

Biçimlendirme uygulamak için iki etiket Web denetimi ayarlamanız yeterlidir `CssClass` özelliklerine `AffordablePriceEmphasis`aşağıdaki kodda gösterildiği gibi:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

İle `ItemDataBound` tamamlandı olay işleyicisi, yeniden ziyaret `Formatting.aspx` sayfasını bir tarayıcıda. Şekil 2 gösterildiği gibi bu ürünlerin fiyatı 20,00 altında hem adı hem de vurgulanmış fiyat sahip.


[![Bu ürünler sayısından az 20,00 vurgulanır](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Şekil 2**: Bu ürünleri sayısından az 20,00 vurgulanır ([tam boyutlu görüntüyü görmek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> DataList bir HTML olarak işlenen bu yana `<table>`, kendi `DataListItem` örneğiniz stil özellikleri, öğenin tamamı için belirli bir stil uygulamak için ayarlanabilir. Örneğin vurgulamak istedik, *tüm* öğesi olduğunda kendi fiyat küçüktür 20,00 sarı, biz etiketleri başvurulan kod değiştirilen ve kendi `CssClass` aşağıdaki kod satırını özelliklerle: `e.Item.CssClass = "AffordablePriceEmphasis"` (bkz: Şekil 3).


`RepeaterItem` Repeater denetiminde ' ancak don t hale s gibi stil düzeyi özellikleri sunar. Bu nedenle, Şekil 2'de yaptığımız gibi yineleyici s şablonlar içindeki Web denetimleri stil özellikleri uygulamaya bir yineleyici için özel biçimlendirme uygulanması gerekir.


[![Tüm ürün ürünleri altında için 20,00 vurgulanır](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Şekil 3**: tüm ürün ürünleri altında için 20,00 vurgulanana ([tam boyutlu görüntüyü görmek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Biçimlendirme işlevlerden şablonu içindeki kullanma

İçinde *GridView denetiminde TemplateField kullanma* verileri temel alan özel biçimlendirme uygulamak için biçimlendirme bir işlev içinde GridView TemplateField kullanma gördüğümüz öğretici GridView s satırlara bağlı. Biçimlendirme işlevine bir şablondan çağrılabilir ve onun yerine derleyicisindeki HTML döndüren bir yöntemdir. Biçimlendirme işlevleri ASP.NET sayfalarının arka plan kod sınıfında bulunabilir veya sınıf dosyalarına Merkezi `App_Code` klasör veya ayrı bir sınıf kitaplığı projesi. Biçimlendirme işlevine ASP.NET sayfası s arka plan kod sınıfı dışında taşıma, birden çok ASP.NET sayfaları ya da diğer ASP.NET web uygulamalarını aynı biçimlendirme işlevi kullanmayı planlıyorsanız idealdir.

Let s biçimlendirme işlevleri göstermek için metni [artık ÜRETİLMİYOR] s ürün adının yanında dahil ürün bilgileri sahip, s kullanımdan kaldırıldı. Ayrıca, let s sahip fiyat vurgulanan sarı ise, s 20,00 değerinden (yaptığımız gibi `ItemDataBound` olay işleyicisi örnek); fiyat 20,00 veya daha yüksek, let s görüntüleme gerçek fiyat, ancak bunun yerine metin, lütfen çağırmak için bir fiyat teklifi. Şekil 4, uygulanan bu biçimlendirme kuralları ile listeleme ürünleri ekran görüntüsü gösterilmektedir.


[![Pahalı ürünleri için bir fiyat teklifi için lütfen arama metniyle fiyat değiştirilir](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Şekil 4**: pahalı ürünleri için bir fiyat teklifi için lütfen arama metniyle fiyat değiştirilir ([tam boyutlu görüntüyü görmek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>1. adım: biçimlendirme işlevleri oluşturma

Vurgulanan bir fiyat görüntüler bu örneğin iki biçimlendirme işlevleri, gerekirse ürün adı [artık ÜRETİLMİYOR] metni görüntüleyen ve başka ihtiyacımız, s 20,00 veya başka bir fiyat teklifi için lütfen arama metni'değerinden küçük. Bu işlevler ASP.NET sayfalarının arka plan kod sınıfında oluşturun ve bunları izin `DisplayProductNameAndDiscontinuedStatus` ve `DisplayPrice`. Her iki yöntem bir dize olarak işlemek için HTML dönmeniz gerekir ve her ikisi de olarak işaretlenmiş gerek `Protected` (veya `Public`) ASP.NET sayfası s bildirim temelli söz dizimi kısımlarından çağrılması için. Bu iki yöntem için kod aşağıdaki gibidir:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Unutmayın `DisplayProductNameAndDiscontinuedStatus` yöntemi değerlerini kabul `productName` ve `discontinued` veri alanları skaler değerler ise `DisplayPrice` yöntemi kabul bir `ProductsRow` örneği (yerine `unitPrice` skaler değer). Her iki yöntemle çalışır; Ancak, biçimlendirme işlevi veritabanı içerebilir skaler değerler ile çalışıyorsa `NULL` değerleri (gibi `UnitPrice`; ne `ProductName` ya da `Discontinued` izin `NULL` değerleri), özel dikkat bu işleme alınması gerekir skaler giriş.

Özellikle, giriş parametresinin türünü olmalıdır `Object` gelen değeri olabileceğinden bir `DBNull` örnek beklenen veri türü yerine. Ayrıca, bir onay gelen değer, bir veritabanı olup olmadığını belirlemek için yapılmalıdır `NULL` değeri. Diğer bir deyişle, istedik, `DisplayPrice` fiyat bir skaler değer olarak, d biz kabul edecek şekilde yöntemine sahip aşağıdaki kodu kullanın:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Unutmayın `unitPrice` giriş parametresi, tür `Object` ve koşullu deyim durumunda olmadığından emin olmak için değiştirilen `unitPrice` olan `DBNull` veya değil. Ayrıca, bu yana `unitPrice` giriş parametresi geçirilir olarak bir `Object`, bir ondalık değer için dönüştürmeniz gerekir.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>2. adım: DataList s ItemTemplate biçimlendirme işlevi çağırma

Bizim ASP.NET sayfalarının arka plan kod sınıfı için eklenen biçimlendirme işlevleri ile tüm bu kalır, Bu işlevlerden s DataList biçimlendirme çağrılacak `ItemTemplate`. Bir şablondan biçimlendirme bir işlevi çağırmak için veri bağlama söz dizimi içinde işlev çağrısı yerleştirin:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

S DataList'te `ItemTemplate` `ProductNameLabel` etiket Web denetimi şu anda atayarak s ürün adını görüntüler, `Text` özelliği sonucu, `<%# Eval("ProductName") %>`. Bunun yerine atar sahip olmak için gerekirse adına ve ' % s'metni [artık ÜRETİLMİYOR] görüntülemek, bildirim temelli söz dizimi güncelleştirerek `Text` özellik değeri, `DisplayProductNameAndDiscontinuedStatus` yöntemi. Bunun yapılması, biz s ürün adı ve artık sağlanmayan değerlerini kullanarak geçmelidir `Eval("columnName")` söz dizimi. `Eval` türünde bir değer döndürür `Object`, ancak `DisplayProductNameAndDiscontinuedStatus` yöntemi türü giriş parametreleri bekliyor `String` ve `Boolean`; bu nedenle, biz tarafından döndürülen değerleri atamalısınız `Eval` beklenen giriş parametre türleri, yöntem şu şekilde:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Fiyat görüntülemek için biz ayarlayabilirsiniz `UnitPriceLabel` etiket s `Text` özelliği tarafından döndürülen değere `DisplayPrice` sadece biz s ürün adını görüntülemek için yaptığınız ve [metin KULLANIMDAN] gibi yöntemi. Bununla birlikte, içinde geçirmek yerine `UnitPrice` skaler giriş parametresi olarak, biz bunun yerine tüm geçirin `ProductsRow` örneği:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Yerinde biçimlendirme işlevlere çağrılarını ilerlememizin bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Ekranınız metni [artık ÜRETİLMİYOR] dahil olmak üzere artık üretilmeyen ürünler ile Şekil 5'e benzer görünmelidir ve birden fazla olması, fiyat 20,00 maliyeti bu ürünlerin metinle çağrısı fiyat teklifi için lütfen değiştirilir.


[![Pahalı ürünleri için bir fiyat teklifi için lütfen arama metniyle fiyat değiştirilir](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Şekil 5**: pahalı ürünleri için bir fiyat teklifi için lütfen arama metniyle fiyat değiştirilir ([tam boyutlu görüntüyü görmek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Özet

Verileri temel alan bir DataList veya Repeater denetimin içeriğini biçimlendirme, iki tekniklerini kullanılarak gerçekleştirilebilir. Bir olay işleyicisi oluşturmak için ilk tekniktir `ItemDataBound` veri kaynağındaki her kayıt yeni bir bağlı olarak, harekete olay `DataListItem` veya `RepeaterItem`. İçinde `ItemDataBound` olay işleyicisi geçerli s öğesi verileri incelenebilir ve ardından biçimlendirme uygulanabilir şablonunun veya için içeriğini `DataListItem` tüm öğe, s.

Alternatif olarak, özel biçimlendirme biçimlendirme işlevleri aracılığıyla alırlar. Biçimlendirme işlevine DataList çağrılabilen bir yöntem veya onun yerine yaymak için HTML döndürür Repeater s Şablonları ' dir. Genellikle, bir biçimlendirme işlevi tarafından döndürülen HTML geçerli öğeye bağlanan değerlere göre belirlenir. Bu değerler biçimlendirme işleve skaler değerler ya da öğeye bağlanan tüm nesnesi geçirerek geçirilebilir (gibi `ProductsRow` örnek).

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Yaakov Ellis ve Randy Etikan Liz Shulok yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [İleri](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
