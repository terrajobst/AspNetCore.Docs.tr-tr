---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Toplu güncelleştirmeler (C#) gerçekleştirme | Microsoft Docs
author: rick-anderson
description: Bir tam olarak düzenlenebilir oluşturmayı öğrenin DataList tüm öğeleri nerede içinde düzenleme modu ve değerleri, bir 'Tümünü Güncelleştir' düğmesini tıklatarak kaydedilebilir...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: 3528444269a3595681696251d3906a204090410c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807811"
---
<a name="performing-batch-updates-c"></a>Toplu güncelleştirmeler gerçekleştirme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) veya [PDF olarak indirin](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Bir tam olarak düzenlenebilir oluşturmayı öğrenin DataList tüm öğeleri nerede içinde düzenleme modu ve değerleri, sayfada bir "Tümünü Güncelleştir" düğmesine tıklayarak kaydedilebilir.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) biz bir öğe düzeyinde DataList oluşturma incelenir. Standart düzenlenebilir GridView her öğe DataList'te dahil gibi bir düzenleme düğmesi, tıklanan, öğesi düzenlenebilir hale getirir. Bu öğe düzeyinde de yalnızca zaman zaman güncelleştirilir veri için düzenleme çalışır, ancak belirli bir kullanım örneği senaryolarını birçok kaydını düzenlemek kullanıcının gerektirir. Bir kullanıcı, kayıt onlarca düzenlemek gereken ve Düzenle'ye tıklayın, yaptıkları değişiklikleri yapın ve her biri için Güncelleştir'e tıklayın zorlanır tıklayarak miktarını kendi üretkenlik engel olabilir. Bu gibi durumlarda, tam olarak düzenlenebilir bir DataList sağlamak için daha iyi bir seçenek olan bir *tüm* öğelerinden olan düzenleme modu ve değerleri, bir sayfa Tümünü Güncelleştir düğmesine tıklayarak düzenlenebilir (bkz. Şekil 1).


[![Her bir tam olarak düzenlenebilir DataList öğesi değiştirilebilir](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Şekil 1**: her bir tam olarak düzenlenebilir DataList öğesindeki değiştirilebilir ([tam boyutlu görüntüyü görmek için tıklatın](performing-batch-updates-cs/_static/image3.png))


Bu öğreticide kullanıcıların tam olarak düzenlenebilir bir DataList kullanarak tedarikçileri adres bilgilerini güncelleştirmesine olanak tanımak nasıl inceleyeceğiz.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>1. adım: DataList s ItemTemplate düzenlenebilir kullanıcı arabirimi oluşturma

Burada standart, öğe düzeyinde düzenlenebilir DataList oluşturma, şu iki şablon kullandık önceki öğreticide:

- `ItemTemplate` salt okunur kullanıcı arabirimi (her bir ürün adı ve fiyat görüntülemek için etiket Web denetimleri) içeriyor.
- `EditItemTemplate` düzenleme modu kullanıcı arabirimi (iki metin kutusuna Web denetimleri) içeriyor.

DataList s `EditItemIndex` özelliği belirleyen ne `DataListItem` (varsa) kullanılarak işlenir `EditItemTemplate`. Özellikle, `DataListItem` olan `ItemIndex` değerle s DataList `EditItemIndex` özelliği kullanılarak işlenir `EditItemTemplate`. Bu model, iyi yalnızca bir öğe bir zaman ancak uzaklıkta yer alan tam olarak düzenlenebilir bir DataList oluştururken düzenlenebilir olduğunda çalışır.

İçin tam olarak düzenlenebilir bir DataList, istediğimiz *tüm* , `DataListItem` düzenlenebilir arabirimini kullanarak işlemek için s. Bunu yapmanın en kolay yolu düzenlenebilir arabiriminde tanımlamaktır `ItemTemplate`. Tedarikçileri adres bilgilerini değiştirmek için adres, şehir ve ülke değerleri için sağlayıcı adı olarak metin ve metin kutuları düzenlenebilir arabirimi içerir.

Başlangıç açarak `BatchUpdate.aspx` sayfasında bir DataList denetimi ekleyin ve ayarlayın, `ID` özelliğini `Suppliers`. DataList s akıllı etiketten adlı yeni bir ObjectDataSource denetimi eklemek için iyileştirilmiş `SuppliersDataSource`.


[![SuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Şekil 2**: adlı yeni bir ObjectDataSource oluşturma `SuppliersDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](performing-batch-updates-cs/_static/image6.png))


ObjectDataSource ile veri almak için yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` metodu (bkz: Şekil 3). Önceki öğreticide, yerine gibi ObjectDataSource sağlayıcı bilgileri güncelleştiriliyor, doğrudan iş mantığı katmanı ile çalışırsınız. Bu nedenle, güncelleştirme sekmesinde aşağı açılan listesine (hiçbiri) ayarlayın (bkz: Şekil 4).


[![GetSuppliers() yöntemiyle sağlayıcı bilgileri alınamıyor](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Şekil 3**: tedarikçi bilgileri kullanarak almak `GetSuppliers()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](performing-batch-updates-cs/_static/image9.png))


[![Güncelleştirme sekmesinde aşağı açılan listesine (hiçbiri) ayarlayın](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Şekil 4**: güncelleştirme sekmesinde aşağı açılan listesine (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](performing-batch-updates-cs/_static/image12.png))


Sihirbazı tamamladıktan sonra Visual Studio otomatik olarak s DataList oluşturur `ItemTemplate` etiket Web denetiminde veri kaynağı tarafından döndürülen her veri alanı görüntülenecek. Bunun yerine düzenleme arabirimi sağlar, böylece bu şablonu değiştirmeniz gerekir. `ItemTemplate` Tasarımcısı DataList s akıllı etiket Şablonları Düzenle seçeneğini kullanarak veya doğrudan bildirim temelli söz dizimi aracılığıyla özelleştirilebilir.

Sağlayıcı adı metin olarak görüntüler, ancak tedarikçi s Adres, şehir ve ülke değerleri için metin kutuları içeren bir düzenleme arabirim oluşturmak için bir dakikanızı ayırın. Bu değişiklikleri yaptıktan sonra sayfa s bildirim temelli sözdizimi aşağıdakine benzer görünmelidir:


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Önceki öğreticide olarak bu öğreticideki DataList görünüm durumunu etkin olmalıdır.


İçinde `ItemTemplate` miyim iki yeni CSS sınıfları kullanarak m `SupplierPropertyLabel` ve `SupplierPropertyValue`, hangi eklenmiştir `Styles.css` sınıfı ve aynı stili ayarları kullanacak şekilde yapılandırılmış `ProductPropertyLabel` ve `ProductPropertyValue` CSS sınıfları.


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Bu değişiklikleri yaptıktan sonra bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Şekil 5 gösterildiği gibi her DataList öğesi üretici adı metin olarak görüntüler ve adres, şehir ve ülke görüntülenecek metin kutuları kullanır.


[![DataList'te her tedarikçi düzenlenebilir olduğunu](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Şekil 5**: her tedarikçi DataList'te düzenlenebilir olduğunu ([tam boyutlu görüntüyü görmek için tıklatın](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>2. adım: bir güncelleştirmeyi tüm düğme ekleme

Şekil 5'te her tedarikçi, adres, şehir ve ülke alanları bir metin kutusunda görüntülenen olsa da şu anda hiçbir güncelleştirme düğmesi mevcut değildir. Öğe başına bir güncelleştir düğmesine bozmanın yerine ile tam olarak düzenlenebilir belirleyebilen genellikle bir tek güncelleştirme tüm düğme vardır sayfada, tıklandığında, güncelleştirmeleri *tüm* DataList'te kayıtlar. Bu öğreticide, s (ya da düğmesi aynı etkinin olacağı rağmen) iki Tümünü Güncelleştir düğmesi - bir sayfanın üst ve alt bir ekleme olanak tanır.

DataList ve kümesi üzerinde bir düğme Web denetimi ekleyerek başlangıç kendi `ID` özelliğini `UpdateAll1`. Ardından, DataList altındaki ikinci düğme Web denetim ekleme ayarı kendi `ID` için `UpdateAll2`. Ayarlama `Text` iki düğme için güncelleştirme tüm özellikleri. Son olarak, her iki düğme için olay işleyicileri oluşturma `Click` olayları. Let s yeniden düzenleme her olay işleyicileri güncelleştirme mantığı yinelemek yerine bu mantığı üçüncü bir yönteme `UpdateAllSupplierAddresses`, yalnızca bu üçüncü yöntem çağırma olay işleyicilerine sahip.


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Şekil 6, güncelleştirme tüm düğmeler eklendikten sonra sayfada gösterilir.


[![İki güncelleştirme tüm düğme sayfaya eklenmiştir](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Şekil 6**: sayfasına iki güncelleştirme tüm düğmeler eklenmiştir ([tam boyutlu görüntüyü görmek için tıklatın](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>3. adım: Tüm Üreticiler adresi bilgileri güncelleştiriliyor

Tüm düzenleme arabirimi görüntüleme DataList s öğelerinin ve Tümünü Güncelleştir düğmesi ile birlikte tüm kalan yazıyor toplu güncelleştirme gerçekleştirmek için kod. Özellikle, arama ve DataList s öğeleri döngü ihtiyacımız `SuppliersBLL` s sınıfı `UpdateSupplierAddress` her biri için yöntemi.

Koleksiyonu `DataListItem` DataList s DataList erişilebilir, düzenini örnekler [ `Items` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Bir başvurusu olan bir `DataListItem`, biz buna karşılık gelen tutabileceğinizi `SupplierID` gelen `DataKeys` toplama ve program aracılığıyla metin Web denetimleri içinde başvuru `ItemTemplate` aşağıdaki kodda gösterildiği gibi:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Tümünü Güncelleştir düğmelerden birine kullanıcı tıkladığında `UpdateAllSupplierAddresses` yöntemi her yinelenir `DataListItem` içinde `Suppliers` DataList ve çağrıları `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi, karşılık gelen değerler geçirerek. Bir adres, şehir veya ülkede geçişleri için girilen olmayan bir değer değeridir `Nothing` için `UpdateSupplierAddress` (boş bir dize yerine), bir veritabanında sonuçlanır `NULL` temel alınan kayıt s alanları.

> [!NOTE]
> Bir geliştirme olarak, toplu güncelleştirme işlemi gerçekleştirildikten sonra bazı onay iletisi sağlayan sayfaya bir durum etiketi Web denetimi eklemek isteyebilirsiniz.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Değiştirilmiş adresleri güncelleştiriliyor

Bu öğretici çağrıları için kullanılan toplu güncelleştirme algoritması `UpdateSupplierAddress` yöntemi *her* sağlayıcı adresi bilgilerine mi değişti bakılmaksızın DataList'te. Böyle blind dahilse t genellikle bir performans sorunu güncelleştirirken, Denetim yapıldığı veritabanı tablosuna değişirse, gereksiz kayıtları neden olabilir. Örneğin, tüm kaydetmek için Tetikleyiciler kullanma `UPDATE` s `Suppliers` denetim tablosuna bir kullanıcı yeni bir denetim kaydı olup kullanıcı hiçbir değişiklik bağımsız olarak sisteminizdeki her üretici için oluşturulacak Tümünü Güncelleştir düğmesini her tıklayışında tablo değiştirir.

ADO.NET veri tablosu ve DataAdapter sınıfları, burada yalnızca değiştirilmiş, silinmiş ve yeni kayıtları sonuçları herhangi bir veritabanı iletişimde toplu güncelleştirmeleri destekleyecek şekilde tasarlanmıştır. Her satırda bir DataTable sahip bir [ `RowState` özelliği](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) satır kendisinden değiştirilmiş, silinmiş bir DataTable eklendi veya değişmeden kalır gösterir. Bir DataTable başlangıçta doldurulduğunda tüm satırları değişmemiş olarak işaretlenir. Satır s sütunlara değerinin değiştirilmesi satır değiştirilmiş olarak işaretlenir.

İçinde `SuppliersBLL` ilk okuyarak tek tedarikçi kayıtta belirtilen üretici s adresi bilgilerini güncelleştiriyoruz sınıfı bir `SuppliersDataTable` ve ardından `Address`, `City`, ve `Country` sütun değerleri aşağıdaki kodu kullanarak:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Bu kod naively geçilen adres, şehir ve ülke değerleri atar `SuppliersRow` içinde `SuppliersDataTable` değerleri değişip değişmediğini ne olursa olsun. Bu değişiklikleri neden `SuppliersRow` s `RowState` özelliği değiştirilmiş olarak işaretlenecek. Veri erişim katmanı s `Update` yöntemi çağrıldığında, bu gördüğünde `SupplierRow` değiştirilmiş ve bu nedenle gönderir bir `UPDATE` veritabanına komutu.

Ancak biz yalnızca gelen farklıysa geçilen adres, şehir ve ülke değerleri atamak için bu yöntem için kod eklenen Imagine `SuppliersRow` s mevcut değerleri. Adres, şehir ve ülke olduğu mevcut verilerle aynı durumda hiçbir değişiklik yapılmaz ve `SupplierRow` s `RowState` olarak işaretlenmiş sol değişmez. Net sonuç, DAL s `Update` yöntemi çağrıldığında, hiçbir veritabanı çağrısı yapılacak `SuppliersRow` değişiklik yapılmadı.

Bu değişikliği kabul etmek için geçilen adres, şehir ve ülke değerleri aşağıdaki kod ile doğrudan atama deyimleri değiştirin:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Bu kod, DAL s eklenen `Update` yöntemi gönderen bir `UPDATE` deyimi yalnızca adresi ile ilgili değerleri değiştirilmiş kayıtlar için veritabanı.

Alternatif olarak, biz geçilen adres alanlarını ve veritabanı verilerinin arasındaki farklılıkları olup olmadığını izler ve, varsa yok, yalnızca DAL s çağrısı atlama `Update` yöntemi. Bu yaklaşım da DB kullanarak yapıldığı dolaysız yöntem, DB doğrudan yöntem birincile t geçirilen beri çalışır bir `SuppliersRow` ayarlanmış örnek `RowState` bir veritabanı çağrısı gerçekten gerekli olup olmadığını belirlemek için denetlenebilir.

> [!NOTE]
> Her zaman `UpdateSupplierAddress` yöntemi çağrıldığında, güncelleştirilen kaydı hakkında bilgi almak için veritabanına bir çağrı yapılır. Ardından, verileri herhangi bir değişiklik varsa, tablo satırı güncelleştirmek için veritabanı başka bir çağrı yapılır. Oluşturarak bu iş akışı iyileştirilmiş bir `UpdateSupplierAddress` kabul eden bir yöntemi aşırı yüklemesini bir `EmployeesDataTable` olan örneği *tüm* değişikliklerden birini `BatchUpdate.aspx` sayfası. Ardından, tüm kayıtları almak için veritabanına bir çağrı yapabilirsiniz `Suppliers` tablo. İki sonuç kümeleri sonra sabit listesi oluşturulamadı ve burada değişiklikler kayıtları güncelleştirilemedi.


## <a name="summary"></a>Özet

Bu öğreticide bir kullanıcının birden çok satıcılara ait adres bilgilerini hızlı bir şekilde değiştirmek bir tam olarak düzenlenebilir DataList oluşturma gördük. DataList s içinde düzenleme arabirimi tedarikçi s Adres, şehir ve ülke değerleri için bir TextBox Web denetimi tanımlayarak Başladık `ItemTemplate`. Ardından, üstteki ve alttaki DataList güncelleştirme tüm düğmeler ekledik. Bir kullanıcı kendi değişiklikler ve güncelleştirme tüm düğmelerden birine tıklandığında sonra `DataListItem` s numaralandırılan ve çağrı `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi yapılır.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Zack Jones ve Ken Pespisa yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [İleri](handling-bll-and-dal-level-exceptions-cs.md)
