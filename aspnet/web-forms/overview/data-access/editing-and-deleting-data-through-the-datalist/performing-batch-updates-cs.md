---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: "Toplu güncelleştirmeleri (C#) gerçekleştirme | Microsoft Docs"
author: rick-anderson
description: "Bir tam olarak düzenlenebilir oluşturmayı öğrenin burada öğelerinden tümü de DataList düzenleme modu ve değerleri 'Tümünü Güncelleştir' düğmesini tıklatarak kaydedilebilmesi için..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: 989bd80bf2d8b6548fd8e4abd492408a72104070
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="performing-batch-updates-c"></a>Toplu güncellemelerini (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) veya [PDF indirin](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Bir tam olarak düzenlenebilir oluşturmayı öğrenin burada öğelerinden tümü de DataList düzenleme modu ve değerleri sayfasında bir "Tümünü Güncelleştir" düğmesini tıklatarak kaydedilebilir.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) biz bir öğe düzeyinde DataList oluşturma inceledi. Standart düzenlenebilir GridView DataList her öğe dahil gibi bir Düzenle düğmesini, tıklandığında, öğesi düzenlenebilir hale getirir. Bu öğe düzeyinde de yalnızca zaman zaman güncelleştirilir veri için düzenleme çalışır, ancak belirli kullanım örneği senaryosu birçok kaydını düzenlemek kullanıcının gerektirir. Kullanıcı kayıtları düzinelerce düzenlemek gereken ve Düzenle'yi tıklatın, kendi değişiklikleri yapın ve her biri için Güncelleştir'i zorlanır, tıklatarak miktarını kendi üretkenlik engel olabilir. Böyle durumlarda, daha iyi bir seçenek tam olarak düzenlenebilir bir DataList sağlamaktır bir *tüm* öğelerinden olan düzenleme modunda ve değerleri sayfasında bir Tümünü Güncelleştir düğmesini tıklatarak düzenlenebilir (bkz: Şekil 1).


[![Her öğe bir tam olarak düzenlenebilir DataList değiştirilebilir](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Şekil 1**: tam olarak düzenlenebilir DataList her öğesinde değiştirilebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-batch-updates-cs/_static/image3.png))


Bu öğreticide biz kullanıcıların tam olarak düzenlenebilir DataList kullanarak Üreticiler adres bilgilerini güncelleştirmek nasıl inceleyeceğiz.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>1. adım: DataList s ItemTemplate düzenlenebilir kullanıcı arabirimi oluşturma

Burada standart, öğe düzeyinde düzenlenebilir DataList oluşturma, şu iki şablonları kullandık önceki öğreticide:

- `ItemTemplate`salt okunur kullanıcı arabirimi (her bir ürün adı ve fiyat görüntüleme için etiket Web denetimleri) içeriyor.
- `EditItemTemplate`düzenleme modu kullanıcı arabirimi (iki metin kutusuna Web denetimleri) içeriyor.

S DataList `EditItemIndex` özelliği belirleyen ne `DataListItem` (varsa) kullanılarak oluşturulması `EditItemTemplate`. Özellikle, `DataListItem` , `ItemIndex` değerle s DataList `EditItemIndex` özelliği kullanılarak işlenir `EditItemTemplate`. Bu model, iyi yalnızca bir öğe bir zaman alır, ancak parçalayın düştüğünde tam olarak düzenlenebilir DataList oluştururken düzenlenebilir olduğunda çalışır.

Tam olarak düzenlenebilir bir DataList için istiyoruz *tüm* , `DataListItem` düzenlenebilir arabirimini kullanarak işlemek için s. Bunu yapmanın en kolay yolu düzenlenebilir arabiriminde tanımlamaktır `ItemTemplate`. Üreticiler adres bilgilerini değiştirmek için düzenlenebilir arabirimi adres, şehir ve ülke değerleri için metin ve metin kutuları olarak sağlayıcı adı içeriyor.

Başlangıç açarak `BatchUpdate.aspx` sayfasında DataList denetimi ekleyin ve ayarlama kendi `ID` özelliğine `Suppliers`. Adlı yeni bir ObjectDataSource denetimi eklemek için DataList s akıllı etiketten kabul `SuppliersDataSource`.


[![SuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Şekil 2**: yeni ObjectDataSource adlandırılmış oluşturma `SuppliersDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-batch-updates-cs/_static/image6.png))


ObjectDataSource kullanarak veri almak için yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi (bkz: Şekil 3). Önceki öğretici yerine gibi ObjectDataSource üzerinden Tedarikçi bilgilerini güncelleştirme ile doğrudan iş mantığı katmanı ile çalışırsınız. Bu nedenle, aşağı açılan liste (hiçbiri) güncelleştirme sekmesindeki ayarlayın (Şekil 4'e bakın).


[![GetSuppliers() yöntemini kullanmayı deneyin Tedarikçi bilgilerini alma](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Şekil 3**: tedarikçi bilgileri kullanarak almak `GetSuppliers()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-batch-updates-cs/_static/image9.png))


[![Güncelleştirme sekmesinde aşağı açılan listesine (hiçbiri) ayarlayın](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Şekil 4**: güncelleştirme sekmesinde aşağı açılan listesine (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-batch-updates-cs/_static/image12.png))


Sihirbazı tamamladıktan sonra Visual Studio otomatik olarak s DataList oluşturur `ItemTemplate` etiket Web denetiminde veri kaynağı tarafından döndürülen her veri alanı görüntülemek için. Böylece yerine düzenleme arabirimi sağlar, bu şablonu değiştirmek gerekir. `ItemTemplate` Tasarımcısı DataList s akıllı etiket Şablonları Düzenle seçeneğini kullanarak veya doğrudan bildirim temelli söz dizimi aracılığıyla özelleştirilebilir.

Üretici s adı metin olarak görüntüler, ancak metin kutuları tedarikçi s adresi, şehir ve ülke değerleri içeren bir düzenleme arabirim oluşturmak için bir dakikanızı ayırın. Bu değişiklikleri yaptıktan sonra sayfa s Tanımlayıcı Sözdizimi aşağıdakine benzer görünmelidir:


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Olarak önceki eğitici etkin görünüm durumunu bu öğreticideki DataList olması gerekir.


İçinde `ItemTemplate` ı iki yeni CSS sınıfları, kullanarak m `SupplierPropertyLabel` ve `SupplierPropertyValue`, hangi eklenmiştir `Styles.css` sınıfı ve aynı stili ayarları kullanmak üzere yapılandırılmış `ProductPropertyLabel` ve `ProductPropertyValue` CSS sınıfları.


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Bu değişiklikleri yaptıktan sonra bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Şekil 5 gösterildiği gibi her bir DataList öğesi üretici adı metin olarak görüntüler ve metin kutuları adres, şehir ve ülke görüntülemek için kullanır.


[![DataList her tedarikçi düzenlenebilir değil](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Şekil 5**: DataList her tedarikçi düzenlenebilir değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>2. adım: bir güncelleştirme tüm düğme ekleme

Şekil 5'te her sağlayıcı adresi, şehir ve bir metin kutusunda görüntülenen ülke alanları sahipken, şu anda hiçbir güncelleştirme düğmesi mevcut değil. Öğe başına bir güncelleştirme düğmesi sahip olmak yerine ile tam olarak düzenlenebilir belirleyebilen yoktur genellikle tek bir güncelleştirme tüm düğme sayfada, tıklatıldığında, güncelleştirmeleri *tüm* DataList kayıtlarında. Bu öğretici için (ya da düğmesini tıklatarak aynı etkiye sahip olacaktır rağmen) iki Tümünü Güncelleştir düğmesi - sayfanın üst kısmındaki diğeri altındaki eklemek s olanak tanır.

Başlat düğmesi Web denetimi kümesi ve DataList yukarıda ekleyerek kendi `ID` özelliğine `UpdateAll1`. Ardından, ikinci düğme Web denetimi DataList altına ekleyin ayarı kendi `ID` için `UpdateAll2`. Ayarlama `Text` iki düğmelere güncelleştirme tüm özellikleri. Son olarak, her iki düğmeleri için olay işleyicileri oluşturma `Click` olaylar. Let s yeniden düzenlemeniz her olay işleyicileri güncelleştirme mantığı çoğaltmak yerine üçüncü bir yöntem bu mantığı `UpdateAllSupplierAddresses`, yalnızca bu üçüncü yöntemi çağırma olay işleyicileri sahip.


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Güncelleştirme tüm düğmeleri eklendikten sonra Şekil 6 sayfası gösterilir.


[![Sayfaya iki güncelleştirme tüm düğmeler eklendi](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Şekil 6**: iki güncelleştirme tüm düğmeler sayfasına eklenmiştir ([tam boyutlu görüntüyü görüntülemek için tıklatın](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>3. adım: Tüm Üreticiler adres bilgilerini güncelleştirme

Düzenleme arabirimi görüntüleme DataList s öğelerin tümünü ile Tümünü Güncelleştir düğmeleri eklenmesi, tüm bu kalır yazılırken toplu güncelleştirmesi gerçekleştirmek için kod. Özellikle, DataList s öğeleri ve çağrı döngü ihtiyacımız `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi her biri için.

Koleksiyonu `DataListItem` DataList s DataList erişilebilir bu oluşma şekli örnekleri [ `Items` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.items.aspx). Bir başvurusu olan bir `DataListItem`, biz buna karşılık gelen yakalayın `SupplierID` gelen `DataKeys` koleksiyonu ve program aracılığıyla metin kutusuna Web denetimleri içinde başvurusu `ItemTemplate` aşağıdaki kodda gösterildiği gibi:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Kullanıcı Tümünü Güncelleştir düğmelerden birini tıklattığında `UpdateAllSupplierAddresses` yöntemi her tekrarlanan `DataListItem` içinde `Suppliers` DataList ve çağrıları `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi, karşılık gelen değerleri geçirme. Bir adresi, şehir veya ülke geçişleri için girilen olmayan bir değer değeridir `Nothing` için `UpdateSupplierAddress` (boş bir dize yerine), hangi sonuçları bir veritabanında `NULL` temel alınan kayıt s alanlar için.

> [!NOTE]
> Bir geliştirme olarak toplu güncelleştirme işlemi yapıldıktan sonra bazı onay iletisi sayfası için durum etiket Web denetimi eklemek isteyebilirsiniz.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Değiştirilmiş adresleri güncelleştiriliyor

Bu öğretici çağrıları için toplu güncelleştirme algoritması `UpdateSupplierAddress` yöntemi *her* adres bilgilerini değiştirilip bağımsız olarak DataList tedarikçi. Bu tür blind geçekleştirilen t genellikle bir performans sorunu güncelleştirirken, Denetim yapıldığı veritabanı tablosuna değişirse bunlar gereksiz kayıtları yol açabilir. Örneğin, tüm kaydetmek için Tetikleyiciler kullanırsanız `UPDATE` s `Suppliers` tabloyu denetim tabloya bir kullanıcı yeni bir denetim kaydı olup kullanıcı herhangi yapılan bağımsız olarak bu sistemdeki her sağlayıcı için oluşturulacak Tümünü Güncelleştir düğmesini tıklattığında her zaman değiştirir.

ADO.NET DataTable ve DataAdapter sınıfları, burada yalnızca değiştirilen, silinen ve yeni kayıtları sonuçları tüm veritabanı iletişiminde toplu güncelleştirmeleri destekleyecek şekilde tasarlanmıştır. DataTable her satıra sahip bir [ `RowState` özelliği](https://msdn.microsoft.com/en-us/library/system.data.datarow.rowstate.aspx) satır, değişiklik, silinecek DataTable eklendi veya değişmeden kalır olup olmadığını gösterir. DataTable başlangıçta doldurulduğunda tüm satırları değişmeden işaretlenir. Satır s sütunlara değerinin değiştirilmesi satır değiştirilmiş olarak işaretler.

İçinde `SuppliersBLL` tek tedarikçi kayıtta ilk okumada tarafından belirtilen tedarikçi s adres bilgilerini güncelleştiriyoruz sınıfı bir `SuppliersDataTable` ve ardından `Address`, `City`, ve `Country` sütun değerleri aşağıdaki kodu kullanarak:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Bu kod naively geçirilen adresi, şehir ve ülke değerlere atar `SuppliersRow` içinde `SuppliersDataTable` değerleri değişmez bağımsız olarak. Bu değişiklikleri neden `SuppliersRow` s `RowState` özelliği değiştirilmiş olarak işaretlenecek. Veri erişim katmanı s `Update` yöntemi çağrıldığında, görür `SupplierRow` değiştirilmiş ve bu nedenle gönderir bir `UPDATE` veritabanına komutu.

Ancak, biz yalnızca gelen farklıysa geçirilen adresi, şehir ve ülke değerleri atamak için bu yöntem için kod eklenen düşünün `SuppliersRow` s varolan değerleri. Adres, şehir ve ülke nerede var olan verileri ile aynı durumda hiçbir değişiklik yapılmaz ve `SupplierRow` s `RowState` olarak işaretlenmiş sol değişmez. Net sonuç olduğunda DAL s `Update` yöntemi çağrıldığında, hiçbir veritabanı çağrı yapılacak `SuppliersRow` değiştirilmedi.

Bu değişikliğini kabul etmek için doğrudan geçirilen adresi, şehir ve ülke değerleri aşağıdaki kodla atama deyimleri değiştirin:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Bu kod, DAL s eklenen `Update` yöntemi gönderir bir `UPDATE` deyimi yalnızca adresi ilgili değerleri değişti kayıtları için veritabanı.

Alternatif olarak, biz geçilen adres alanlarını ve veritabanı verileri arasındaki farklılıkları olup izlemek ve, varsa none, DAL s çağrısı yalnızca atlama `Update` yöntemi. İyi DB kullanarak yapıldığı dolaysız yöntem, DB doğrudan yöntemi olmayan t geçirilen beri bu yaklaşım çalışır bir `SuppliersRow` özelliği örneği `RowState` bir veritabanı çağrısı gerçekten gerekli olup olmadığını belirlemek için denetlenebilir.

> [!NOTE]
> Her zaman `UpdateSupplierAddress` yöntemi çağrıldığında, güncelleştirilmiş kaydı hakkında bilgi almak için veritabanına bir çağrı yapılır. Sonra verileri herhangi bir değişiklik olursa, tablo satırı güncelleştirmek için veritabanı başka bir çağrı yapılır. Bu iş akışı oluşturarak iyileştirilmiş bir `UpdateSupplierAddress` kabul yöntemi aşırı yüklemesini bir `EmployeesDataTable` sahip örnek *tüm* değişikliklerden birini `BatchUpdate.aspx` sayfa. Ardından, tüm kayıtları almak için yapılan bir çağrı veritabanına hale getirebilecek `Suppliers` tablo. İki Sonuçkümelerini sonra numaralandırılan ve burada değişiklikler kayıtları güncelleştirilemedi.


## <a name="summary"></a>Özet

Bu öğreticide hızlı bir şekilde birden çok Üreticiler adres bilgilerini değiştirmek bir kullanıcının tam olarak düzenlenebilir bir DataList oluşturma gördük. DataList s düzenleme arabirimi tedarikçi s adresi, şehir ve ülke değerleri için bir metin kutusu Web denetimi tanımlayarak başlatılan `ItemTemplate`. Ardından, üstteki ve alttaki DataList Tümünü Güncelleştir düğmeler eklendi. Bir kullanıcı kendi değişiklikler ve güncelleştirme tüm düğmeleri birini tıklattınız sonra `DataListItem` s numaralandırılan ve yapılan bir çağrı `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi yapılır.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Zack Can ve Ken Pespisa yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
[sonraki](handling-bll-and-dal-level-exceptions-cs.md)
