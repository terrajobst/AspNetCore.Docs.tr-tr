---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: DataList özelleştirme arabirimi (VB) düzenleme kullanıcının | Microsoft Docs
author: rick-anderson
description: Bu öğreticide DataList için DropDownLists ve bir onay kutusu içeren bir zengin düzenleme arabirimi oluşturacağız.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fbdb4687a23604b9f505bbf782d59a8c78e5a02
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-datalists-editing-interface-vb"></a>DataList'ın düzenleme arabirimi (VB) özelleştirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) veya [PDF indirin](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> Bu öğreticide DataList için DropDownLists ve bir onay kutusu içeren bir zengin düzenleme arabirimi oluşturacağız.


## <a name="introduction"></a>Giriş

İşaretleme ve Web denetimleri s DataList `EditItemTemplate` düzenlenebilir arabirimiyle tanımlayın. Tüm düzenlenebilir DataList örneklerin biz ve düzenlenebilir incelenmesi bugüne kadarki arabirimi, metin kutusuna Web denetimlerin oluşan. İçinde [önceki öğretici](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) biz doğrulama denetimleri ekleyerek saat düzenleme kullanıcı deneyimi geliştirildi.

`EditItemTemplate` Daha fazla Web denetimleri DropDownLists, RadioButtonLists, takvimler gibi TextBox dışında içerecek şekilde genişletilebilir ve benzeri. Diğer Web denetimleri dahil etmek için düzenleme arabirimi özelleştirirken metin kutuları gibi ile aşağıdaki adımları kullanın:

1. Web denetimine ekleme `EditItemTemplate`.
2. Veri bağlama sözdizimini uygun özelliğine karşılık gelen veri alan değeri atamak için kullanın.
3. İçinde `UpdateCommand` olay işleyicisi, program aracılığıyla erişim Web değerini denetlemek ve uygun BLL yönteme geçirin.

Bu öğreticide DataList için DropDownLists ve bir onay kutusu içeren bir zengin düzenleme arabirimi oluşturacağız. Özellikle, ürün bilgilerini listeler ve s ürün adı, tedarikçi, kategori ve devam etmeyen durumu güncelleştirilmesi izin veren bir DataList oluşturacağız (bkz: Şekil 1).


[![Metin kutusu, iki DropDownLists ve bir onay kutusu düzenleme arabirimi içerir](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Şekil 1**: metin kutusu, iki DropDownLists ve bir onay kutusu düzenleme arabirimi içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>1. adım: Ürün bilgilerini görüntüleme

DataList s düzenlenebilir arabirimi oluşturabilmeniz için önce ilk salt okunur arabirimi oluşturmamız gereken. Başlangıç açarak `CustomizedUI.aspx` gelen sayfa `EditDeleteDataList` klasörü ve DataList sayfasına ekleme Tasarımcısı'ndan ayarını kendi `ID` özelliğine `Products`. DataList s akıllı etiketten yeni ObjectDataSource oluşturun. Bu yeni ObjectDataSource ad `ProductsDataSource` ve verilerin alınacağı yapılandırın `ProductsBLL` s sınıfı `GetProducts` yöntemi. Olarak önceki düzenlenebilir DataList öğreticiler ile düzenlenen ürün s bilgilerini doğrudan iş mantığı katmanı giderek güncelleştiriyoruz. Buna göre güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![Güncelleştirme, ekleme ve silme sekmeleri açılan listeleri (hiçbiri) ayarlayın](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Şekil 2**: güncelleştirme, ekleme ve silme sekmelerini aşağı açılan listeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


ObjectDataSource yapılandırdıktan sonra Visual Studio varsayılan oluşturacak `ItemTemplate` için ad ve değer veri alanların her biri için listeler DataList döndürdü. Değiştirme `ItemTemplate` şablonu, ürün adı listeler böylece bir `<h4>` kategori adı, üretici adı, fiyat ve devam etmeyen durum birlikte öğesi. Ayrıca, dikkat ederek bir Düzenle düğmesi ekleme kendi `CommandName` özelliği düzenleme için ayarlanır. Bildirim temelli biçimlendirme için my `ItemTemplate` izler:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Yukarıdaki biçimlendirme ürün bilgileri kullanarak çıkışı yerleştirir bir &lt;h4&gt; s ürün adı ve dört sütunlu için başlık `<table>` kalan alanları için. `ProductPropertyLabel` Ve `ProductPropertyValue` tanımlanan CSS sınıfları `Styles.css`, önceki eğitimlerine ele. Şekil 3'te bir tarayıcıdan görüntülendiğinde bizim ilerleme durumunu gösterir.


[![Ad, tedarikçi, kategori, kullanımdan durumu ve her ürün fiyat görüntülenir](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Şekil 3**: Name, tedarikçi, kategori, kullanımdan durumu ve her ürün fiyat görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>2. adım: Web denetimleri düzenleme arabirimi ekleme

Düzenleme arabirimidir gerekli Web denetimleri eklemek için özelleştirilmiş DataList oluşturmanın ilk adımı `EditItemTemplate`. Özellikle, bir DropDownList tedarikçi için başka ve devam etmeyen durumu için bir onay kutusu kategorisi için ihtiyacımız var. Ürün s fiyat bu örnekte düzenlenebilir olmadığından, biz bir etiket Web denetimi kullanarak görüntüleme devam edebilirsiniz.

Düzenleme arabirimini özelleştirmek için DataList s akıllı etiket Şablonları Düzenle bağlantısından tıklatın ve seçin `EditItemTemplate` aşağı açılan listeden seçeneği. Bir DropDownList eklemek `EditItemTemplate` ve kendi `ID` için `Categories`.


[![Bir DropDownList kategorilerini ekleyin](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Şekil 4**: bir DropDownList kategorilerini ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Ardından, DropDownList s akıllı etiket veri kaynağı Seç seçeneği seçin ve adlı yeni bir ObjectDataSource oluşturma `CategoriesDataSource`. Kullanmak için bu ObjectDataSource yapılandırma `CategoriesBLL` s sınıfı `GetCategories()` yöntemi (bkz. Şekil 5). Ardından, her biri için kullanılacak veri alanları için veri kaynağı Yapılandırma Sihirbazı DropDownList s ister `ListItem` s `Text` ve `Value` özellikleri. DropDownList görüntü sahip `CategoryName` veri alan ve kullanım `CategoryID` Şekil 6'da gösterildiği gibi değer olarak.


[![CategoriesDataSource adlı yeni bir ObjectDataSource oluşturma](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Şekil 5**: yeni ObjectDataSource adlandırılmış oluşturma `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![DropDownList s görüntü yapılandırmak ve alan değeri](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Şekil 6**: DropDownList s görüntüleme ve değeri alanlarını yapılandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Bu dizi bir DropDownList tedarikçileri oluşturma adımları yineleyin. Ayarlama `ID` bu DropDownList için `Suppliers` ve kendi ObjectDataSource ad `SuppliersDataSource`.

İki DropDownLists ekledikten sonra devam etmeyen durumu için bir onay kutusu ve ürün adı için bir metin kutusu ekleyin. Ayarlama `ID` onay kutusunu ve metin kutusuna s `Discontinued` ve `ProductName`sırasıyla. Kullanıcı ürün s adı için bir değer sağlar emin olmak için bir RequiredFieldValidator ekleyin.

Son olarak, Güncelleştir ve İptal düğmeleri ekleyin. Bu iki düğmelerini, kesinlik temelli olduğunu unutmayın, kendi `CommandName` özellikleri, Güncelleştir ve iptal etmek, sırasıyla için ayarlanır.

Ancak, istediğiniz düzenleme arabirimi düzenlemek çekinmeyin. I ullanıcı seçti aynı dört sütunlu kullanmak için `<table>` salt okunur arabirimi, aşağıdaki bildirim temelli söz dizimini ve ekran görüntüsü olarak düzeninden gösterilmektedir:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![Düzenleme yerleştirilmiş çıkışı salt okunur arabirimi gibi arabirimdir](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Şekil 7**: düzenleme arabirimi olduğunu yerleştirilmiş salt okunur arabirimi gibi ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>3. adım: EditCommand ve CancelCommand olay işleyicileri oluşturma

Şu anda hiçbir veri bağlamasını sözdiziminde bulunmamaktadır `EditItemTemplate` (dışında `UnitPriceLabel`, hangi kopyalanmıştır üzerinden aynen `ItemTemplate`). Veri bağlama sözdizimi kısa bir süre içinde ekleyeceğiz, ancak ilk let s s DataList için olay işleyicileri oluşturma `EditCommand` ve `CancelCommand` olaylar. Sözcüğünün sorumluluğu `EditCommand` olay işleyicisidir, Düzenle düğmesine tıklandığında, DataList öğesi düzenleme arabirimi ancak işlemek için `CancelCommand` s iş DataList önceden düzenleme durumuna geri dönün.

Bu iki olay işleyicileri oluşturma ve bunları aşağıdaki kodu kullanın:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Bu bir yerde, iki olay işleyicileri için Düzenle düğmesini düzenleme arabirimi görüntüler ve iptal düğmesi, salt okunur modda düzenlenen öğesi döndürür. Düzenle düğmesi Chef Anton s Baharat karışımı tıklatıldıktan sonra Şekil 8 DataList gösterir. Biz bu yana henüz hiçbir veri bağlamasını sözdizimi düzenleme arabirimine eklemek için ve `ProductName` TextBox boşsa `Discontinued` onay kutusunu işaretlemeden ve ilk öğeler seçildiği `Categories` ve `Suppliers` DropDownLists.


[![Düzenle düğmesi görüntüler düzenleme arabirimi tıklatarak](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Şekil 8**: düzenleme arabirimi görüntüler için Düzenle düğmesini ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>4. adım: Veri bağlamada sözdizimi düzenleme arabirimi ekleme

Geçerli ürün s değerlerini görüntüleme düzenleme arabirimi sağlamak için biz veri alan değerlerini uygun Web denetimi değerleri atamak için veri bağlamasını sözdizimi kullanmanız gerekir. Veri bağlama sözdizimi Şablonları Düzenle ekranına giderek Designer üzerinden uygulanabilir ve Web sunucusundan veri bağlamaları Düzenle bağlantısını seçerek akıllı etiketler denetler. Alternatif olarak, veri bağlama sözdizimi doğrudan bildirim temelli biçimlendirme eklenebilir.

Ata `ProductName` verileri alan değerine `ProductName` TextBox s `Text` özelliği, `CategoryID` ve `SupplierID` verileri alan değerine `Categories` ve `Suppliers` DropDownLists `SelectedValue` özelliklerini ve `Discontinued` verileri alan değerine `Discontinued` onay kutusunu s `Checked` özelliği. Tasarımcı veya aracılığıyla doğrudan bildirim temelli biçimlendirme bu değişiklikleri yaptıktan sonra sayfası bir tarayıcı aracılığıyla yeniden ziyaret ve Chef Anton s Baharat karışımı için Düzenle düğmesini tıklatın. Şekil 9 gösterildiği gibi veri bağlama sözdizimi metin kutusuna, DropDownLists ve onay kutusu geçerli değerleri ekledi.


[![Düzenle düğmesi görüntüler düzenleme arabirimi tıklatarak](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Şekil 9**: düzenleme arabirimi görüntüler için Düzenle düğmesini ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>5. adım: kullanıcı s değişiklikleri UpdateCommand olay işleyicisini kaydetme

Kullanıcı bir ürün düzenler ve geri gönderimin oluştuğu güncelleştirme düğmesine tıklar ve s DataList `UpdateCommand` olay etkinleşir. Olay işleyicisi, Web denetimlerinde değerleri okunacak ihtiyacımız `EditItemTemplate` ve BLL ürünü veritabanında güncelleştirmek için arabirimle. Biz olarak önceki eğitimlerine görülen ve `ProductID` güncelleştirilmiş üzerinden erişilebilir ürünüdür `DataKeys` koleksiyonu. Kullanıcı tarafından girilen alanları programlamayla kullanarak Web denetimleri başvurarak erişilen `FindControl("controlID")`, aşağıdaki kodda gösterildiği gibi:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Danışmanlık tarafından kodu başlar `Page.IsValid` özelliği sayfadaki tüm doğrulama denetimleri geçerli olduğundan emin olun. Varsa `Page.IsValid` olan `True`, düzenlenen ürün s `ProductID` değer okuma `DataKeys` koleksiyonu ve Web denetimleri veri girişi `EditItemTemplate` programlı olarak başvurulur. Ardından, bu Web denetimlerinden sonra uygun geçirilir değişkenleri içine değerler `UpdateProduct` aşırı yükleme. Verileri güncelleştirdikten sonra DataList önceden düzenleme durumuna geri döner.

> [!NOTE]
> T ve özel durum işleme mantığı eklenen atlanmış [işleme BLL - ve DAL düzeyinde istisnalar](handling-bll-and-dal-level-exceptions-vb.md) kod ve bu örnek tutmak için öğretici odaklanır. Bu öğreticiyi tamamladıktan sonra bu işlev bir alıştırma ekleyin.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>6. adım: NULL adlı kullanıcı, Categoryıd'si ve TedarikçiKimliği değerlerini işleme

Northwind veritabanı sağlar `NULL` değerleri `Products` s tablosu `CategoryID` ve `SupplierID` sütun. Ancak, bizim düzenleme arabirimi içermiyor t şu anda uyum `NULL` değerleri. Sahip bir ürün düzenlemek çalışırsanız bir `NULL` değeri ya da kendi `CategoryID` veya `SupplierID` geçeceğiz sütunları, bir `ArgumentOutOfRangeException` benzer bir hata iletisiyle: *'Kategorisi' mevcut olmadığından, geçersiz olan bir SelectedValue vardır öğeler listesinde yok.* Ayrıca, orada s şu anda hiçbir şekilde s ürün kategorisi veya tedarikçi değiştirmek için değer olmayan bir`NULL` değeri bir `NULL` biri.

Desteklemek için `NULL` değerleri tedarikçi DropDownLists ve kategori için ihtiyacımız ek eklemek `ListItem`. T (hiçbiri) olarak kullanmak üzere seçilen ullanıcı `Text` bu değeri `ListItem`, ancak d, isterseniz bunu (gibi boş bir dize) başka bir şey için değiştirebilirsiniz. Son olarak, DropDownLists ayarlamayı unutmayın `AppendDataBoundItems` için `True`; kategorileri bunun unuttunuz ve DropDownList bağlı Üreticiler statik olarak eklenen üzerine yazılacak `ListItem`.

S DataList DropDownLists biçimlendirmede bu değişiklikleri yaptıktan sonra `EditItemTemplate` aşağıdakine benzer görünmelidir:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statik `ListItem` s DropDownList Tasarımcı veya doğrudan bildirim temelli söz dizimi aracılığıyla eklenebilir. Bir veritabanı temsil etmek için bir DropDownList öğesi eklerken `NULL` değeri, eklediğinizden emin olun `ListItem` aracılığıyla Tanımlayıcı Sözdizimi. Kullanırsanız `ListItem` Koleksiyonu Düzenleyicisi Tasarımcısı'nda oluşturulan Tanımlayıcı Sözdizimi atlayın `Value` tamamen ayarıyla atanan tanımlayıcı biçimlendirme gibi oluşturma, boş bir dize: `<asp:ListItem>(None)</asp:ListItem>`. Bu zararsız, görünebilir ancak eksik `Value` kullanmak DropDownList neden `Text` onun yerine özellik değeri. Olması durumunda bu anlamına `NULL` `ListItem` olan seçili değer (hiçbiri) ürün veri alanına atanan kurulmaya çalışılır (`CategoryID` veya `SupplierID`, bu öğreticide), bir özel durum oluşur. Açıkça ayarlayarak `Value=""`, `NULL` değeri ürüne atanacak veri alanı `NULL` `ListItem` seçilir.


Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Bir ürün düzenlerken unutmayın `Categories` ve `Suppliers` DropDownLists hem (hiçbiri) sahip DropDownList başlangıç seçeneği.


[![Kategoriler ve Üreticiler DropDownLists (None) seçeneği içerir](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Şekil 10**: `Categories` ve `Suppliers` DropDownLists (None) seçeneği içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Kaydetmek için bir veritabanı olarak (hiçbiri) seçeneğini `NULL` değeri ihtiyacımız geri dönmek `UpdateCommand` olay işleyicisi. Değişiklik `categoryIDValue` ve `supplierIDValue` boş değer atanabilir tamsayı olması ve bunları dışında bir değer atamak için değişkenleri `Nothing` yalnızca DropDownList s `SelectedValue` boş bir dize değil:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Bu değişiklik, bir değeri ile `Nothing` içine geçirilen `UpdateProduct` kullanıcı (hiçbiri) seçtiyseniz BLL yöntemi seçeneği da karşılık gelen açılan listeleri bir `NULL` veritabanı değeri.

## <a name="summary"></a>Özet

Bu öğreticide bir metin kutusuna, iki DropDownLists ve doğrulama denetimlerle birlikte bir onay kutusu üç farklı giriş Web denetimleri dahil daha karmaşık bir düzenleme DataList arabirimi oluşturma gördük. Düzenleme arabirimi oluştururken kullanılan Web denetimleri bakılmaksızın aynı adımlardır: s DataList Web denetimler ekleyerek Başlat `EditItemTemplate`; uygun Web karşılık gelen veri alan değerleriyle atamak için veri bağlama söz dizimini kullanın Denetim Özellikleri; hem de `UpdateCommand` olay işleyicisi program aracılığıyla Web denetimleri ve BLL değerlerine geçirme özelliklerinin uygun erişim.

Düzenleme bir arabirim olup oluştururken bu s oluşan yalnızca metin kutuları veya farklı Web denetimler koleksiyonu, veritabanı düzgün işlenecek emin `NULL` değerleri. Hesap zaman `NULL` bu durumda varolan yalnızca doğru görüntüleme kesinlik temelli `NULL` düzenleme arabirimi, aynı zamanda, değerinde bir değer olarak işaretlemek için bir yol sunar `NULL`. Belirleyebilen içinde DropDownLists için genellikle anlamına statik ekleme `ListItem` , `Value` özelliğini açıkça boş bir dize olarak ayarlayın (`Value=""`) ve biraz kod ekleme `UpdateCommand` belirlemek için olay işleyicisini olsun veya olmasın `NULL``ListItem` seçilmedi.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Dennis Patterson, David Suru ve Randy Etikan yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
