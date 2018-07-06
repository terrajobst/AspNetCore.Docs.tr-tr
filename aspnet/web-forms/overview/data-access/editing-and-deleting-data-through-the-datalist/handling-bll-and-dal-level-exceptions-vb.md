---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: BLL ve DAL düzeyi özel durumları (VB) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, tactfully düzenlenebilir DataList'in güncelleştirme iş akışı sırasında oluşturulan özel durumları işlemek nasıl göreceğiz.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aee5c26bfbbbc46c2151a56fed60057930cba80
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814258"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>BLL ve DAL düzeyi özel durumları (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) veya [PDF olarak indirin](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> Bu öğreticide, tactfully düzenlenebilir DataList'in güncelleştirme iş akışı sırasında oluşturulan özel durumları işlemek nasıl göreceğiz.


## <a name="introduction"></a>Giriş

İçinde [genel bakış, düzenleme ve silme DataList'te verileri](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) öğreticide oluşturduğumuz basit düzenleme ve silme özelliklerini sunulan bir DataList. Tam olarak işlevsel, düzenleme sırasında gerçekleşen herhangi bir hata olarak pek kullanışlı veya silme işlemi içinde işlenmeyen bir özel sonuçlandı. Örneğin, ürün s adını atlama veya bir ürün düzenlerken çok fiyat değerinin girilmesi ekonomik!, bir özel durum oluşturur. Kodda özel durum yakalandı olduğundan, ardından web sayfasındaki s özel durum ayrıntıları görüntüler ASP.NET çalışma zamanı kadar Balonlar.

İçinde gördüğümüz gibi [işleme BLL ve DAL düzeyi özel durumları bir ASP.NET sayfasında](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Öğreticisi, iş mantığı ve veri erişim katmanları derinlikleri bir özel durum oluşursa özel durum ayrıntıları döndürülür ObjectDataSource için ve ardından GridView için. Düzgün bir şekilde oluşturarak bu özel durumları işlemek nasıl gördüğümüz `Updated` veya `RowUpdated` ObjectDataSource veya bir özel durum için denetimi ve ardından özel durumu işlenmiş gösteren GridView için olay işleyicileri.

DataList öğreticilerimizden, ancak dahilse t verileri güncelleştirme ve silme için ObjectDataSource kullanma. Bunun yerine, doğrudan karşı BLL çalışıyoruz. BLL veya DAL ağlardan kaynaklanan özel durumlarını algılamak için özel durum işleme kodunu ASP.NET sayfamızı plan kod içinde uygulamak gerekiyor. Bu öğreticide daha tactfully iş akışını güncelleştirmek düzenlenebilir DataList s sırasında oluşturulan özel durumları işlemek nasıl göreceğiz.

> [!NOTE]
> İçinde *, bir genel bakış düzenleme ve silme DataList'te verileri* Öğreticisi, düzenleme ve DataList verileri silmek için farklı tekniklerini ele aldığımız bazı teknikleri söz konusu güncelleştirmek için bir ObjectDataSource kullanma ve siliniyor. Bu teknikler kullanmak istemiyorsunuz, ObjectDataSource s aracılığıyla BLL ve DAL ait özel durumları işleyebilir `Updated` veya `Deleted` olay işleyicileri.


## <a name="step-1-creating-an-editable-datalist"></a>1. adım: düzenlenebilir bir DataList oluşturma

Güncelleştirme iş akışı sırasında oluşan özel durumları işleme hakkında endişe önce ilk düzenlenebilir bir DataList oluşturun s olanak tanır. Açık `ErrorHandling.aspx` sayfasını `EditDeleteDataList` DataList, tasarımcıya eklemek klasörü ayarlayın, `ID` özelliğini `Products`, adlı yeni bir ObjectDataSource ekleyin `ProductsDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProducts()` yöntemi seçme kaydeder; INSERT, UPDATE, açılan listeler ayarlama ve silme (hiçbiri) için sekmeler.


[![GetProducts() yöntemi kullanılarak ürün bilgilerini döndürür](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Şekil 1**: ürün bilgileri kullanarak dönüş `GetProducts()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


ObjectDataSource Sihirbazı tamamladıktan sonra Visual Studio otomatik olarak oluşturacak bir `ItemTemplate` DataList için. Bununla değiştirin bir `ItemTemplate` her ürün s ad ve fiyat görüntüler ve Düzenle düğmesine içerir. Ardından, oluşturun bir `EditItemTemplate` bir TextBox Web denetimi için ad ve fiyat ve güncelleştir ve İptal düğmeleri. Son olarak, s DataList ayarlamak `RepeatColumns` özelliği 2.

Bu değişikliklerden sonra sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir. Düzenleme, iptal, emin olmak için denetleyin ve güncelleştirmeyi düğmeleri kendi `CommandName` düzenlemek, iptal etmek ve sırasıyla güncelleştirmek için özelliklerini ayarlayın.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Bu öğreticide DataList s Görünüm durumunun etkinleştirilmesi gerekir.


Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın (bkz: Şekil 2).


[![Her ürünün bir Düzenle düğmesi içerir.](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Şekil 2**: her ürünün bir Düzenle düğmesi içerir ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Şu anda, Düzenle düğmesini yalnızca bir geri göndermenin neden olur, eklenmemişse t henüz olun ürün düzenlenebilir. Düzenlemeyi etkinleştirmek için s DataList için olay işleyicilerini oluşturma ihtiyacımız `EditCommand`, `CancelCommand`, ve `UpdateCommand` olayları. `EditCommand` Ve `CancelCommand` olayları yalnızca güncelleştirme s DataList `EditItemIndex` özelliği ve yeniden bağlamasını DataList verileri:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

`UpdateCommand` Biraz daha karmaşık olay işleyicisi. Düzenlenen üründe s okumak gereken `ProductID` gelen `DataKeys` koleksiyonun s ürün adını ve metin kutuları içinde fiyatın yanı sıra `EditItemTemplate`ve sonra çağrı `ProductsBLL` s sınıfı `UpdateProduct` DataList döndürmeden önce yöntemi önceden düzenleme durumuna.

Şimdilik, let s yalnızca tam aynı koddan kullanın `UpdateCommand` olay işleyicisinde *genel bakış, düzenleme ve silme DataList'te verileri* öğretici. 2. adımda özel durumlar düzgün bir şekilde işlemek üzere kod ekleyeceğiz.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

Geçersiz giriş karşılaşıldığında, bir Düzensiz biçimlendirilmiş bir birim fiyatı, bir geçersiz birim fiyat değeri gibi-$5.00 veya bir özel durum ürün adı Java'daki biçiminde olabilir. Bu yana `UpdateCommand` olay işleyicisi bir özel durum işleme kodunu bu noktada içermez, özel durum kadar ASP.NET çalışma zamanı, son kullanıcıya uygulamanın nerede görüntülenecek Kabarcık (bkz: Şekil 3).


![İşlenmeyen bir özel durum oluştuğunda, son kullanıcıya bir hata sayfası görür.](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Şekil 3**: işlenmeyen bir özel durum oluştuğunda, son kullanıcıya bir hata sayfası görür.


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>2. adım: Düzgün bir şekilde UpdateCommand olay işleyicisi özel durumları işleme

Özel durumlar oluşabilir güncelleştirme iş akışı sırasında `UpdateCommand` olay işleyicisi, BLL ve DAL. Örneğin, kullanıcının girdiği fiyatı çok pahalı `Decimal.Parse` deyiminde `UpdateCommand` olay işleyicisi oluşturur bir `FormatException` özel durum. Kullanıcı, ürün adı çıkarırsa veya fiyat negatif bir değere sahipse, DAL bir özel durum oluşturacak.

Bir özel durum oluştuğunda, sayfa içinde bilgilendirici bir ileti görüntülemek istiyoruz. Bir etiket Web ekleme sayfa için ayarlanmış denetleme `ID` ayarlanır `ExceptionDetails`. Bir kırmızı, çok büyük atayarak kalın ve italik yazı tipi görüntülenecek etiket s metin yapılandırmak, `CssClass` özelliğini `Warning` tanımlanan CSS sınıfını `Styles.css` dosya.

Bir hata oluştuğunda, yalnızca bir kez görüntülenecek etiket istiyoruz. Diğer bir deyişle, sonraki Geri göndermeler üzerinde etiket s uyarı iletisi ortadan kalkması gerekir. Bu iki etiket s temizleyerek gerçekleştirilebilir `Text` özelliği veya ayarları kendi `Visible` özelliğini `False` içinde `Page_Load` olay işleyicisi (geri yaptığımız gibi [işleme BLL ve DAL düzeyi özel durumları bir ASP .NET sayfası](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) öğretici) veya etiket s görünüm durumu desteği devre dışı bırakılıyor. İkinci seçeneği kullanın s olanak tanır.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Bir özel durum oluştuğunda, özel durum ayrıntılarını atamaya başlayacaksınız `ExceptionDetails` etiket denetimini s `Text` özelliği. Görünüm durumunu, sonraki Geri göndermeler üzerinde devre dışı bırakıldığından `Text` s özelliği programlı değişiklikler kaybolacak, böylece uyarı iletisi gizleme geri varsayılan metni (boş bir dize) döndürülüyor.

Sayfada yararlı bir ileti görüntülemek için bir hata olduğunda yükseltildikten belirlemek için eklemek ihtiyacımız bir `Try ... Catch` bloğunu `UpdateCommand` olay işleyicisi. `Try` Bölümü, bir özel durum açabilir kodunu içerir ancak `Catch` karşılaşıldığında bir özel durum yürütülen kod bloğu içerir. Kullanıma [özel durum işleme Temelleri](https://msdn.microsoft.com/library/2w8f0bss.aspx) bölümünde daha fazla bilgi için .NET Framework belgelerinde üzerinde `Try ... Catch` blok.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Herhangi bir türde bir özel durum harekete geçirildiğinde içindeki kod tarafından `Try` bloğu `Catch` blok s kod yürütülürken başlayacak. Oluşturulan özel durum türünü `DbException`, `NoNullAllowedException`, `ArgumentException`ve vb. ne üzerinde tam olarak bağlıdır hatayla ilk başta precipitated. Varsa bir sorun s vardır ve veritabanı düzeyinde bir `DbException` oluşturulur. İçin geçersiz bir değer girilirse `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, veya `ReorderLevel` alanlar, bir `ArgumentException` Bu alan değerlerini doğrulamak için kod ekledik gibi oluşturulacaktır `ProductsDataTable` sınıfı (bkz [ Bir iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-vb.md) öğretici).

Biz, son kullanıcıya daha yararlı bir açıklama özel durum yakalandı türüne göre ileti metni dayandırmamaya özen sağlayabilir. Neredeyse aynı formda kullanılan aşağıdaki kodu geri [işleme BLL ve DAL düzeyi özel durumları bir ASP.NET sayfasında](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) öğretici, bu düzeyde ayrıntı sağlar:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Bu öğreticiyi tamamlamak için yalnızca çağrı `DisplayExceptionDetails` yönteminden `Catch` bloğu yakalanan geçirme `Exception` örneği (`ex`).

İle `Try ... Catch` yerinde engelleme, Şekil 4 ve 5 Göster, kullanıcıların daha bilgilendirici bir hata iletisi ile sunulur. Düzenleme modunda karşılaşıldığında bir özel durum DataList kalan unutmayın. Özel durum oluştuğunda sonra denetim akışı hemen yönlendireceği olmasıdır `Catch` atlayarak DataList önceden düzenleme durumuna geri döner kod bloğu.


[![Bir kullanıcı gerekli bir alan çıkarırsa bir hata iletisi görüntülenir](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Şekil 4**: bir kullanıcı gerekli bir alan çıkarırsa bir hata iletisi görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Bir hata iletisi görüntülenir, girilmesi negatif bir fiyat olur](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Şekil 5**: bir hata iletisi şudur görüntülenen zaman girme negatif bir fiyat ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Özet

GridView ve ObjectDataSource herhangi güncelleştirme ve silme iş akışı sırasında ortaya çıktı özel durumların yanı sıra özel durumu kaldırıldı olup olmadığını belirtmek için ayarlanabilir özellikleri hakkında bilgi içeren sonrası düzeyi olay işleyicileri sağlar İşlenmiş. Bu özellikler, ancak kullanılamaz DataList'i ile çalışma ve BLL kullanarak doğrudan. Bunun yerine, özel durum işleme uygulamak için sorumlu duyuyoruz.

Bu öğreticide düzenlenebilir bir DataList s ekleyerek iş akışını güncelleştirmek için özel durum işleme ekleme gördüğümüz bir `Try ... Catch` bloğunu `UpdateCommand` olay işleyicisi. Güncelleştirme iş akışı sırasında bir özel durum oluşturulursa `Catch` blok s kodu yürütür, yararlı bilgileri görüntüleme `ExceptionDetails` etiketi.

Bu noktada, DataList ilk başta karşılaşmamak özel durumları engellemek için çaba göstermez. Biliyoruz negatif bir fiyat bir özel durum neden olur, biz haven rağmen t proaktif olarak bir kullanıcı bu tür geçersiz giriş girmesini önlemek için herhangi bir işlevsellik henüz eklendi. Sonraki müşterilerimize öğreticide tarafından geçersiz kullanıcı girişini doğrulama denetimleri ekleyerek kaynaklı özel durumların azaltmaya yardımcı olmak nasıl görüyoruz `EditItemTemplate`.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Özel Durumlar için Tasarım Yönergeleri](https://msdn.microsoft.com/library/ms298399.aspx)
- [Hata günlüğü modüller ve işleyiciler (ELMAH)](http://workspaces.gotdotnet.com/elmah) (günlük kaydı hataları için bir açık kaynak kitaplığı)
- [Kurumsal kitaplığı için .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (özel durum yönetimi uygulama bloğu içerir)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı Gözden Geçiren, Ken Pespisa oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](performing-batch-updates-vb.md)
> [İleri](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
