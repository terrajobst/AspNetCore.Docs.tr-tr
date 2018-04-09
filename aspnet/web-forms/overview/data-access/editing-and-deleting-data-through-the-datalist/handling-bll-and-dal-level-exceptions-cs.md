---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: BLL ve DAL düzeyi özel durumları (C#) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, tactfully düzenlenebilir bir DataList'ın güncelleştirme iş akışı sırasında oluşturulan özel durumları işleme göreceğiz.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: fd88260e1ac941b053aae8b4e500a7ab5f3091a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>BLL ve DAL düzeyi özel durumları (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) veya [PDF indirin](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> Bu öğreticide, tactfully düzenlenebilir bir DataList'ın güncelleştirme iş akışı sırasında oluşturulan özel durumları işleme göreceğiz.


## <a name="introduction"></a>Giriş

İçinde [genel bakış düzenlemesini ve DataList verileri silme](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) öğretici, oluşturduğumuz basit düzenleme ve silme özellikleri sunulan bir DataList. Tam olarak işlevsel durumdayken düzenleme sırasında oluşan herhangi bir hata olarak pek kolay veya silme işlemi işlenmeyen bir özel durumla sonuçlandı. Örneğin, ürün s adını atlama veya, bir ürün düzenlerken, çok fiyat değerini girerek uygun maliyetli!, bir özel durum oluşturur. Kodda bu özel durum yakalandı olduğundan, ardından web sayfasında s özel durum ayrıntıları görüntüler ASP.NET çalışma zamanı kadar köpürür.

İçinde gördüğümüz gibi [işleme BLL - ve ASP.NET sayfası DAL düzeyi durumlar](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) öğretici, iş mantığı ya da veri erişim katmanları derinliklerine özel durum oluşturuldu özel durum ayrıntıları döndürülür ObjectDataSource ve ardından GridView için. Düzgün biçimde oluşturarak bu özel durumları işleme gördüğümüz `Updated` veya `RowUpdated` ObjectDataSource veya için bir özel durum denetimi ve özel durumun işlendiğini gösteren GridView, olay işleyicileri.

DataList öğreticilerimizi, ancak geçekleştirilen t ObjectDataSource güncelleştirmek ve verileri silme için kullanma. Bunun yerine, doğrudan BLL karşı çalışıyoruz. Özel durumlar BLL veya DAL kaynaklanan algılamak için özel durum kodu ASP.NET sayfamızı plan kod içinde işleme uygulamak ihtiyacımız. Bu öğreticide daha tactfully iş akışı güncelleştiriliyor düzenlenebilir DataList s sırasında oluşturulan özel durumları işleme göreceğiz.

> [!NOTE]
> İçinde *, bir genel bakış düzenleme ve silme verileri DataList* öğretici, düzenleme ve DataList veri silmek için farklı teknikleri biz ele alınan bazı teknikleri söz konusu güncelleştirmek için bir ObjectDataSource kullanarak ve siliniyor. Bu teknikler işe alırsanız ObjectDataSource s'nda özel durumlara BLL veya DAL işleyebilir `Updated` veya `Deleted` olay işleyicileri.


## <a name="step-1-creating-an-editable-datalist"></a>1. adım: bir düzenlenebilir DataList oluşturma

Güncelleştirme iş akışı sırasında oluşan özel durum işleme hakkında endişelenmeniz önce ilk düzenlenebilir bir DataList oluşturmak s olanak tanır. Açık `ErrorHandling.aspx` sayfasındaki `EditDeleteDataList` klasörünü kümesi DataList Designer'a Ekle kendi `ID` özelliğine `Products`, ve adlı yeni bir ObjectDataSource ekleme `ProductsDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProducts()` seçme yöntemini kaydeder; INSERT, UPDATE, aşağı açılır listeler ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![GetProducts() yöntemi kullanılarak ürün bilgilerini döndürür](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Şekil 1**: ürün bilgileri kullanarak dönmek `GetProducts()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


ObjectDataSource Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik olarak oluşturacak bir `ItemTemplate` DataList için. Bunu yerine bir `ItemTemplate` , her ürün s adı ve fiyat görüntüler ve bir Düzenle düğmesi içerir. Ardından, oluşturun bir `EditItemTemplate` adını ve fiyat güncelleştir ve İptal düğmeleri için metin kutusuna Web denetimi ile. Son olarak, s DataList ayarlamak `RepeatColumns` özelliğini 2.

Bu değişikliklerden sonra sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir. Düzenleme, iptal, emin olmak için bir kez daha denetleyin ve güncelleştirme düğmeleri kendi `CommandName` düzenlemek, iptal etmek ve sırasıyla güncelleştirmek için özellikleri ayarlayın.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> Bu öğretici için DataList s Görünüm durumunun etkinleştirilmesi gerekir.


Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın (bkz: Şekil 2).


[![Her ürünün bir Düzenle düğmesi içerir](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Şekil 2**: her ürünün bir Düzenle düğmesi içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


Şu anda Düzenle düğmesi yalnızca geri gönderimin neden olur, mevcut değil t henüz olun ürün düzenlenebilir. Düzenleme etkinleştirmek için olay işleyicileri DataList s oluşturmak ihtiyacımız `EditCommand`, `CancelCommand`, ve `UpdateCommand` olaylar. `EditCommand` Ve `CancelCommand` yalnızca update s DataList events `EditItemIndex` özelliği ve DataList verileri yeniden bağlayın:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

`UpdateCommand` Olay işleyicisi biraz daha karmaşık. Düzenlenen ürün s okumak gereken `ProductID` gelen `DataKeys` koleksiyonu s ürün adı ve kutularındaki fiyatından yanı sıra `EditItemTemplate`ve ardından arama `ProductsBLL` s sınıfı `UpdateProduct` DataList dönmeden önce yöntemi önceden düzenleme durumuna.

Şimdilik, let s yalnızca tam aynı koddan kullanmak `UpdateCommand` olay işleyicisini *genel bakış düzenlemesini ve DataList verileri silme* Öğreticisi. 2. adımda özel durumları düzgün bir şekilde işlemek üzere kod ekleyeceğiz.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

Geçersiz giriş karşısında hangi düzgün biçimlendirilmemiş birim fiyat, bir geçersiz birim fiyat değeri gibi-$5.00 ya da özel durum oluşturuldu ürün s adı atlandığını biçiminde olabilir. Bu yana `UpdateCommand` olay işleyicisi hiçbir özel durum kodu bu noktada işleme içermez, özel durum ASP.NET çalışma zamanı en son kullanıcıya burada görüntülenir Kabarcık (bkz: Şekil 3).


![İşlenmeyen bir özel durum oluştuğunda, son kullanıcı bir hata sayfası görür.](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Şekil 3**: işlenmeyen bir özel durum oluştuğunda, son kullanıcı bir hata sayfası görür.


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>2. adım: Düzgün biçimde UpdateCommand olay işleyicisi özel durumları işleme

Özel durumlar oluşabilir güncelleştirme iş akışı sırasında `UpdateCommand` olay işleyicisi, BLL veya DAL. Örneğin, bir kullanıcı çok fiyatı girerse pahalı, `Decimal.Parse` deyiminde `UpdateCommand` olay işleyicisi oluşturur bir `FormatException` özel durum. Ürün adı kullanıcı çıkarırsa veya fiyat negatif bir değer varsa, DAL bir özel durum oluşturacak.

Özel durum oluştuğunda bilgilendirici bir ileti sayfasında görüntülenecek istiyoruz. Bir etiket Web ekleme özelliği sayfasına denetim `ID` ayarlanır `ExceptionDetails`. Atayarak kalın ve italik yazı tipini bir kırmızı, çok büyük görüntülenecek etiket s metin yapılandırmak, `CssClass` özelliğine `Warning` tanımlanan CSS sınıfı `Styles.css` dosya.

Hata oluştuğunda, yalnızca bir kez görüntülenecek etiket istiyoruz. Diğer bir deyişle, sonraki Geri göndermeler üzerinde etiket s uyarı iletisi kaybolur. Bu da s etiket temizleyerek gerçekleştirilebilir `Text` özelliği veya ayarları kendi `Visible` özelliğine `False` içinde `Page_Load` olay işleyicisi (geri yaptığımız gibi [işleme BLL - ve bir ASP DAL düzeyi özel durumları .NET sayfası](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) öğretici) veya etiket s görünüm durumu desteğini devre dışı bırakma. İkinci seçeneği kullanın s olanak tanır.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Bir özel durum oluştuğunda, özel durumu ayrıntılarını atamanız `ExceptionDetails` etiket denetimini s `Text` özelliği. Görünüm durumunu, sonraki Geri göndermeler üzerinde devre dışı olduğundan `Text` özellik s programlı değişiklikler kaybolur, böylece uyarı iletisi gizleme geri varsayılan metni (boş bir dize), döndürülüyor.

Sayfada yararlı bir ileti görüntülemek için bir hata olduğunda yükseltildiğinde belirlemek için eklemek ihtiyacımız bir `Try ... Catch` için engelleyin `UpdateCommand` olay işleyicisi. `Try` Bölümü bir özel durum açabilir kodunu içerir ancak `Catch` bloğu karşısında bir özel durum yürütülen kodu içerir. Kullanıma [özel durum işleme Temelleri](https://msdn.microsoft.com/library/2w8f0bss.aspx) üzerinde bölümünde daha fazla bilgi için .NET Framework belgelerinde `Try ... Catch` bloğu.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Ne zaman herhangi bir türde özel durum kodu içinde tarafından `Try` bloğu `Catch` blok s kodu yürütme başlayacak. Oluşturulan özel durumun türünü `DbException`, `NoNullAllowedException`, `ArgumentException`ve benzeri ne üzerinde tam olarak bağlıdır hata ilk başta precipitated. Varsa burada s bir sorun veritabanı düzeyinde bir `DbException` oluşturulur. İçin geçersiz bir değer girdiyseniz `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, veya `ReorderLevel` alanları, bir `ArgumentException` Bu alan değerlerini doğrulamak için kod eklediğimiz gibi durum oluşturulacak `ProductsDataTable` sınıfı (bkz: [ Bir iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-cs.md) öğretici).

İleti metni özel durumu yakalandı türüne alma tarafından size daha yararlı bir açıklama son kullanıcıya sağlayabilir. Neredeyse aynı formda kullanılan aşağıdaki kod geri [işleme BLL - ve ASP.NET sayfası DAL düzeyi durumlar](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Eğitmeni bu düzeyde ayrıntı sağlar:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Bu öğreticiyi tamamlamak için yalnızca çağrısı `DisplayExceptionDetails` yönteminden `Catch` yakalanan geçirme blok `Exception` örneği (`ex`).

İle `Try ... Catch` yerinde engellemek, Şekil 4 ve 5 Göster kullanıcıları daha bilgilendirici bir hata iletisi ile sunulur. Bir özel durum DataList karşısında kalan unutmayın düzenleme modunda. Özel durum oluştu sonra denetim akışı hemen yönlendirilir olmasıdır `Catch` DataList önceden düzenleme durumuna geri döner kod atlayarak bloğu.


[![Gerekli bir alan bir kullanıcı çıkarırsa bir hata iletisi görüntülenir](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Şekil 4**: gerekli bir alan bir kullanıcı çıkarırsa bir hata iletisi görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![Bir hata iletisi görüntülenen zaman girilmesi negatif bir fiyat olur](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Şekil 5**: bir hata iletisi şudur görüntülenen zaman girme negatif bir fiyat ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>Özet

ObjectDataSource ve GridView herhangi güncelleştirme ve silme iş akışı sırasında ortaya çıktı özel durumlar olarak özel durum işlenmişse olup olmadığını belirtmek için ayarlayabileceğiniz özellikler hakkında bilgi içeren sonrası düzeyi olay işleyicileri sağlar İşlenmiş. Bu özellikler, ancak kullanılamaz DataList ile çalışma ve BLL kullanarak doğrudan. Bunun yerine, özel durum işleme uygulamak için sorumlu duyuyoruz.

Özel durum işleme iş akışı ekleyerek güncelleştirme düzenlenebilir bir DataList s ekleme gördüğümüz Bu öğreticide bir `Try ... Catch` için engelleyin `UpdateCommand` olay işleyicisi. Güncelleştirme iş akışı sırasında bir özel durum oluşursa `Catch` blok s kodu yürütür, yararlı bilgileri görüntüleme `ExceptionDetails` etiketi.

Bu noktada, ilk başta gerçekleştiği gelen özel durumlarını önlemek için hiçbir çaba DataList yapar. Biliyoruz negatif bir fiyat bir özel durum neden olur, biz başlattıysanız rağmen t proaktif olarak bir kullanıcı bu tür geçersiz giriş girmesini engellemek için herhangi bir işlevsellik henüz eklendi. Bizim sonraki öğreticide tarafından geçersiz kullanıcı girişini doğrulama denetimleri ekleyerek neden özel durumlarını azaltılmasına yardımcı olmak nasıl göreceğiz `EditItemTemplate`.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Özel Durumlar için Tasarım Yönergeleri](https://msdn.microsoft.com/library/ms298399.aspx)
- [Hata günlük modülleri ve işleyicileri (ELMAH)](http://workspaces.gotdotnet.com/elmah) (günlük kaydı hataları için bir açık kaynak kitaplığı)
- [.NET Framework 2.0 için Kurumsal Kitaplığı](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (özel durum yönetimi uygulama bloğu içerir)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Ken Pespisa oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](performing-batch-updates-cs.md)
> [sonraki](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
