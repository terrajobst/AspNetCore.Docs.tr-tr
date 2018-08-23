---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: (C#) veri değişikliği arabirimini özelleştirme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide standart metin değiştirerek düzenlenebilir bir GridView arayüzünü özelleştirmek nasıl inceleyeceğiz ve onay kutusu denetimleri ile alternati...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f7004192edd636f4660f3184c3e725a6bfda865c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752956"
---
<a name="customizing-the-data-modification-interface-c"></a>(C#) veri değişikliği arabirimini özelleştirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) veya [PDF olarak indirin](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> Bu öğreticide standart metin değiştirerek düzenlenebilir bir GridView arayüzünü özelleştirmek nasıl inceleyeceğiz ve onay kutusu ile giriş alternatif Web denetimleri denetler.


## <a name="introduction"></a>Giriş

GridView ve DetailsView denetimlerini tarafından kullanılan CheckBoxFields ve BoundFields salt okunur, düzenlenebilir ve Insertable arabirimleri işlemek için kendi yeteneği nedeniyle verileri değiştirme işlemini basitleştirir. Bu arabirimler, herhangi bir ek bildirim temelli işaretleme veya kod eklemeye gerek kalmadan oluşturulabilir. Ancak, genellikle gerçek dünya senaryolarında gerekli sağlamadığından CheckBoxField'ın ve BoundField arabirimleri yoksundur. GridView veya DetailsView düzenlenebilir veya Insertable arabirimini özelleştirmek için biz bunun yerine bir TemplateField kullanmanız gerekir.

İçinde [önceki öğretici](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) doğrulama Web denetimleri ekleyerek veri değişikliği arabirimleri özelleştirme gördük. Bu öğreticide BoundField ve CheckBoxField'ın standart metin değiştirme, gerçek veri koleksiyonu Web denetimleri ve alternatif giriş Web denetimleri CheckBox denetimleriyle nasıl özelleştirileceği şu konuları inceleyeceğiz. Özellikle, bir ürünün adı, kategori, tedarikçi ve artık sağlanmayan durum güncelleştirilmesini sağlayan bir düzenlenebilir GridView oluşturacağız. Belirli bir satır düzenlerken, kategori ve tedarikçi alanlar kullanılabilir kategori ve aralarından seçim yapabileceğiniz tedarikçileri kümesini içeren DropDownList işlenir. Ayrıca, size iki seçenek sunar RadioButtonList denetimiyle CheckBoxField'ın varsayılan onay kutusunu değiştireceksiniz: "Etkin" ve "Kullanımdan".


[![GridView'ın düzenleme arabirimini DropDownList ve RadioButton denetimleri içerir.](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Şekil 1**: GridView'ın arabirimi içeren DropDownList düzenleme ve RadioButton'ları ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>1. adım: uygun oluşturma`UpdateProduct`aşırı yükleme

Bu öğreticide, bir ürün adı, kategori, tedarikçi düzenleme verir ve durum kullanımdan düzenlenebilir bir GridView oluşturulacak. Bu nedenle, ihtiyacımız bir `UpdateProduct` bu dört ürün değerleri beş giriş parametrelerini kabul eden aşırı artı `ProductID`. Bizim önceki aşırı bu bir işlem gibi:

1. Veritabanı için belirtilen ürün bilgisi almak `ProductID`,
2. Güncelleştirme `ProductName`, `CategoryID`, `SupplierID`, ve `Discontinued` alanları ve
3. TableAdapter bağdaştırıcısının aracılığıyla DAL güncelleştirme isteğini göndermek `Update()` yöntemi.

Bu belirli bir aşırı yükleme için miyim sağlar, sağlayıcı tarafından sunulan tek ürün kullanımdan olarak işaretlenmiş bir ürün değil iş kuralı onay verilmemiştir. Tercih ederseniz ekleyebilirsiniz ücretsiz kullanım veya ideal olarak, ayrı bir yöntem mantığını kullanıma yeniden düzenleyin.

Aşağıdaki kod yeni gösterir `UpdateProduct` de aşırı `ProductsBLL` sınıfı:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>2. adım: düzenlenebilir GridView hazırlayın

İle `UpdateProduct` aşırı eklendi, biz bizim düzenlenebilir GridView oluşturmaya hazırsınız. Açık `CustomizedUI.aspx` sayfasını `EditInsertDelete` klasörü ve tasarımcıya bir GridView denetimi ekleyin. Ardından, yeni ObjectDataSource GridView'ın akıllı etiketten oluşturun. ObjectDataSource ile ürün bilgisi almak için yapılandırma `ProductBLL` sınıfın `GetProducts()` yöntemi ve ürün kullanarak verileri güncelleştirmek için `UpdateProduct` oluşturduğumuz aşırı yükleme. INSERT ve DELETE sekmeler aşağı açılan listelerden (hiçbiri) seçin.


[![Yeni oluşturduğunuz UpdateProduct aşırı yüklemesini kullanın ObjectDataSource yapılandırın](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Şekil 2**: ObjectDataSource kullanılacak yapılandırma `UpdateProduct` aşırı yükleme, yeni oluşturduğunuz ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image6.png))


Veri değişikliği öğreticileri anlatıldığı gibi Visual Studio tarafından oluşturulan ObjectDataSource bildirim temelli söz dizimi atar `OldValuesParameterFormatString` özelliğini `original_{0}`. Bizim yöntemleri orijinal beklemiyoruz olduğundan bu, bizim ile iş mantığı katmanı Elbette çalışmaz `ProductID` geçirilmesi için değer. Önceki öğreticilerde uyguladığımız güncelleştirmede olduğu gibi bu nedenle, bu özellik ataması bildirim temelli söz dizimi kaldırın veya bunun yerine, bu özelliğin değerini ayarlamak, birkaç dakikanızı `{0}`.

Bu değişiklikten sonra ObjectDataSource bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Unutmayın `OldValuesParameterFormatString` özelliği kaldırılmıştır ve var olan bir `Parameter` içinde `UpdateParameters` her biri tarafından beklenen giriş parametreleri için koleksiyon bizim `UpdateProduct` aşırı yükleme.

GridView şu anda yalnızca bir alt kümesini ürün değerleri güncelleştirmek için ObjectDataSource yapılandırılırken gösterir *tüm* ürün alanlarının. GridView düzenlemek için bir dakikanızı ayırın şekilde:

- Yalnızca içeren `ProductName`, `SupplierName`, `CategoryName` BoundFields ve `Discontinued` CheckBoxField
- `CategoryName` Ve `SupplierName` (sol tarafında) önce görüntülenecek alanları `Discontinued` CheckBoxField
- `CategoryName` Ve `SupplierName` BoundFields' `HeaderText` özelliği ayarlanmış "Kategori" ve "Sağlayıcı" sırasıyla
- Düzenleme desteği etkin (GridView'ın akıllı etiket düzenlemeyi etkinleştir onay kutusu denetimi)

Bu değişikliklerden sonra Tasarımcı aşağıda gösterilen GridView'ın bildirim temelli söz dizimi ile Şekil 3'e benzer olacaktır.


[![GridView ' gereksiz alanları Kaldır](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Şekil 3**: gereksiz alanları GridView kaldırın ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

Bu noktada GridView'ın salt okunur davranışı tamamlanmıştır. Verileri görüntülerken, her ürün ürün adı, kategori, tedarikçi, gösteren GridView bir satır olarak oluşturulur ve durumu kullanımdan kaldırıldı.


[![GridView'ın salt okunur arabirimidir tamamlandı](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Şekil 4**: GridView'ın salt okunur arabirimidir tamamlandı ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Bölümünde açıklandığı gibi [öğretici, bir genel bakış ekleme, güncelleştirme ve silme veri](an-overview-of-inserting-updating-and-deleting-data-cs.md), s görünüm durumu GridView (varsayılan davranış) etkin oldukça önemlidir. GridView s ayarlarsanız `EnableViewState` özelliğini `false`, eş zamanlı kullanıcıların yanlışlıkla silme veya düzenleme kayıtları riskiyle karşılaşırsınız. Bkz: [uyarısı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews bu düzenleme desteği ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/posts/10054.aspx) daha fazla bilgi için.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>3. adım: tedarikçi arabirimleri düzenleme ve kategori için bir DropDownList kullanma

Bu geri çağırma `ProductsRow` nesnesini içeren `CategoryID`, `CategoryName`, `SupplierID`, ve `SupplierName` gerçek yabancı anahtar kimliği değerlerinin sağlayan özellikler `Products` veritabanı tablo ve karşılık gelen `Name` değerler `Categories` ve `Suppliers` tablolar. `ProductRow`'S `CategoryID` ve `SupplierID` hem gelen okunabilir ve while yazılan `CategoryName` ve `SupplierName` özellikleri salt okunur işaretlenir.

Salt okunur durumu nedeniyle `CategoryName` ve `SupplierName` özellikleri, karşılık gelen BoundFields olduğu kendi `ReadOnly` özelliğini `true`, bu değerleri bir satır düzenlendiğinde değiştiren engelliyor. Biz ayarlayabilirsiniz `ReadOnly` özelliğini `false`, işleme `CategoryName` ve `SupplierName` BoundFields düzenleme sırasında metin kutuları olarak, bu tür bir yaklaşım sonuçlanacak bir özel durum kullanıcı olduğundan ürün güncelleştirmeye çalıştığında yok `UpdateProduct` içinde alan aşırı yüklemesini `CategoryName` ve `SupplierName` giriş. Aslında, biz iki nedenden dolayı bu tür bir aşırı oluşturmak istemiyorsanız:

- `Products` Tablo yoksa `SupplierName` veya `CategoryName` alanları, ancak `SupplierID` ve `CategoryID`. Bu nedenle, bu belirli bir kimliği değerleri, kendi arama tabloları değerleri geçirilecek bizim yöntemi istiyoruz.
- Kullanılabilir kategori ve üreticiler ve bunların doğru yazım bilmesini gerektirir üretici veya kategori adını yazın kullanıcının gerek değerinden en uygun seçenektir.

Kategori ve tedarikçileri adları salt okunur modda olduğunda Tedarikçi ve kategori alanlarını görüntülenmelidir (şimdi yaptığı gibi) ve aşağı açılan listesini düzenlenmekte olan seçenekleri uygulanabilir. Açılan listeyi kullanarak, son kullanıcı, hızlı bir şekilde hangi kategorileri ve tedarikçimiz arasından seçim için kullanılabilir olan görebilir ve daha kolayca kendi seçim yapabilirsiniz.

Bu davranışı sağlamak için dönüştürülecek ihtiyacımız `SupplierName` ve `CategoryName` BoundFields TemplateField halinde olan `ItemTemplate` yayan `SupplierName` ve `CategoryName` değerleri ve ayarlanmış `EditItemTemplate` listesine bir DropDownList denetimi kullanır kullanılabilir kategori ve sağlayıcıları.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Ekleme`Categories`ve`Suppliers`DropDownList

Başlangıç dönüştürerek `SupplierName` ve `CategoryName` TemplateField tarafından içine BoundFields: GridView'ın akıllı etiketinde sütunları Düzenle bağlantısına tıklayarak; BoundField sol alt köşesindeki; listeden seçerek ve tıklayarak "Bu alana dönüştürün bir TemplateField"bağlantısı. Dönüştürme işlemini bir TemplateField ikisiyle oluşturacak bir `ItemTemplate` ve `EditItemTemplate`bildirim temelli aşağıdaki sözdiziminde gösterildiği gibi:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

BoundField olarak salt okunur olarak işaretlendikten sonra hem `ItemTemplate` ve `EditItemTemplate` içeren bir etiketi Web ayarlanmış denetim `Text` özelliği için geçerli veri alanına bağlı (`CategoryName`, yukarıdaki söz diziminde). Değiştirilecek ihtiyacımız `EditItemTemplate`, etiket Web denetimi bir DropDownList denetimi ile değiştiriliyor.

Önceki öğreticilerde anlatıldığı gibi bir şablon Tasarımcısı aracılığıyla veya doğrudan bildirim temelli söz dizimi düzenlenebilir. Tasarımcı ile düzenlemek için GridView'ın akıllı etiketinde Şablonları Düzenle bağlantısına tıklayın ve kategori alanın ile çalışmayı tercih `EditItemTemplate`. Etiket Web denetimi kaldırın ve DropDownList'ın kimliği özelliğini ayarlamak bir DropDownList denetimi yerine `Categories`.


[![TexBox kaldırın ve bir DropDownList EditItemTemplate için ekleyin](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Şekil 5**: TexBox kaldırın ve eklemek için bir DropDownList `EditItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image15.png))


Sonraki DropDownList kategorileri ile doldurmak ihtiyacımız var. Akıllı etiket DropDownList'ın veri kaynağı Seç bağlantıdan tıklayın ve adlı yeni bir ObjectDataSource oluşturmayı `CategoriesDataSource`.


[![CategoriesDataSource adlı yeni bir ObjectDataSource denetimi oluşturma](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Şekil 6**: yeni bir ObjectDataSource Denetimi adlı oluşturma `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image18.png))


Kategorilerin tümünü dönüş bu ObjectDataSource sağlamak için kendisine bağlamak `CategoriesBLL` sınıfın `GetCategories()` yöntemi.


[![ObjectDataSource CategoriesBLL'ın GetCategories() yönteme bağlama](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Şekil 7**: ObjectDataSource için bağlama `CategoriesBLL`'s `GetCategories()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image21.png))


Son olarak, DropDownList'ın ayarlarını yapılandırma gibi `CategoryName` alan her bir DropDownList içinde görüntülenen `ListItem` ile `CategoryID` alan değeri olarak kullanılır.


[![CategoryName alanı görüntülenir ve CategoryID değeri olarak kullanılır](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Şekil 8**: sahip `CategoryName` alanı görüntülenir ve `CategoryID` değeri olarak kullanılır ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image24.png))


Bunları yapmak için bildirim temelli biçimlendirme değiştirdikten sonra `EditItemTemplate` içinde `CategoryName` TemplateField bir DropDownList hem bir ObjectDataSource içerir:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> İçinde DropDownList `EditItemTemplate` görünüm durumunu etkin olması gerekir. Kısa süre içinde veri bağlama söz dizimi DropDownList'ın bildirim temelli söz dizimi ekleyeceğiz ve veri bağlama komutları gibi `Eval()` ve `Bind()` yalnızca görünüm durumu etkin denetimlerinde görünebilir.


Adlı bir DropDownList eklemek için bu adımları yineleyin `Suppliers` için `SupplierName` TemplateField'ın `EditItemTemplate`. Bunun için bir DropDownList ekleme içerecektir `EditItemTemplate` ve başka bir ObjectDataSource oluşturma. `Suppliers` DropDownList'ın ObjectDataSource ancak yapılandırılmalıdır çağrılacak `SuppliersBLL` sınıfın `GetSuppliers()` yöntemi. Ayrıca, yapılandırma `Suppliers` görüntülenecek DropDownList `CompanyName` kullanın ve alan `SupplierID` alan değeri olarak kendi `ListItem` s.

İçin iki DropDownList ekledikten sonra `EditItemTemplate` s, bir tarayıcıda sayfayı ve Chef Acı'nın Cajun Seasoning ürün için Düzenle düğmesini tıklatın. Şekil 9 görüldüğü gibi ürün kategorisi ve tedarikçi sütunları kullanılabilir kategori ve sağlayıcılar arasından seçim içeren aşağı açılır listeler olarak işlenir. Ancak, unutmayın *ilk* hem açılan listelerdeki seçili öğeler (İçecekler kategorisi için) ve Exotic Liquids varsayılan tedarikçi, Chef Acı'nın Cajun Seasoning New Orleans Cajun tarafından sağlanan bir Condiment olsa bile Delights.


[![Aşağı açılan listeler ilk öğesinde varsayılan olarak seçilidir.](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Şekil 9**: ilk öğesinde açılan listeler, varsayılan olarak seçilidir ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image27.png))


Güncelleştir'e tıklayın, ayrıca, bu ürünün bulabilirsiniz `CategoryID` ve `SupplierID` değerler ayarlanmış `NULL`. Bu davranış neden olduğundan istenmeden içinde DropDownList `EditItemTemplate` s tüm veri alanları temel alınan ürün verileri bağlı değil.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Bağlama için DropDownList`CategoryID`ve`SupplierID`veri alanları

Düzenlenen ürün kategorisi ve tedarikçi açılır listede uygun değerlere ve gönderilen geri BLL için kullanıcının şu değerleri içeren kümesi `UpdateProduct` Güncelleştir'i tıklatarak üzerinde yöntemi, ihtiyacımız DropDownList bağlamak `SelectedValue` özellikleri `CategoryID` ve `SupplierID` veri alanlarını, çift yönlü veri bağlamasını kullanma. Bunu yapmanın `Categories` ekleyebileceğiniz DropDownList, `SelectedValue='<%# Bind("CategoryID") %>'` doğrudan için bildirim temelli söz dizimi.

Alternatif olarak, şablon Tasarımcısı aracılığıyla düzenlemeyi ve DropDownList'ın akıllı etiket Düzenle DataBindings bağlantıyı tıklatarak DropDownList'ın databindings ayarlayabilirsiniz. Ardından, göstermek `SelectedValue` özelliği bağlanan `CategoryID` çift yönlü veri bağlamasını kullanma alan (bkz. Şekil 10). Bağlama için ya da bildirim temelli veya tasarımcı işlemi tekrarlayın `SupplierID` veri alanı için `Suppliers` DropDownList.


[![CategoryID DropDownList'ın SelectedValue çift yönlü veri bağlamasını kullanma özelliği Bağla](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Şekil 10**: bağlama `CategoryID` DropDownList'e 's `SelectedValue` özelliğini kullanarak iki yönlü veri bağlama ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image30.png))


Bağlamaları için uygulandıktan sonra `SelectedValue` iki DropDownList özelliklerini, düzenlenen ürün kategorisi ve tedarikçi sütunları varsayılan olarak ürünün geçerli değerleri. Güncelleştirmeyi tıklayarak üzerine `CategoryID` ve `SupplierID` Seçili aşağı açılan liste öğesinin değerleri geçirilir `UpdateProduct` yöntemi. Veri bağlama ifadeleri eklendikten sonra Şekil 11 öğretici gösterir. nasıl Cajun Chef Acı'nın Seasoning için açılır listede seçilen öğeleri doğru Condiment ve New Orleans Cajun Delights olduğuna dikkat edin.


[![Düzenlenen ürünün geçerli kategori ve tedarikçi değerleri varsayılan olarak seçilidir](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Şekil 11**: düzenlenebilir ürünün geçerli kategori ve tedarikçi değerleri varsayılan olarak seçilidir ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>İşleme`NULL`değerleri

`CategoryID` Ve `SupplierID` sütunlarında `Products` tablo olabilir `NULL`, henüz içinde DropDownList `EditItemTemplate` s temsil etmek için bir liste öğesi eklenmeyen bir `NULL` değeri. Bu iki sonucu vardır:

- Kullanıcı, bir ürün kategorisi veya tedarikçi olmayan bir değiştirmek için arabirimimizi kullanamaz`NULL` değerini bir `NULL` bir
- Bir ürün varsa bir `NULL` `CategoryID` veya `SupplierID`, Düzenle düğmesine tıklayarak bir özel durum neden olur. Bunun nedeni, `NULL` tarafından döndürülen değer `CategoryID` (veya `SupplierID`) içinde `Bind()` deyimi bir değere DropDownList eşlenmiyor (DropDownList bir özel durum oluşturur, kendi `SelectedValue` özelliğini değerineayarlayın*değil* liste öğelerinin, koleksiyondaki).

Desteklemek için `NULL` `CategoryID` ve `SupplierID` değerleri ihtiyacımız diğerine eklemek `ListItem` temsil etmek için her bir DropDownList için `NULL` değeri. İçinde [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Öğreticisi, ek bir ekleme gördüğümüz `ListItem` DropDownList'ın ayarlama dahil DropDownList sınırlama için bir `AppendDataBoundItems` özelliğini `true` ve el ile ek ekleme `ListItem`. Önceki öğreticide yer, ancak ekledik bir `ListItem` ile bir `Value` , `-1`. Ancak, veri bağlama mantık, ASP.NET, otomatik olarak boş bir dizeye dönüştürür bir `NULL` ve tersi bir değer. Bu nedenle, Bu öğretici için istediğimiz `ListItem`'s `Value` boş bir dize olmalıdır.

İki DropDownList ayarlayarak başlangıç `AppendDataBoundItems` özelliğini `true`. Ardından, ekleme `NULL` `ListItem` aşağıdakileri ekleyerek `<asp:ListItem>` bildirim temelli biçimlendirmeyi aşağıdaki şekilde gözükmesi her DropDownList öğesine ister:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

Ben "(hiçbiri)" kullanılacak metin değeri olarak bunun için seçtiğiniz `ListItem`, ancak isterseniz de boş bir dize olacak şekilde değiştirebilirsiniz.

> [!NOTE]
> İçinde gördüğümüz gibi *ana/ayrıntı filtreleme ile bir DropDownList* öğreticide `ListItem` s eklenebilir Tasarımcısı aracılığıyla bir DropDownList DropDownList üzerinde 's tıklayarak `Items` özelliği Özellikler penceresinde (Bu görüntüler `ListItem` Koleksiyonu Düzenleyicisi). Ancak, eklediğinizden emin olun `NULL` `ListItem` bildirim temelli söz dizimi aracılığıyla Bu öğretici için. Kullanırsanız `ListItem` Koleksiyonu Düzenleyicisi, oluşturulan bildirim temelli söz dizimi sayar `Value` tamamen ayarlama gibi bildirim temelli biçimlendirme oluşturma, boş bir dize atanmış: `<asp:ListItem>(None)</asp:ListItem>`. Bu zararsız görünebilir, ancak eksik değer kullanılacak DropDownList neden `Text` onun yerine özellik değeri. Olması durumunda bu anlamına `NULL` `ListItem` olduğu belirlenirse, değeri "(hiçbiri)" atanacak denenecek `CategoryID`, bir özel durum oluşur. Açıkça ayarlayarak `Value=""`, `NULL` için değeri atanır `CategoryID` olduğunda `NULL` `ListItem` seçilir.


Üreticiler DropDownList için bu adımları yineleyin.

Bu ek `ListItem`, düzenleme arabirimi artık atayabilirsiniz `NULL` ürünün değerlerini `CategoryID` ve `SupplierID` Şekil 12'de gösterilen alanlar.


[![(Hiçbiri) bir ürün kategorisi veya sağlayıcı için bir NULL değer atamak için seçin](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Şekil 12**: (hiçbiri) atanacak seçin bir `NULL` bir ürün kategorisi veya tedarikçi değerini ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>4. adım: Kullanımdan kaldırılan durumu RadioButton'ları kullanma

Şu anda ürünlerin `Discontinued` veri alanı salt okunur satırlar için devre dışı onay kutusu ve düzenlenmekte olan satır için etkin bir onay kutusu işler bir CheckBoxField kullanılarak ifade edilir. Bu kullanıcı arabirimi genellikle uygun olsa da, biz bir TemplateField kullanma gerekirse özelleştirebilirsiniz. Bu öğreticide, CheckBoxField içinden kullanıcı ürünün belirtebileceğiniz iki seçenek "Etkin" ve "Kullanımdan" RadioButtonList denetimi kullanan bir TemplateField değiştirelim `Discontinued` değeri.

Başlangıç dönüştürerek `Discontinued` CheckBoxField ile bir TemplateField oluşturacak bir TemplateField içine bir `ItemTemplate` ve `EditItemTemplate`. CheckBox ile hem şablonları içerir, `Checked` özelliği bağlı `Discontinued` veri alanı, oluşturuluyor, ikisi arasındaki tek fark `ItemTemplate`ait onay kutusunu'nın `Enabled` özelliği `false`.

Hem de onay kutusunu değiştirin `ItemTemplate` ve `EditItemTemplate` RadioButtonList denetimde olduğu gibi her iki RadioButtonLists ayarlama `ID` özelliklerine `DiscontinuedChoice`. Ardından, RadioButtonLists her iki radyo düğmeleri, tek etiketli "etkin" içermesi gereken "False" ve "Kullanımdan" etiketli bir değeri olan "True" değeri ile belirtin. Bunu ya da girebilirsiniz gerçekleştirmek için `<asp:ListItem>` öğeleri doğrudan kullanın ve bildirim temelli söz dizimi aracılığıyla `ListItem` Koleksiyonu Düzenleyicisi'nden Tasarımcı. Şekil 13 gösterir `ListItem` iki radyo düğmesi seçenekleri sonra Koleksiyonu Düzenleyicisi belirtildi.


[![Ekleme](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Şekil 13**: "Etkin" ve "Artık Üretilmiyor" seçenekleri için RadioButtonList ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image39.png))


RadioButtonList içinde bu yana `ItemTemplate` düzenlenebilir olmamalıdır ayarlayın, `Enabled` özelliğini `false`, bırakma `Enabled` özelliğini `true` (varsayılan) RadioButtonList için `EditItemTemplate`. Bu radyo düğmeleri salt okunur olarak düzenlenen sıradaki yapar, ancak kullanıcının düzenlenen satır RadioButton değerlerini değiştirmek izin verir.

Hala RadioButtonList denetimleri atamak ihtiyacımız `SelectedValue` özelliklerine uygun radyo düğmesi seçilir, ürünün bağlı `Discontinued` veri alanı. İle bu öğreticinin önceki bölümlerinde incelenen DropDownList gibi bu veri bağlama söz dizimi ya da doğrudan bildirim temelli biçimlendirme veya veri bağlamaları Düzenle bağlantısına RadioButtonLists akıllı etiketler aracılığıyla eklenebilir.

İki RadioButtonLists ekleme ve bunları, yapılandırma sonrasında `Discontinued` TemplateField'ın bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Bu değişikliklerle `Discontinued` sütun dönüştürülmüş onay kutularından oluşan iki listesinden bir radyo düğmesi çiftleri (bkz. Şekil 14) bir listesi için. Bir ürün düzenlerken uygun radyo düğmesi seçilir ve ürünün artık sağlanmayan durum radyo düğmesini seçerek ve Güncelleştir'i tıklatarak güncelleştirilebilir.


[![Radyo düğmesi çiftlerine göre artık sağlanmayan onay kutularını değiştirilmiştir](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Şekil 14**: kullanımdan onay kutularını değiştirildi radyo düğmesi çiftleri tarafından ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Bu yana `Discontinued` sütununda `Products` veritabanı olamaz `NULL` değerleri değil ihtiyacımız yakalama hakkında endişelenmeye gerek `NULL` arabirimi bilgileri. Eğer, ancak `Discontinued` sütun içerebilir `NULL` biz üçüncü eklemek isteyeceğiniz değerler radyo düğmesini içeren listeye kendi `Value` boş bir dize olarak ayarlanmış (`Value=""`), kategori ve tedarikçi DropDownList ile olduğu gibi.


## <a name="summary"></a>Özet

Salt okunur, düzenleme ve ekleme arabirimleri BoundField ve CheckBoxField otomatik olarak işleme sırasında özelleştirme olanağı yeterli değildir. Genellikle, yine de arabirimi ekleme veya düzenleme özelleştirmek (önceki öğreticide gördüğümüz gibi), belki de doğrulama denetimleri ekleme gerekir veya veri koleksiyonu kullanıcı arabirimini özelleştirme (biz Bu öğreticide gördüğünüz gibi) tarafından. Bir TemplateField arabirimiyle özelleştirme aşağıdaki adımlarda toplanabilir:

1. Bir TemplateField eklemek veya mevcut BoundField ya da CheckBoxField bir TemplateField Dönüştür
2. Arabirim gerektiği gibi artırabilir.
3. Uygun veri alanlarını çift yönlü veri bağlamasını kullanma yeni eklenen Web denetimleri bağlama

Yerleşik ASP.NET Web denetimler kullanmanın yanı sıra, derlenmiş ve özel sunucu denetimleri ve kullanıcı denetimleri ile bir TemplateField şablonları özelleştirebilirsiniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [İleri](implementing-optimistic-concurrency-cs.md)
