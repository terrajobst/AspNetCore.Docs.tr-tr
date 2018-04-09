---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Onay kutularını (VB) GridView sütunun ekleme | Microsoft Docs
author: rick-anderson
description: Bu öğretici, kullanıcı G. birden çok satır seçme sezgisel bir yol sağlamak üzere bir GridView denetimi için onay kutularını bir sütun ekleme bakan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: beb28de901805e07365f336192d20e914eeebb1e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Onay kutularını (VB) GridView sütunun ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) veya [PDF indirin](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> Bu öğreticide kullanıcının GridView birden çok satır seçme sezgisel bir yol sağlamak üzere bir GridView denetimi için onay kutularını bir sütun ekleme bakar.


## <a name="introduction"></a>Giriş

Önceki öğreticide belirli bir kayıt seçmek amacıyla GridView radyo düğmelerini bir sütun eklemek nasıl incelendi. Kullanıcı en fazla bir öğe kılavuzdan seçme için sınırlı olduğunda bir sütunu radyo düğmelerini uygun kullanıcı arabirimidir. Bazen, ancak biz rasgele bir kılavuz öğesi sayısı çekme kullanıcıya izin vermek isteyebilirsiniz. Web tabanlı e-posta istemcileri, örneğin, genellikle bir sütun onay kutularını sahip iletilerin listesini görüntüler. Kullanıcı rasgele bir ileti sayısı seçin ve ardından e-postaları başka bir klasöre taşıma veya bunları silme gibi bazı eylemleri gerçekleştirin.

Bu öğreticide onay kutuları, bir sütun eklemeyi ve hangi onay kutularını geri göndermede denetlendiğini belirleyin göreceğiz. Özellikle, biz web tabanlı e-posta istemcisi kullanıcı arabirimini yakından taklit eden bir örnek yapı. Bizim örneğimizde ürünlerinde listeleme disk belleğine alınan GridView içerecektir `Products` her bir onay kutusu ile veritabanı tablosu satır (bkz: Şekil 1). Tıklatıldığında Sil Seçili ürünlerin düğmesi, seçili bu ürünlerden silinmesine neden olur.


[![Bir onay kutusu her ürün satır içerir](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Şekil 1**: bir onay kutusu her ürün satır içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>1. adım: ürün bilgileri listeleyen bir disk belleğine alınan GridView ekleme

Onay kutuları, bir sütun ekleme hakkında endişelenmeniz önce disk belleği destekleyen GridView ürünlerinde listeleme s ilk odaklanmasına olanak tanır. Başlangıç açarak `CheckBoxField.aspx` sayfasındaki `EnhancedGridView` klasörü ve sürükleme GridView ayarı tasarımcıya araç kendi `ID` için `Products`. Ardından, GridView adlı yeni bir ObjectDataSource bağlanacağını seçin `ProductsDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLL` çağrılırken, sınıf `GetProducts()` verileri döndürmek için yöntem. Bu GridView salt okunur olacağından, güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![ProductsDataSource adlı yeni bir ObjectDataSource oluşturma](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Şekil 2**: yeni ObjectDataSource adlandırılmış oluşturma `ProductsDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![ObjectDataSource GetProducts() yöntemini kullanarak veri almak için yapılandırma](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Şekil 3**: ObjectDataSource kullanarak veri almak için yapılandırma `GetProducts()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) silme](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Şekil 4**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra BoundColumns ve CheckBoxColumn ürünle ilgili veri alanları için otomatik olarak Visual Studio oluşturur. Önceki öğreticide yaptığımız gibi Kaldır dışındaki tüm `ProductName`, `CategoryName`, ve `UnitPrice` BoundFields, değiştirip `HeaderText` ürün, kategori ve fiyat özellikleri. Yapılandırma `UnitPrice` BoundField böylece değeri para birimi olarak biçimlendirilir. Ayrıca akıllı etiketinden sayfalama etkinleştir onay kutusunu işaretleyerek disk belleği desteklemek için GridView yapılandırın.

Ayrıca seçili ürünlerin silmek için kullanıcı arabirimi ekleyin s olanak tanır. Bir düğme Web denetimi ayarı GridView altında ekleyin, `ID` için `DeleteSelectedProducts` ve kendi `Text` seçili ürünleri silme özelliğine. Ürünler veritabanından gerçekten silmek yerine, bu örnek için biz yalnızca silinmiş ürünleri bildiren bir ileti görüntülersiniz. Bunu yapabilmek için bir etiket Web denetimi düğmesi altındaki ekleyin. Kimliğini ayarlamak `DeleteResults`, kullanıma açık kendi `Text` özelliği ve kümesi kendi `Visible` ve `EnableViewState` özelliklerine `False`.

Bu değişiklikleri yaptıktan sonra GridView, ObjectDataSource, düğme ve etiket s bildirim temelli biçimlendirme aşağıdakine benzer olmalıdır:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Sayfasını bir tarayıcıda görüntülemek için bir dakikanızı ayırın (bkz. Şekil 5). Bu noktada ad, kategori ve ilk on ürünlerin fiyat görmeniz gerekir.


[![Ad, kategori ve ilk on ürünlerin fiyat listelenir](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Şekil 5**: ad, kategori ve ilk on ürünlerin fiyat listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>2. adım: onay kutuları, bir sütun ekleme

ASP.NET 2.0 bir CheckBoxField içerir olduğundan, biri onu bir GridView onay kutuları, bir sütun eklemek için kullanılabilir düşünebilirsiniz. CheckBoxField bir Boole veri alanı ile çalışmak üzere tasarlanmıştır gibi ne yazık ki, bu durumda değil. Diğer bir deyişle, CheckBoxField kullanabilmek için biz değeri işlenmiş onay kutusunun işaretli olup olmadığını belirlemek için alınmadığında temel alınan veri alanı belirtmeniz gerekir. CheckBoxField biz yalnızca işaretli onay kutuları, bir sütun eklemek için kullanamazsınız.

Bunun yerine, biz bir TemplateField ekleyin ve bir onay kutusu Web denetimi eklemeniz gerekir, `ItemTemplate`. Bir tane eklemek için bir TemplateField `Products` GridView ve ilk (sol uzak) alanı yapın. GridView s akıllı etiket Şablonları Düzenle bağlantısına tıklayın ve sonra bir onay kutusu Web denetimi araç sürükleyin `ItemTemplate`. Bu onay kutusunu s ayarlamak `ID` özelliğine `ProductSelector`.


[![TemplateField s ItemTemplate ProductSelector adlı bir onay kutusu Web denetim ekleme](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Şekil 6**: bir onay kutusu Web denetimi adlı eklemek `ProductSelector` TemplateField s `ItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


Eklenen TemplateField ve onay kutusu Web denetimi ile her satır bir onay kutusu artık içerir. Şekil 7 TemplateField ve onay kutusu eklendikten sonra bir tarayıcı görüntülendiğinde bu sayfada görüntülenir.


[![Her ürün satır artık bir onay kutusu içerir](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Şekil 7**: her ürün satır artık bir onay kutusu içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>3. adım:, Geri göndermede teslim hangi onay kutularını belirleme

Bu noktada onay kutularını ancak hiçbir şekilde geri göndermede hangi onay kutularını işaretlenmedi belirlemek için bir sütun vardır. Ancak, seçilen ürünleri Sil düğmesine tıklandığında, biz hangi onay kutularını bu ürünlerden silmek için işaretlenmedi bilmeniz gerekir.

GridView s [ `Rows` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) GridView veri satırları erişim sağlar. Bu satırlar yinelemek programlı erişim onay kutusu denetimi ve ardından bakın, `Checked` onay kutusunun seçili olup olmadığını belirlemek için özellik.

İçin bir olay işleyicisi oluşturun `DeleteSelectedProducts` düğmesi Web denetimi s `Click` olay ve aşağıdaki kodu ekleyin:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows` Özelliği bir koleksiyonunu döndürür `GridViewRow` bu oluşma şekli GridView s veri satırına örnekleri. `For Each` Döngü burada bu koleksiyon numaralandırır. Her `GridViewRow` nesne satır s onay kutusu, kullanarak program aracılığıyla erişilir `row.FindControl("controlID")`. Onay kutusu işaretli değilse, ilgili satır s `ProductID` değeri alınır `DataKeys` koleksiyonu. Bu alıştırmada, biz yalnızca bilgilendirici bir ileti görüntüler `DeleteResults` çalışan bir uygulama d bunun yerine bir çağrı vermiyoruz olsa da, etiket `ProductsBLL` s sınıfı `DeleteProduct(productID)` yöntemi.

Bu olay işleyicisi eklenmesiyle Sil Seçili ürünleri artık düğmesi görüntüler `ProductID` seçili ürünlerin s.


[![Sil Seçili ürünleri düğme tıklatıldığında seçili ürünleri ProductIDs listelenir](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Şekil 8**: zaman Sil Seçili ürünleri düğmesine tıklandığında seçili ürünleri `ProductID` s listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>4. adım: Tüm denetler ve tüm düğmeleri işaretini ekleme

Geçerli sayfadaki tüm ürünleri silmek bir kullanıcı istiyorsa, bunların her on onay kutularını denetlemelisiniz. Bu işlem bir denetleyin tüm ekleyerek hızlandırmak yardımcı olabiliriz, düğme tıklatıldığında, tüm onay kutularını kılavuzda seçer. Bir onay kutusunu temizleyin tüm düğmesi eşit yararlı olacaktır.

GridView yerleştirme sayfasında, iki düğmesi Web denetimleri ekleyin. İlk s ayarlamak `ID` için `CheckAll` ve kendi `Text` özelliğini denetleyin tüm; ikinci bir s kümesi `ID` için `UncheckAll` ve kendi `Text` özelliğine tüm seçeneğinin işaretini kaldırın.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Ardından, adlı arka plan kodu sınıfında bir yöntem oluşturma `ToggleCheckState(checkState)` , çağrıldığında, numaralandırır `Products` GridView s `Rows` koleksiyonu ve her onay kutusu s ayarlar `Checked` özelliği için geçirilen değerini de *checkState*  parametresi.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Ardından, oluşturun `Click` olay işleyicileri için `CheckAll` ve `UncheckAll` düğmeler. İçinde `CheckAll` s olay işleyicisi, yalnızca çağrısı `ToggleCheckState(True)`; `UncheckAll`, çağrı `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Bu kod Denetle düğmesini tıklatarak geri gönderimin neden olur ve tüm onay kutularını GridView içinde denetler. Benzer şekilde, tüm onay kutularının işaretini tüm tıklatarak seçili olanları kaldırdığında. Şekil 9 denetle düğmesine kaydedildikten sonra ekran gösterilir.


[![Tüm düğmesini onay tıklatmak tüm onay kutularını seçer](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Şekil 9**: denetleyin tüm düğmesini seçer tüm onay kutularını tıklatarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Onay kutuları, seçme veya tüm onay kutularını unselecting bir yaklaşım sütunun görüntüleme üstbilgi satırındaki onay kutusunu aracılığıyla olduğunda. Ayrıca, geçerli tüm denetleyin / işaretini tüm geri gönderimin gerektirir. Onay kutularını, ancak işaretli veya işaretsiz, böylece snappier bir kullanıcı deneyimi sağlayan tamamen istemci tarafı komut dosyası, olabilir. İstemci-tarafı teknikleri kullanarak bir bilgi ile birlikte ayrıntılı denetle ve işaretini tüm için bir başlık satırı onay kullanarak keşfetmek için kullanıma [denetimi tüm onay kutularını GridView kullanarak istemci tarafı komut dosyası ve bir denetleyin tüm onay](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Özet

Devam etmeden önce bir GridView rasgele bir satır sayısı arasından kullanıcılar izin vermeniz gerekiyor burada durumlarda, onay kutuları, bir sütun eklemeyi bir seçenektir. Bu öğreticide gördüğümüz gibi onay kutularını GridView içinde bir sütunu dahil TemplateField bir onay kutusu Web denetimi ile ekleme kapsar. Bir Web denetimi (önceki öğreticide yaptığımız gibi karşı biçimlendirme şablonuna doğrudan injecting) kullanarak ASP.NET otomatik olarak hatırlıyor ne onay kutularını olan ve geri gönderme arasında işaretli değil. Biz onay kutularını kodda belirli bir onay kutusunun işaretli olup olmadığını belirlemek için veya işaretli durumu değiştirmek için program aracılığıyla da erişebilirsiniz.

Bu öğretici ve sonuncu GridView satır seçici sütun ekleme Aranan. Bizim sonraki öğreticide nasıl iş bir bit ekleme özellikleri GridView ekleyebiliriz inceleyeceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](adding-a-gridview-column-of-radio-buttons-vb.md)
> [sonraki](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
