---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: "Yineleyici ve DataList biçimlendirme (VB) verilerine dayalı | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide biz biz DataList ve yineleyici denetimlerin görünümünü, biçimlendirme işlevleriyle kullanarak ya da biçimini örnekleri adım..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 48e0f2bad8c048e943ec2a3ce72cc0f7ca4d34d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Yineleyici (VB) verilerine göre ve DataList biçimlendirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) veya [PDF indirin](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> Bu öğreticide biz biz DataList ve yineleyici denetimlerin görünümünü, şablonlar içindeki biçimlendirme işlevleri kullanarak veya veriye bağlı olay işleme biçimini örnekleri adım.


## <a name="introduction"></a>Giriş

Önceki öğreticide gördüğümüz gibi DataList görünümünü etkileyen stili ile ilgili özellikler sunar. Özellikle, varsayılan CSS sınıfları s DataList atama gördüğümüz `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, ve `SelectedItemStyle` özellikleri. Dört bu özelliğe ek olarak, DataList diğer stil ilgili özellikler gibi içerir `Font`, `ForeColor`, `BackColor`, ve `BorderWidth`, birkaçıdır. Yineleyici denetim stili ile ilgili herhangi bir özellik içermiyor. Bu tür bir stil ayarlarını doğrudan yineleyici s şablonlarındaki biçimlendirmesi içinde yapılması gerekir.

Genellikle, yine de verilerin nasıl biçimlendirilmiş veriler üzerinde bağlıdır. Örneğin, ürünleri listelerken biz ürün bilgilerini bir açık gri yazı tipi rengi, kesilir veya vurgulamak isteyebilirsiniz görüntülemek isteyebilirsiniz `UnitsInStock` sıfır ise, değer. GridView, DetailsView ve FormView önceki eğitimlerine gördüğümüz gibi kendi verilerine dayalı görünümlerini biçimlendirmek için iki farklı yol sunar:

- **`DataBound` Olay** için uygun bir olay işleyicisi oluşturun `DataBound` verileri her öğeye bağlı sonra ateşlenir olayı (olduğu GridView için `RowDataBound` olay; DataList ve yineleyici içindir`ItemDataBound`olay). Bu olay işleyicisi, veri yalnızca bağlı incelenebilir ve kararları biçimlendirme yapılır. Biz bu teknik incelenmesi [özel biçimlendirme göre bağlı verileri](../custom-formatting/custom-formatting-based-upon-data-vb.md) Öğreticisi.
- **Şablonları işlevlerde biçimlendirme** TemplateFields DetailsView veya GridView denetimleri ya da FormView denetimindeki bir şablon kullanıldığında, bir biçimlendirme işlevi ASP.NET sayfası s arka plandaki kod sınıfı, iş mantığı katmanı veya herhangi bir ekleyebiliriz web uygulamasından erişilebilir diğer sınıf kitaplığı. Bu biçimlendirme işlevi rasgele bir girdi parametresi sayısı kabul edebilir, ancak şablonda oluşturulacak HTML döndürmesi gerekir. Biçimlendirme işlevleri önce incelenmesi [kullanarak TemplateFields GridView denetiminde](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Öğreticisi.

DataList ve yineleyici denetimleriyle teknikleri biçimlendirme bunların her ikisi de kullanılabilir. Bu öğreticide şu iki denetimleri için her iki tekniği kullanan örnekler adım.

## <a name="using-theitemdataboundevent-handler"></a>Kullanarak`ItemDataBound`olay işleyicisi

Ne zaman veri bağlı bir DataList için bir veri kaynağı denetimi veya program aracılığıyla veri s denetimine atama yoluyla `DataSource` özelliği ve arama kendi `DataBind()` yöntemi, s DataList `DataBinding` olay gönderir, numaralandırılmış, veri kaynağı ve her veri kaydı DataList bağlıdır. Veri kaynağındaki her kayıt için DataList oluşturur bir [ `DataListItem` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalistitem.aspx) diğer bir deyişle nesne sonra geçerli kayda bağlı. Bu işlem sırasında DataList iki olaylar oluşur:

- **`ItemCreated`**sonra ateşlenir `DataListItem` oluşturuldu
- **`ItemDataBound`**Geçerli kayıt için bağlı sonra ateşlenir`DataListItem`

Veri bağlama işlemini DataList denetimi için aşağıdaki adımları verilmiştir.

1. S DataList [ `DataBinding` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.control.databinding.aspx) etkinleşir
2. DataList veri bağlama  
  
 Veri kaynağındaki her kayıt için 

    1. Oluşturma bir `DataListItem` nesnesi
    2. Yangın [ `ItemCreated` olayı](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Kayda bağlama`DataListItem`
    4. Yangın [ `ItemDataBound` olayı](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Ekleme `DataListItem` için `Items` koleksiyonu

Yineleyici denetimine veri bağlama sırasında tam aynı adımlar dizisini ilerler. Tek fark, yerine olan `DataListItem` oluşturulan örnekleri, yineleyici kullanan [ `RepeaterItem` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Akıllı duruma okuyucu hafif bir anomali GridView veriye bağlı olduğunda DataList ve yineleyici verilerine karşı bağlı olduğunda, gerçekleşen adımların sırasını arasındaki fark etmiş olabilirsiniz. Veri bağlama işleminin tail sonunda GridView başlatır `DataBound` olay; ancak böyle bir olay DataList ne yineleyici denetime sahip. Öncesi ve sonrası düzeyi olay işleyicisi düzeni yaygın hale önce DataList ve yineleyici denetimleri geri ASP.NET 1.x zaman çerçevesinde, oluşturulan olmasıdır.


GridView ile verileri temel alan biçimlendirme için bir seçenek için bir olay işleyicisi oluşturmaktır gibi `ItemDataBound` olay. Bu olay işleyicisi yalnızca için bağlı verileri incelemek `DataListItem` veya `RepeaterItem` ve denetimi gerektiği gibi biçimlendirme etkiler.

Kullanarak öğenin tamamı uygulanabilir için DataList denetimi için biçimlendirme değişiklikleri `DataListItem` standart dahil stili ilgili özelliklerinde, `Font`, `ForeColor`, `BackColor`, `CssClass`ve benzeri. Belirli Web denetimleri DataList s şablonu içindeki biçimlendirme etkilemek için programlı olarak erişmek ve bu Web denetimleri stilini değiştirmek ihtiyacımız. Bu geri yüklemenin nasıl yapılacağını gördüğümüz *özel biçimlendirme göre bağlı verileri* Öğreticisi. Yineleyici denetim gibi `RepeaterItem` sınıfı hiçbir stil ilgili özellikler vardır; bu nedenle, tüm stil ilgili yapılan değişiklikler bir `RepeaterItem` içinde `ItemDataBound` olay işleyicisi program aracılığıyla erişme ve Web denetimleri içinde güncelleştirme tarafından yapılması gerekir Şablon.

Bu yana `ItemDataBound` DataList ve yineleyici Bizim örneğimizde, DataList kullanarak olmadığını odaklanmak neredeyse aynı için teknik biçimlendirme.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>1. adım: DataList ürün bilgilerini görüntüleme

Biçimlendirme hakkında endişelenmeniz önce let s ilk oluşturmanız ürün bilgilerini görüntülemek için bir DataList kullanan bir sayfa. İçinde [önceki öğretici](displaying-data-with-the-datalist-and-repeater-controls-vb.md) DataList oluşturduğumuz, `ItemTemplate` görüntülenen her s ürün adı, kategori, tedarikçi, birim ve fiyat başına miktarı. Burada bu işlev Bu öğreticide yineleyin s olanak tanır. Bunu gerçekleştirmek için DataList ve alt ObjectDataSource sıfırdan ya da yeniden veya önceki öğreticide oluşturduğunuz sayfasından bu denetimleri kopyalayabilirsiniz (`Basics.aspx`) ve bunları Bu öğretici için sayfaya yapıştırın (`Formatting.aspx`).

DataList ve ObjectDataSource işlevinden çoğaltıldıktan sonra `Basics.aspx` içine `Formatting.aspx`, s DataList değiştirmek için bir dakikanızı ayırın `ID` özelliğinden `DataList1` daha açıklayıcı için `ItemDataBoundFormattingExample`. Ardından, bir tarayıcıda DataList görüntüleyin. Şekil 1'de görüldüğü gibi yalnızca biçimlendirme arasındaki her ürünün arka plan rengini diğerleri farktır.


[![Ürünler DataList denetiminde listelenir](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Şekil 1**: ürünleri DataList denetiminde listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Bu öğretici için DataList ürünlerden $20,00'den bir fiyat ile hem de kendi adına sahip olacaktır ve birim fiyat vurgulanan sarı gibi biçimlendirin s olanak tanır.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>2. adım: Program aracılığıyla ItemDataBound olay işleyicisi verilerde değerini belirleme

Uygulanan özel biçimlendirme yalnızca bir fiyatla $20,00 altında ürünleri sahip olduğundan, biz her ürün s fiyat belirlemek mümkün olması gerekir. Bir DataList veri bağlama sırasında DataList kendi veri kaynağı kayıtları numaralandırır ve her bir kayıt oluşturur bir `DataListItem` örneği, veri kaynağı kaydı bağlama `DataListItem`. Belirli bir kayıtla sonra s geçerli veri bağlandı `DataListItem` nesnesi, s DataList `ItemDataBound` olay tetiklenir. Geçerli veri değerlerini incelemek bu olay için bir olay işleyicisi oluşturabiliriz `DataListItem` ve bu değerleri alarak, tüm biçimlendirme gerekli değişiklikleri yapın.

Oluşturma bir `ItemDataBound` DataList olayı ve aşağıdaki kodu ekleyin:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

While s DataList ardındaki semantik ve kavram `ItemDataBound` olay işleyicisi GridView s tarafından kullanılanlarla aynı olan `RowDataBound` olay işleyicisini *özel biçimlendirme göre bağlı verileri* öğretici, sözdizimi farklıdır biraz. Zaman `ItemDataBound` olay ateşlenir `DataListItem` yalnızca verilere bağlama ile ilgili olay işleyicisine geçirilir `e.Item` (yerine `e.Row`, GridView s olarak `RowDataBound` olay işleyicisi). S DataList `ItemDataBound` olay işleyicisi harekete *her* üstbilgi satırlarını, altbilgi satırları ve satır ayırıcı dahil DataList için eklenen satır. Ancak, ürün bilgilerini yalnızca veri satırlarına bağlıdır. Bu nedenle, kullanırken `ItemDataBound` verileri incelemek için olay DataList bağımlı, ilk emin olmak ihtiyacımız biz sahip veri öğesi çalışma re. Bu denetleyerek gerçekleştirilebilir `DataListItem` s [ `ItemType` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), hangi aşağıdakilerden biri olabilir [aşağıdaki sekiz değerleri](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Her ikisi de `Item` ve `AlternatingItem``DataListItem` s oluşma şekli DataList s veri öğeleri. Varsayılarak biz çalışmaya bir `Item` veya `AlternatingItem`, biz gerçek erişim `ProductsRow` geçerli bağlıydı örneği `DataListItem`. `DataListItem` s [ `DataItem` özelliği](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.datalistitem.dataitem.aspx) başvuruyor `DataRowView` nesnesi, özelliği `Row` özelliği, gerçek bir başvuru sağlar `ProductsRow` nesnesi.

Ardından, biz denetleyin `ProductsRow` örneği s `UnitPrice` özelliği. S Ürünler tablosunun itibaren `UnitPrice` alan verir `NULL` erişmeye çalışmadan önce değerleri, `UnitPrice` özelliği biz öncelikle kontrol etmelidir olup olmadığını görmek için bir `NULL` kullanarak değer `IsUnitPriceNull()` yöntemi. Varsa `UnitPrice` değeri değil `NULL`, biz sonra onay olmadığını görmek için bu s $20,00 küçüktür. $20,00 gerçekten altında etkinleştirilmişse, ardından özel biçimlendirme uygulamak ihtiyacımız.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>3. adım: Ürün adı ve fiyat vurgulama

Bir ürün s fiyat değerinden $20,00 olduğunu biliyoruz sonra kalan tek şey adını ve fiyat vurgulayın. Bunu başarmak için biz ilk program aracılığıyla etiket denetimlerinde başvurmalıdır `ItemTemplate` fiyat ve ürün s adını görüntüler. Ardından, bunları sarı bir arka plan görüntülemek ihtiyacımız. Bu biçimlendirme bilgilerini doğrudan etiketleri değiştirerek uygulanabilir `BackColor` özellikleri (`LabelID.BackColor = Color.Yellow`); ideal olarak, tüm ilgili görüntüleme önemlidir geçişli stil sayfaları yine de ifade edilmelidir. Aslında, biz istenen tanımlanan biçimlendirme sağlayan bir stil zaten `Styles.css`  -  `AffordablePriceEmphasis`, oluşturulan ve ele *özel biçimlendirme göre bağlı verileri* Öğreticisi.

Biçimlendirme uygulamak üzere yalnızca iki etiket Web denetimleri Ayarla `CssClass` özelliklerine `AffordablePriceEmphasis`aşağıdaki kodda gösterildiği gibi:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

İle `ItemDataBound` olay işleyicisi tamamlandı, yeniden ziyaret `Formatting.aspx` sayfasını bir tarayıcıda. Şekil 2 gösterildiği gibi bir fiyat $20,00 altında bu ürünlerle hem adı hem de vurgulanan fiyat sahip.


[![Bu ürünler sayısından az $20,00 vurgulanır](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Şekil 2**: Bu ürünleri sayısından az $20,00 vurgulanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> DataList bir HTML olarak işlenen beri `<table>`, kendi `DataListItem` örnekleri belirli bir stil tüm öğeye uygulamak üzere ayarlanabilir stili ilgili özellikleri vardır. Örneğin, vurgulamak istediyseniz *tüm* öğesi kendi fiyat değerinden $20,00 olduğunda sarı, biz etiketleri başvurulan kod değiştirilen ve ayarlayın kendi `CssClass` aşağıdaki kod satırını özelliklerle: `e.Item.CssClass = "AffordablePriceEmphasis"` (bkz. Şekil 3).


`RepeaterItem` Yineleyici denetimi oluşturan ancak tan t olun s gibi stili düzeyi özellikleri sunar. Bu nedenle, Şekil 2'de komutlarında yaptığımız gibi stil özelliklerini uygulama yineleyici s şablonlar içindeki Web denetimleri için bir özel yineleyici biçimlendirme uygulama gerektirir.


[![Tüm ürün ürünleri altında için $20,00 vurgulanmış](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Şekil 3**: tüm ürün vurgulanmış ürünleri altında için $20,00 ([tam boyutlu görüntüyü görüntülemek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Şablon biçimlendirme işlevler kullanma

İçinde *kullanarak TemplateFields GridView denetiminde* GridView TemplateField biçimlendirme işlevindeki verilerine göre özel biçimlendirme uygulamak için nasıl kullanılacağını gördüğümüz öğretici GridView s satırlarına bağlı. Biçimlendirme işlevi bir şablondan çağrılabilir ve onun yerine yayınlaması için HTML döndüren bir yöntemdir. Biçimlendirme işlevleri ASP.NET sayfası s arka plan kodu sınıfında bulunabilir veya sınıf dosyalarına Merkezi `App_Code` klasör veya ayrı bir sınıf kitaplığı proje. Birden çok ASP.NET sayfaları veya diğer ASP.NET web uygulamalarını aynı biçimlendirme işlevi kullanmayı planlıyorsanız, ASP.NET sayfası s arka plandaki kod sınıfı dışında biçimlendirme işlevi taşıma idealdir.

Let s biçimlendirme işlevleri tanıtmak için metni [üretimi DURDURULMUŞ] ve ürün s adının yanındaki varsa içeren ürün bilgilerini sahip onu s devam etmez. Ayrıca, let s sahip fiyat vurgulanan sarı ise, s $20,00'den (biz yaptığınız gibi `ItemDataBound` olay işleyicisi örnek); $20,00 fiyatıdır veya daha yüksek, let s gerçek fiyat görüntüleme, ancak bunun yerine metin, lütfen çağırmak için bir fiyat teklifi. Şekil 4 uygulanan kurallar biçimlendirme ile listeleme ürünleri ekran görüntüsü gösterilmektedir.


[![Pahalı ürünler için fiyat Lütfen çağrısı fiyat teklifi için bir metin ile değiştirilir](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Şekil 4**: pahalı ürünler için lütfen çağrısı fiyat teklifi için bir metin ile fiyat değiştirilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>1. adım: biçimlendirme işlevleri oluşturma

Vurgulanan bir fiyat görüntüler bu örneğin iki biçimlendirme işlevleri, gerekirse metni [üretimi DURDURULMUŞ] yanı sıra ürün adı görüntüleyen, diğeri ihtiyacımız onu s $20,00 ya da metin, aksi takdirde bir fiyat teklifi için lütfen çağırın. Bu işlevler ASP.NET sayfası s arka plandaki kod sınıfı oluşturmak ve bunları s izin `DisplayProductNameAndDiscontinuedStatus` ve `DisplayPrice`. Bir dize olarak işlemek için HTML döndürmek iki yöntem gerekir ve her ikisi de işaretlenmesi gerek `Protected` (veya `Public`) ASP.NET sayfası s Tanımlayıcı Sözdizimi kısımlarından çağrılması için. Bu iki yöntem için kod aşağıdaki gibidir:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Unutmayın `DisplayProductNameAndDiscontinuedStatus` yöntemi değerlerini kabul `productName` ve `discontinued` veri alanları skaler değerler ancak `DisplayPrice` yöntemi kabul bir `ProductsRow` örneği (yerine bir `unitPrice` skaler değer). Her iki yaklaşım çalışır; Ancak, biçimlendirme işlevi veritabanı içeren bir skaler değerler ile çalışıp çalışmadığını `NULL` değerleri (gibi `UnitPrice`; hiçbiri `ProductName` ya da `Discontinued` izin `NULL` değerleri), özellikle dikkat bu işleme alınması gerekir skaler girişleri.

Giriş parametresi türü özellikle olmalıdır `Object` gelen değeri olabileceğinden bir `DBNull` beklenen veri türü yerine örneği. Ayrıca, bir onay gelen değeri bir veritabanı olup olmadığını belirlemek için yapılması gerekir `NULL` değeri. Diğer bir deyişle, biz istediyseniz `DisplayPrice` fiyat bir skaler değer olarak, biz d kabul etmek için bir yöntemi olması aşağıdaki kodu kullanmak:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Unutmayın `unitPrice` giriş parametresidir türü `Object` ve koşul deyimi, onaylaması için değiştirilmiş `unitPrice` olan `DBNull` veya değil. Ayrıca, bu yana `unitPrice` giriş parametresi geçirilir olarak bir `Object`, ondalık bir değeri dönüştürmeniz gerekir.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>2. adım: DataList s ItemTemplate biçimlendirme işlevi çağırma

Bizim ASP.NET sayfası s arka plandaki kod sınıfı eklenen biçimlendirme işlevleriyle kalan tek şey bu s DataList işlevlerden biçimlendirme çağrılacak `ItemTemplate`. Bir şablondan biçimlendirme bir işlevi çağırmak için databinding söz dizimi içinde işlev çağrısı koyun:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

S DataList içinde `ItemTemplate` `ProductNameLabel` etiket Web denetimi şu anda atayarak ürün s adını görüntüler, `Text` özelliği sonucu, `<%# Eval("ProductName") %>`. Bunun yerine atar olması için gerektiğinde adına ve [üretimi DURDURULMUŞ] metni görüntülemek Tanımlayıcı Sözdizimi güncelleştirerek `Text` özellik değeri, `DisplayProductNameAndDiscontinuedStatus` yöntemi. Bunun yapılması, biz s ürün adı ve devam etmeyen değerlerini kullanarak geçmelidir `Eval("columnName")` sözdizimi. `Eval`türünde bir değer döndürür `Object`, ancak `DisplayProductNameAndDiscontinuedStatus` yöntemi türü giriş parametreleri bekliyor `String` ve `Boolean`; bu nedenle, tarafından döndürülen değer dönüştürmeniz gerekir `Eval` yöntemine beklenen giriş parametre türleri şu şekilde:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Fiyat görüntülemek için biz yalnızca ayarlayabilirsiniz `UnitPriceLabel` etiket s `Text` tarafından döndürülen değer özelliğine `DisplayPrice` yalnızca biz ürün s adını görüntülemek için yaptığınız ve [metin DÖNÜŞTÜRÜLMESİNE] gibi yöntemi. Ancak, bilgilerinde yerine `UnitPrice` skaler bir giriş parametresi olarak biz bunun yerine tüm geçirmek `ProductsRow` örneği:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Yerinde biçimlendirme işlevlere çağrılarla bir tarayıcıda bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Ekranınızın [üretimi DURDURULMUŞ] metni dahil olmak üzere devam etmeyen ürünleriyle Şekil 5'e benzer görünmelidir ve birden fazla $ kendi fiyat sahip 20,00 maliyeti bu ürünlerden metinle çağrısı fiyat teklifi için lütfen değiştirilir.


[![Pahalı ürünler için fiyat Lütfen çağrısı fiyat teklifi için bir metin ile değiştirilir](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Şekil 5**: pahalı ürünler için lütfen çağrısı fiyat teklifi için bir metin ile fiyat değiştirilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Özet

Verilere bağlı bir DataList veya yineleyici denetiminin içeriğini biçimlendirme, iki teknikler kullanılarak gerçekleştirilebilir. Bir olay işleyicisi oluşturmak için ilk tekniktir `ItemDataBound` veri kaynağındaki her kayıt yeni bir bağlı olarak hangi harekete olay `DataListItem` veya `RepeaterItem`. İçinde `ItemDataBound` olay işleyicisi geçerli s öğesi verileri incelenebilir ve ardından biçimlendirme uygulanabilir şablonunun veya için içeriğini `DataListItem` tüm öğeye s.

Alternatif olarak, özel biçimlendirme biçimlendirme işlevleri aracılığıyla alırlar. Biçimlendirme işlevi DataList çağrılabilen bir yöntem veya onun yerine yaymak üzere HTML döndürür yineleyici s şablonları kullanılabilir. Genellikle, bir biçimlendirme işlev tarafından döndürülen HTML geçerli öğeye bağlanan değerlere göre belirlenir. Bu değerleri biçimlendirme işlevdeki skaler değerler veya öğesine bağlanan tüm nesne geçirerek geçirilebilir (gibi `ProductsRow` örnek).

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Yaakov Ellis, Randy Etikan ve Liz Shulok yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
[sonraki](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
