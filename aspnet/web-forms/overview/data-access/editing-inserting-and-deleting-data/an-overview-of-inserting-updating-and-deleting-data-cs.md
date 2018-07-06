---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Genel Bakış ekleme, güncelleştirme ve silme (C#) veri | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bir ObjectDataSource INSERT(), Update(), eşleme görüyoruz ve sınıfları BLL yöntemlerini Delete() yöntemlerine yapılan nasıl yanı sıra...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: abe42d01cc31ea46c97888f31d768ebfede64abd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821708"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Genel Bakış ekleme, güncelleştirme ve silme veri (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) veya [PDF olarak indirin](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> Bu öğreticide bir ObjectDataSource INSERT(), Update(), eşleme görüyoruz ve BLL yöntemlerinin Delete() yöntemlere sınıfları, veri değişikliği yetenekleri sağlamak üzere FormView GridView ve DetailsView denetimlerini yapılandırmak nasıl yanı sıra.


## <a name="introduction"></a>Giriş

Geçen birkaç öğreticiler size FormView GridView ve DetailsView denetimlerini kullanarak bir ASP.NET sayfasında verileri görüntülemek nasıl anlatılmaktadır. Bu denetimler, yalnızca kendisine sağlanan verilerle çalışın. Yaygın olarak, bu denetimlerin ObjectDataSource gibi bir veri kaynağı denetimi verilerini erişin. ASP.NET sayfası ve temel alınan verileri arasında bir proxy olarak ObjectDataSource nasıl ele alacağını gördük. GridView verileri görüntülemek gerektiğinde, kendi ObjectDataSource çağırdığı `Select()` karşılık gelen bizim iş mantığı katmanı (uygun veri erişim katmanının içinde (DAL) bir yöntemi çağıran BLL), bir yöntemi çağıran, yöntemi sırayla gönderen TableAdapter bir `SELECT` Northwind veritabanına sorgu.

Biz TableAdapters içinde DAL oluşturduğunuzda, geri çağırma [ilk öğreticimize](../introduction/creating-a-data-access-layer-cs.md), Visual Studio otomatik olarak güncelleştirme, ekleme, yöntemleri eklenir ve arka plandaki silme verileri veritabanı tablo. Ayrıca, içinde [iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-cs.md) adlı BLL yöntemleri bu veri değişikliği DAL yöntemleri tasarladığımız.

Ek olarak kendi `Select()` yöntemi ObjectDataSource de sahip `Insert()`, `Update()`, ve `Delete()` yöntemleri. Gibi `Select()` yöntemi, temel alınan nesnede yöntemleri için bu üç yöntem eşlenebilir. FormView GridView ve DetailsView denetimlerini eklemek, güncelleştirmek veya verileri silmek için yapılandırıldığında, temel alınan verileri değiştirmek için bir kullanıcı arabirimi sağlar. Bu kullanıcı arabirimini çağıran `Insert()`, `Update()`, ve `Delete()` ardından temel alınan nesnede çağıran ObjectDataSource yöntemlerinin (bkz. Şekil 1) yöntemleri ilişkili.


[![ObjectDataSource INSERT() Update() ve Delete() yöntemleri BLL bir Proxy olarak hizmet](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Şekil 1**: ObjectDataSource `Insert()`, `Update()`, ve `Delete()` yöntemleri BLL içine bir Proxy olarak hizmet veren ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


Bu öğreticide nasıl eşleneceğini ObjectDataSource görüyoruz `Insert()`, `Update()`, ve `Delete()` BLL yanı sıra veri değişikliği sağlamak FormView GridView ve DetailsView denetimlerini yapılandırmak nasıl sınıfların yöntemlerinde yöntemleri özellikleri.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>1. adım: ekleme, güncelleştirme ve silme öğreticiler Web sayfaları oluşturma

Ekleme, güncelleştirme ve verileri silmek nasıl araştırma başlamadan önce ilk yapmamız gereken Bu öğretici ve sonraki birkaç olanlar için Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir zaman ayırabiliriz. Başlangıç adlı yeni bir klasör ekleyerek `EditInsertDelete`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Veri değişikliği ilgili öğreticiler için ASP.NET sayfaları ekleme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Şekil 2**: ASP.NET sayfaları için veri değişikliği ilgili öğreticiler Ekle


Diğer klasörler gibi `Default.aspx` içinde `EditInsertDelete` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfanın Tasarım görünümü Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Şekil 3**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Son olarak, girişleri olarak istediğiniz sayfaları eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme özelleştirilmiş biçimlendirme sonrasında eklemeniz `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık düzenleme, ekleme ve silme öğreticiler için öğeleri içerir.


![Site Haritası artık düzenleme, ekleme ve silme öğreticiler için girişler içeriyor](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Şekil 4**: Site Haritası artık düzenleme, ekleme ve silme öğreticiler için girişler içeriyor


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>2. adım: Ekleme ve ObjectDataSource Denetimi yapılandırma

GridView DetailsView ve her farklı kendi veri değişikliği özellikleri ve düzeni FormView itibaren her biri ayrı ayrı inceleyelim. Her denetim, kendi ObjectDataSource kullanarak yaptırmak yerine, ancak yalnızca tüm üç denetim örnekleri paylaşabilirsiniz tek bir ObjectDataSource oluşturalım.

Açık `Basics.aspx` sayfasında bir ObjectDataSource tasarımcı araç kutusundan sürükleyin ve akıllı etiketinde veri kaynağı yapılandırma bağlantısına tıklayın. Bu yana `ProductsBLL` düzenleme, ekleme ve silme yöntemleri, bu sınıfı kullanan ObjectDataSource yapılandırma sağlayan tek BLL sınıftır.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Şekil 5**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Sonraki ekranda hangi yöntemlerinin biz belirtebilirsiniz `ProductsBLL` sınıfı ObjectDataSource eşlendi `Select()`, `Insert()`, `Update()`, ve `Delete()` uygun sekmeyi seçip, aşağı açılan listeden yöntemi seçme. Artık tanıdık gelecektir, ObjectDataSource eşler Şekil 6 `Select()` yönteme `ProductsBLL` sınıfın `GetProducts()` yöntemi. `Insert()`, `Update()`, Ve `Delete()` üstünde listeden uygun sekmeyi seçerek yöntemleri yapılandırılabilir.


[![Sahip ObjectDataSource dönüş tüm ürünleri](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Şekil 6**: ObjectDataSource dönüş tüm ürünlerin sahip ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Şekil 7, 8 ve 9 ObjectDataSource UPDATE, INSERT ve DELETE Göster sekmeler. Bu sekmeler yapılandırma böylece `Insert()`, `Update()`, ve `Delete()` yöntemleri çağırma `ProductsBLL` sınıfın `UpdateProduct`, `AddProduct`, ve `DeleteProduct` yöntemleri, sırasıyla.


[![Map ProductBLL sınıfın UpdateProduct yöntemi için ObjectDataSource Update() yöntemi](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Şekil 7**: ObjectDataSource harita `Update()` yönteme `ProductBLL` sınıfın `UpdateProduct` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Map ProductBLL sınıfın AddProduct yönteme ObjectDataSource INSERT() yöntemi](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Şekil 8**: ObjectDataSource harita `Insert()` yönteme `ProductBLL` sınıfın Ekle `Product` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Map ProductBLL sınıfın DeleteProduct yönteme ObjectDataSource Delete() yöntemi](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Şekil 9**: ObjectDataSource harita `Delete()` yönteme `ProductBLL` sınıfın `DeleteProduct` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


UPDATE, INSERT ve DELETE sekmeleri açılan listelerde bu yöntemler seçili olduğunu fark etmiş olabilirsiniz. Bu bizim sayesinde kullanımıdır `DataObjectMethodAttribute` yöntemlerinin düzenler `ProducstBLL`. Örneğin, aşağıdaki imzası DeleteProduct yöntemi vardır:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

`DataObjectMethodAttribute` Öznitelik, her bir yöntemin amacı seçme, ekleme, güncelleştirme veya silme ve olsun veya olmasın, varsayılan değer: olup olmadığını gösterir. BLL sınıflarınızı oluştururken bu öznitelikler atlanırsa, el ile yöntemleri UPDATE'ten seçmek için Ekle ve sekmeleri SİLİN.

Uygun olduktan sonra `ProductsBLL` yöntemleri ObjectDataSource eşlendi `Insert()`, `Update()`, ve `Delete()` yöntemleri, Sihirbazı tamamlamak için Son'u tıklatın.

## <a name="examining-the-objectdatasources-markup"></a>ObjectDataSource biçimlendirme İnceleme

ObjectDataSource kendi Sihirbazı'nı yapılandırdıktan sonra oluşturulan bildirim temelli biçimlendirme incelemek için kaynak görünümüne gidin. `<asp:ObjectDataSource>` Nesnesini ve yöntemlerini çağırmak için etiketini belirtir. Ayrıca, vardır `DeleteParameters`, `UpdateParameters`, ve `InsertParameters` , eşleme için giriş parametrelerini `ProductsBLL` sınıfın `AddProduct`, `UpdateProduct`, ve `DeleteProduct` yöntemleri:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource parametre her girdi parametrelerinin listesi gibi kendi ilişkili yöntemleri içerir. `SelectParameter` s değerinin giriş parametresi bekliyor seçin bir yöntemi çağırmak için ObjectDataSource yapılandırıldığında var ( gibi`GetProductsByCategoryID(categoryID)`). Kısa bir süre sonra bu değerleri anlatıldığı `DeleteParameters`, `UpdateParameters`, ve `InsertParameters` otomatik olarak GridView DetailsView ve FormView ile ObjectDataSource çağırmadan önce ayarlanır `Insert()`, `Update()`, veya `Delete()` yöntem. Bir sonraki öğreticide açıklayacağız olarak bu değerleri de gerektiğinde, programlı olarak ayarlanabilir.

ObjectDataSource için Yapılandırma Sihirbazı'nı kullanarak bir yan etkisi olan Visual Studio ayarlar [ObjectDataSource'taki özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) için `original_{0}`. Bu özellik değeri, düzenlenmekte olan veri öğesinin özgün değerleri eklemek için kullanılır ve iki senaryolarda kullanışlıdır:

- Bir kaydın düzenlenmesi, kullanıcıların birincil anahtar değerini değiştirme olanağına sahip olursunuz Böylece özgün birincil anahtar değeri ile kayıt bulunamadı ve buna uygun olarak güncelleştirildi, değerini, bu durumda, yeni birincil anahtar değeri hem özgün birincil anahtar değeri sağlanmalıdır.
- İyimser eşzamanlılık kullanırken. İyimser eşzamanlılık iki emin olmak için bir tekniktir eşzamanlı kullanıcıların başka birinin değişikliklerin üzerine yazmayın ve konu için gelecekteki bir öğreticidir.

`OldValuesParameterFormatString` Özelliği, temel alınan nesnenin güncelleştirme ve silme yöntemleri orijinal değerleri için giriş parametrelerini adını belirtir. İyimser eşzamanlılık inceleyeceğiz olduğunda bu özellik ve daha ayrıntılı amacını açıklayacağız. Ben artık, ancak bizim BLL'ın yöntemleri orijinal değerleri beklenmez ve bu nedenle bu özelliği kaldırın, önemli olduğundan getirin. Bırakarak `OldValuesParameterFormatString` varsayılan dışında bir özelliği (`{0}`) ObjectDataSource çağırmak bir veri Web denetimi çalıştığında bir hata neden olacak `Update()` veya `Delete()` yöntemleri ObjectDataSource olur çünkü her ikisinde de geçirilmeye çalışıldı `UpdateParameters` veya `DeleteParameters` yanı sıra özgün değer parametreleri belirtildi.

Bu juncture bu en çok açık değilse, endişelenmeyin, bu özellik ve onun yardımcı programı bir sonraki öğreticide inceleyeceğiz. Şu an için yalnızca bu özellik bildiriminde tamamen bildirim temelli söz dizimi kaldırın ya da değeri varsayılan değere ayarlayın emin olun ({0}).

> [!NOTE]
> Yalnızca silerseniz `OldValuesParameterFormatString` Tasarım görünümünde, özelliği Özellikler penceresinden özellik değeri, bildirim temelli söz diziminde var olmaya devam edecek, ancak boş bir dize olarak ayarlanmış. Bu, ne yazık ki, yine de aynı sorun yukarıda açıklanan sonuçlanır. Bu nedenle, kaldırabilir ya da özelliği tamamen bildirim temelli söz veya, Özellikler penceresinden değeri varsayılan olarak ayarlamak `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>3. adım: veri Web denetim ekleme ve veri değişikliği için yapılandırma

ObjectDataSource sayfasına eklenen ve yapılandırılmış sonra hem verileri görüntülemek ve değiştirmek son kullanıcı için bir yol sağlamak için sayfayı veri Web denetimleri eklemek hazırız. Bu veri Web denetimleri, veri değişikliği özellikleri ve yapılandırmasını farklı olduğundan GridView, DetailsView ve FormView ayrı ayrı inceleyeceğiz.

Bu makalenin geri kalanında çok temel düzenleme, ekleme ve silme DetailsView, GridView aracılığıyla destek ekleme görüyoruz ve FormView denetimleri gibi birkaç onay kutularını denetimi gerçekten kadar kolaydır. Birçok ıot'nin ve edge bu işlevselliğin hemen üzerine gelin ve tıklayın çok daha karmaşık sağlama olun gerçek dünyadaki durumlarda vardır. Bu öğreticide, ancak yalnızca alıyormuş veri değiştirme özelliklerini kanıtlama üzerinde odaklanır. Sonraki öğreticiler gerçek ayarında kuşkusuz çıkabilecek sorunları inceleyeceksiniz.

## <a name="deleting-data-from-the-gridview"></a>GridView silme verileri

GridView araç tasarımcıya sürükleyerek başlatın. Ardından, ObjectDataSource GridView'ın akıllı etiket aşağı açılan listeden seçerek GridView'a bağlayın. Bu noktada GridView'ın bildirim temelli biçimlendirme olacaktır:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

ObjectDataSource ile akıllı etiketinde GridView bağlama iki avantajı vardır:

- BoundFields ve CheckBoxFields ObjectDataSource tarafından döndürülen alanların her biri için otomatik olarak oluşturulur. Ayrıca, BoundField ve CheckBoxField'ın özelliklerini temel alan meta göre ayarlanır. Örneğin, `ProductID`, `CategoryName`, ve `SupplierName` alanları olarak salt okunur olarak işaretlenmiş `ProductsDataTable` ve bu nedenle düzenlerken güncelleştirilebilir olmamalıdır. Bu, bu BoundFields uyum sağlayacak şekilde [salt okunur özellikler](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) ayarlandığından `true`.
- [DataKeyNames özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) için birincil anahtar alanları temel nesnenin atanır. Bu gerekli olduğunda bu özellik alan (veya alanları kümesini) da anlaşılacağı gibi düzenleme veya veri silme GridView kullanan bu benzersiz her kaydı tanımlar. Daha fazla bilgi için `DataKeyNames` özelliği kiracıurl [ana/Ayrıntılar Detailview'u ile seçilebilir bir ana GridView kullanan Detail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) öğretici.

GridView ObjectDataSource Özellikler penceresinde veya bildirim temelli söz dizimi ile ilişkili olabilir, ancak bunun yapılması uygun BoundField el ile eklemeyi gerektirir ve `DataKeyNames` biçimlendirme.

GridView denetiminde satır düzeyinde düzenleme ve silme için yerleşik destek sağlar. GridView silme destekleyecek şekilde yapılandırma Sil düğmeleri içeren bir sütun ekler. Son kullanıcının belirli bir satır için Sil düğmesine tıkladığında, bir geri gönderme ensues ve GridView aşağıdaki adımları gerçekleştirir:

1. ObjectDataSource `DeleteParameters` değerler atanır
2. ObjectDataSource `Delete()` yöntemi çağrılır, belirtilen kaydı siliniyor
3. GridView kendisi için ObjectDataSource çağırarak rebinds kendi `Select()` yöntemi

Atanan değerler `DeleteParameters` değerleri `DataKeyNames` olan Sil düğmesine tıklandığında satır için alanları. Bu nedenle önemlidir, GridView'ın `DataKeyNames` özelliğinin doğru şekilde ayarlanması gerekir. Eğer yoksa `DeleteParameters` atanacak bir `null` karşılık birinde neden olmaz adım 1'deki değer 2. adım kayıtlar silindi.

> [!NOTE]
> `DataKeys` Koleksiyon güncelleştirmeyeceği GridView s denetim durumda depolandığı `DataKeys` değerleri anımsanacak geri gönderme arasında bile GridView s görünüm durumu devre dışı bırakıldı. Ancak, düzenleme veya silme (varsayılan davranış) destekleyen GridViews için Görünüm durumunun etkin kalmasını çok önemlidir. GridView s ayarlarsanız `EnableViewState` özelliğini `false`, düzenleme ve silme davranışı, tek bir kullanıcı için düzgün çalışır, ancak veri silme eş zamanlı kullanıcılar varsa, bu eş zamanlı kullanıcılar yanlışlıkla olabilir olasılığı vardır silme veya düzenleme kaydeder, bunlar etmedi t düşündüğünüz. My blog girişine bakın [uyarısı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews bu düzenleme desteği ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), daha fazla bilgi için.


Bu aynı uyarı DetailsViews ve FormViews için de geçerlidir.

GridView'a silme özellikleri eklemek, akıllı etiketinde gidin ve silmeyi etkinleştir onay kutusunu işaretleyin.


![Onay kutusu silme etkinleştir denetleyin](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Şekil 10**: Checkbox silme etkinleştir denetleyin


Akıllı etiket silmeyi etkinleştir onay denetleniyor GridView'a bir CommandField ekler. Bir sütunda bir veya daha fazla aşağıdaki görevleri gerçekleştirmek için düğmelerle GridView CommandField oluşturur: bir kayıt seçme, bir kaydın düzenlenmesi ve kayıt silme. Kayıtları seçme ile uygulamada CommandField daha önce gördüğümüz [ana/Ayrıntılar Detailview'u ile seçilebilir bir ana GridView kullanan Detail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) öğretici.

CommandField içerir `ShowXButton` hangi dizi düğme CommandField içinde görüntülenen belirten özellikler. Silmeyi etkinleştir onay kutusu bir CommandField denetleyerek, `ShowDeleteButton` özelliği `true` GridView'ın sütunlar koleksiyonuna eklendi.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

Bu noktada, believe, olmadığı, GridView'a silme desteği ekleme ile tamamlandı! Şekil 11 gösterildiği gibi bu sayfayı Sil düğmeleri içeren bir sütun tarayıcısından ziyaret mevcut olduğunda.


[![CommandField Sil düğmeleri içeren bir sütun ekler.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Şekil 11**: bir sütun, Sil düğmeleri CommandField ekler ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Bu öğreticide baştan, kendi tıklayarak bu sayfayı test ederken oluşturuluyorsa Sil düğmesine bir özel durum oluşturacak. Bu özel durumların neden ortaya çıktı ve nasıl düzeltileceğini dair bilgi için okumaya devam edin.

> [!NOTE]
> Bu öğreticide eşlik eden indirme kullanarak boyunca, takip ediyorsanız, bu sorunları zaten karşıladığından. Ancak, karşılaşabileceğiniz sorunları ve uygun bir geçici çözümler belirlemenize yardımcı olması için aşağıda listelenen ayrıntılara okuma geçmenizi öneriyoruz.


Bir ürün silinmeye çalışılırken bir özel durum, ileti benzer alırsanız, "*ObjectDataSource 'ObjectDataSource1' genel olmayan yöntemin 'parametreleri olan DeleteProduct' bulamadı: ProductID, özgün\_ ProductID*, "kaldırmak büyük olasılıkla unuttum `OldValuesParameterFormatString` ObjectDataSource özelliği. İle `OldValuesParameterFormatString` özelliği belirtildi, ObjectDataSource çalışır hem de geçirilecek `productID` ve `original_ProductID` giriş parametreleri için `DeleteProduct` yöntemi. `DeleteProduct`, ancak yalnızca bir tek giriş parametresi, bu nedenle kabul özel durum. Kaldırma `OldValuesParameterFormatString` özelliği (veya bu ayarın `{0}`) özgün giriş parametresinde geçirilecek kullanmamanız ObjectDataSource bildirir.


[![ObjectDataSource'taki özelliği temizlendikten emin olun](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Şekil 12**: emin `OldValuesParameterFormatString` özelliği sahip olan temizlenmiş Out ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Kaldırdığınız olsa bile `OldValuesParameterFormatString` özelliği, yine de alırsınız bir özel durum iletisiyle bir ürünü silmek çalışırken: "*DELETE deyiminin, REFERENCE kısıtlayıcısıyla çakıştı ' FK\_sipariş\_ayrıntıları \_Ürünlerin*. " Northwind veritabanı arasında bir yabancı anahtar kısıtlaması içeriyor `Order Details` ve `Products` tablo, içinde onun için bir veya daha fazla kayıt varsa ürün sistemden silinemiyor anlamı `Order Details` tablo. Northwind veritabanındaki her ürüne en az bir kayıt olduğundan `Order Details`, önce ürünün ilişkili sipariş ayrıntıları kayıtları silmemiz kadar tüm ürünleri silme hatası.


[![Bir yabancı anahtar kısıtlaması ürünleri silinmesini engeller.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Şekil 13**: bir yabancı anahtar kısıtlaması, silme ürünleri engeller ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


Müşterilerimize öğreticide, github'dan tüm kayıtlarını silmeniz yeterlidir `Order Details` tablo. Gerçek bir uygulamada biz ya da gerekir:

- Sipariş Ayrıntıları bilgilerini yönetmek için başka bir ekrana sahip
- Büyütmek `DeleteProduct` belirtilen ürün sipariş ayrıntıları silmek için mantığı içerecek şekilde yöntemi
- Belirtilen ürünün Sipariş Ayrıntıları silinmesini için TableAdapter tarafından kullanılan SQL sorgusunu Değiştir

Şimdi tüm kayıtlarını silmeniz yeterlidir `Order Details` yabancı anahtar kısıtlaması aşmak için tablo. Visual Studio sunucu Gezgini'nde gidin, sağ `NORTHWND.MDF` düğümünü ve yeni sorguyu seçin. Daha sonra sorgu penceresine şu SQL ifadesini çalıştırın: `DELETE FROM [Order Details]`


[![Sipariş Ayrıntıları tablosundan tüm kayıtları silin](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Şekil 14**: tüm kayıtları silme `Order Details` tablo ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Temizleme sonra `Order Details` Tablo Sil düğmesine tıklayarak, ürün hatasız siler. GridView'ın emin olmak için ürün ve Sil düğmesine tıklayarak silinmez, denetleyin `DataKeyNames` özelliği için birincil anahtar alanı ayarlayın (`ProductID`).

> [!NOTE]
> Sil düğmesine tıklandığında bir geri gönderme ensues ve kaydı silinir. Yanlışlıkla yanlış sıranın Sil düğmesine tıklayın daha kolay olduğundan bu tehlikeli olabilir. Bir sonraki öğreticide kayıt silerken istemci tarafı doğrulama ekleme göreceğiz.


## <a name="editing-data-with-the-gridview"></a>GridView ile verileri düzenleme

GridView denetiminde silme yanı sıra yerleşik satır düzeyinde düzenleme desteği de sağlar. GridView düzenlemeyi destekleyecek şekilde yapılandırma Düzenle düğmeleri içeren bir sütun ekler. Düzenlenebilir olmasını satır bir sıranın Düzenle düğmesini nedenleri tıkladığınızda son kullanıcı açısından bakıldığında, var olan değerleri içeren ve güncelleştirme ile Düzenle düğmesi ve İptal düğmeleri değiştirerek metin kutuları hücreleri kapatılıyor. Son kullanıcı, istenen bir değişiklik yapıldıktan sonra değişiklikleri kaydetmek için Güncelleştir düğmesine veya bunları atmak iptal düğmesine tıklayabilirsiniz. Her iki durumda da, güncelleştirme veya İptal'i tıklattıktan sonra GridView önceden düzenleme durumuna geri döner.

Sayfasında geliştirici olarak bizim açısından son kullanıcının belirli bir satır için Düzenle düğmesine tıkladığında, bir geri gönderme ensues ve GridView aşağıdaki adımları gerçekleştirir:

1. GridView'ın `EditItemIndex` özelliği, Düzenle düğmesine tıkladı satırın dizini atandığında
2. GridView kendisi için ObjectDataSource çağırarak rebinds kendi `Select()` yöntemi
3. Eşleşen satır dizini `EditItemIndex` "düzenleme modunda." işlenir Bu modda, güncelleştirme ve İptal düğmeleri ve BoundFields Düzenle düğmesini almıştır, `ReadOnly` özelliklerdir False (varsayılan) metin kutusuna Web ayarlanmış olarak işlenen `Text` özellikleri için veri alanlarını değerler atanır.

Bu noktada işaretleme sıranın veri değişiklik yapmak son kullanıcının tarayıcıyı döndürülür. Kullanıcı güncelleştir düğmesine tıkladığında, bir geri gönderme oluşur ve GridView aşağıdaki adımları gerçekleştirir:

1. ObjectDataSource `UpdateParameters` değerleri, GridView'ın düzenleme arabirimine son kullanıcı tarafından girilen değerler atanır
2. ObjectDataSource `Update()` yöntemi çağrılır, belirtilen kaydı güncelleştiriliyor
3. GridView kendisi için ObjectDataSource çağırarak rebinds kendi `Select()` yöntemi

Atanan birincil anahtar değerlerini `UpdateParameters` 1. adımda belirtilen değerleri geldiğini `DataKeyNames` özelliği birincil olmayan anahtar değerlerinin düzenlenmiş satır için metin kutusu Web denetimlerindeki metin gelir ise. Silme işlemine çok önemli olduğu gibi bir GridView'ın `DataKeyNames` özelliğinin doğru şekilde ayarlanması gerekir. Eğer yoksa `UpdateParameters` birincil anahtar değeri atanabilir bir `null` karşılık birinde neden olmaz adım 1'deki değer güncelleştirilmiş kayıtları 2. adım.

Düzenleme işlevselliği GridView'ın akıllı etiket düzenlemeyi etkinleştir onay kutusunu işaretleyerek etkin hale getirilebilir.


![Onay kutusu düzenlemeyi etkinleştir denetleyin](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Şekil 15**: Checkbox düzenlemeyi etkinleştir denetleyin


Düzenlemeyi Etkinleştir onay kutusu, bir CommandField eklenir, (gerekirse) denetleniyor ve kümesi kendi `ShowEditButton` özelliğini `true`. Daha önce bahsettiğim gibi bir dizi CommandField içeren `ShowXButton` hangi dizi düğme CommandField içinde görüntülenen belirten özellikler. Düzenlemeyi Etkinleştir onay kutusu denetimi ekler `ShowEditButton` mevcut CommandField özelliği:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Tüm ilkel düzenleme desteği ekleme için yoktur. Düzenleme arabirimi yerine kaba Figure16 gösterildiği gibi her BoundField olan `ReadOnly` özelliği `false` (varsayılan), bir metin kutusu olarak işlenir. Bu gibi alanları içerir `CategoryID` ve `SupplierID`, diğer tablolara anahtarları olan.


[![Tıklayarak Chai s Düzenle düğmesini satır düzenleme modunda görüntüler.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Şekil 16**: düzenleme modunda Düzenle düğmesine tıklayarak Chai s görüntüler satır ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Yabancı anahtar değerlerine doğrudan düzenlenecek kullanıcılar soran ek olarak, aşağıdaki yollarla düzenleme arabirimin arabirimi bulunmaması:

- Kullanıcı girerse bir `CategoryID` veya `SupplierID` veritabanında mevcut olmayan `UPDATE` oluşturulması bir özel duruma neden bir yabancı anahtar kısıtlaması ihlal.
- Düzenleme arabirimi, tüm doğrulama içermez. Gerekli değer sağlamıyorsa (gibi `ProductName`), veya sayısal bir değer ("Çok!" girerek gibi burada beklenen bir dize değeri girin içine `UnitPrice` metin kutusu), bir özel durum oluşturulur. Bir sonraki öğretici düzenleme kullanıcı arabirimine doğrulama denetimleri ekleme inceleyeceksiniz.
- Şu anda *tüm* salt okunur olmayan ürün alanları GridView eklenmesi gerekir. GridView ' bir alanı kaldırmak için olsaydık söyleyin `UnitPrice`, GridView verileri güncelleştirme ayarı yapılmadı `UnitPrice` `UpdateParameters` veritabanı kaydın değiştirirsiniz değeri `UnitPrice` için bir `NULL` değeri. Benzer şekilde, gerekli bir alan, gibi `ProductName`, kaldırılır GridView ' güncelleştirme ile aynı yapamaz "*'ProductName' sütunu null değerlere izin vermiyor*" özel durum bahsedilen yukarıda.
- Düzenleme arabirim biçimlendirme istenen için çok fazla bırakır. `UnitPrice` Dört ondalık basamak ile gösterilir. İdeal olarak `CategoryID` ve `SupplierID` sistemde üreticiler ve kategoriler listesinde DropDownList değerleri içerebilir.

Şimdi, ancak ile canlı gerekir tüm eksiklikleri bunlar sonraki öğreticilerde ele.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Ekleme, düzenleme ve DetailsView verilerle siliniyor

Önceki öğreticilerde anlatıldığı gibi DetailsView denetiminde bir kaydı aynı anda ve GridView gibi görüntüler, düzenleme ve şu anda görüntülenen kaydını silme olanağı sağlar. Her iki son kullanıcı deneyimi, düzenleme ve bir DetailsView ve ASP.NET tarafından gelen bir iş akışı öğeleri silme ile GridView aynıdır. Burada DetailsView GridView ' yerleşik ekleme desteği de sağlar, farklıdır.

GridView'ın veri değişikliği özellikleri göstermek için başlatmak için bir DetailsView ekleyerek `Basics.aspx` sayfasında mevcut GridView ve mevcut ObjectDataSource DetailsView'ın akıllı etiket aracılığıyla bağlayın. Sonraki DetailsView'ın Temizle `Height` ve `Width` özellikleri ve onay akıllı etiket sayfalama Etkinleştir seçeneği. Düzenlemeyi etkinleştirmek için ekleme ve silme desteği, akıllı etiket düzenlemeyi etkinleştir, etkinleştirme eklemeyi ve silmeyi etkinleştir onay kutularını işaretleyerek.


![DetailsView düzenleme, ekleme ve silme desteği için yapılandırma](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Şekil 17**: DetailsView düzenleme, ekleme ve silme desteği için yapılandırma


GridView düzenleme, ekleme, ekleme veya silme desteği bir CommandField DetailsView için aşağıdaki bildirim temelli söz dizimi gösterildiği gibi ekler:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

DetailsView CommandField için sütun koleksiyonundaki sonunda varsayılan olarak göründüğüne dikkat edin. DetailsView'ın alanları CommandField ile ekleme, satır olarak görünüp satırlar olarak işlenen bu yana düzenleyebilir ve DetailsView alt kısmındaki düğmeleri silebilirsiniz.


[![DetailsView düzenleme, ekleme ve silme desteği için yapılandırma](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Şekil 18**: ekleme ve silme düzenleme desteği için DetailsView yapılandırın ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Sil düğmesine tıklayarak aynı olaylar dizisi ile başlar gibi GridView: geri gönderme; bir kendi ObjectDataSource doldurma DetailsView tarafından izlenen `DeleteParameters` göre `DataKeyNames` değerleri; ve bir çağrı ile kendi ObjectDataSource tamamlandı `Delete()` yöntemi gerçekten ürün veritabanından kaldırır. DetailsView içinde düzenleme de GridView öğesinin aynı şekilde çalışır.

Ekleme için son kullanıcı ile bir yeni sunulur, düğmesine tıklandığında, "ekleme modunda" DetailsView işler "Ekle moduyla" yeni düğme ekleme ve İptal düğmeleri ve yalnızca bu BoundFields almıştır, `InsertVisible` özelliği `true` (varsayılan) görüntülenir. Bu veri alanları gibi otomatik artış alanları olarak tanımlanan `ProductID`, sahip kendi [InsertVisible özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) kümesine `false` DetailsView akıllı etiket aracılığıyla veri kaynağına bağlanırken.

Bir DetailsView aracılığıyla akıllı etiket için bir veri kaynağına bağlanırken, Visual Studio ayarlar `InsertVisible` özelliğini `false` yalnızca otomatik artış alanları için. Salt okunur alanları `CategoryName` ve `SupplierName`, sürece "ekleme modu" kullanıcı arabiriminde görüntülenecek kendi `InsertVisible` özelliği ayarlanmış açıkça `false`. Bu iki alan ayarlamak için bir dakikanızı ayırın `InsertVisible` özelliklerine `false`, akıllı etiketinde alanları Düzenle veya DetailsView'ın bildirim temelli söz dizimi aracılığıyla bağlayın. Şekil 19 gösterir ayarı `InsertVisible` özelliklerine `false` düzenleme alanları tıklayarak bağlantı.


[![Northwind Traders artık Acme Çay sunar](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Şekil 19**: Northwind Traders artık sunar Acme Çay ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Ayarlanmasından sonra `InsertVisible` özellikleri, Görünüm `Basics.aspx` sayfasında bir tarayıcıda ve yeni düğmesine tıklayın. Şekil 20 DetailsView gösteren yeni bir içecek eklerken bizim ürün satıra Acme Çay.


[![Northwind Traders artık Acme Çay sunar](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Şekil 20**: Northwind Traders artık sunar Acme Çay ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Yeni bir kayıt eklenir ve Acme Çay için ayrıntıları girerek ve Ekle düğmesine tıklandıktan sonra bir geri gönderme ensues `Products` veritabanı tablosu. Bu DetailsView ürünleri veritabanı tablosu, oldukları sırada listelendiğinden, biz ürün yeni ürünü görmek için son sayfa gerekir.


[![Acme Çay için Ayrıntılar](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Şekil 21**: Acme Çay ayrıntılarını ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> DetailsView'ın [CurrentMode özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) görüntülenmesini arabirimi gösterir ve aşağıdaki değerlerden biri olabilir: `Edit`, `Insert`, veya `ReadOnly`. [DefaultMode özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) DetailsView döndürür sonra bir düzenleme veya ekleme modunda tamamlandı ve kalıcı olarak Düzenle veya modu INSERT bir DetailsView görüntülemek için yararlıdır.


GridView onunla aynı sınırlamalara gelen ekleme ve DetailsView özelliklerini düzenleme ve noktası etkilese: Kullanıcı mevcut girmeden `CategoryID` ve `SupplierID` değerleri bir metin kutusu aracılığıyla; arabirimi oturumda herhangi bir doğrulama mantığı; tüm izin verme ürün alanları `NULL` değerleri veya varsayılan yoksa ekleme arabirimi vb. veritabanı düzeyinde belirtilen değeri dahil edilmelidir.

Genişletme ve makaleleri DetailsView denetimin düzenleme ve arabirimleri de ekleme için uygulanabilir GridView'ın düzenleme arabirimi gelecekteki geliştirme teknikleri inceleyeceğiz.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>FormView için daha esnek bir veri değişikliği kullanıcı arabirimini kullanarak

FormView ekleme, düzenleme ve verileri silmek için yerleşik destek sunar, ancak alanları yerine şablonları kullandığından BoundFields veya veri sağlamak için GridView ve DetailsView denetimlerini tarafından kullanılan CommandField eklemek için bir yer yoktur değişikliği arabirimini. Bunun yerine, kullanıcı toplamak için Web denetimleri yeni bir öğe eklerken, giriş veya mevcut bir yeni yanı sıra düzenleme düzenleme, silme, INSERT, Update ve İptal düğmeleri bu arabirim uygun şablonlar için el ile eklenmesi gerekir. Neyse ki, Visual Studio otomatik olarak gerekli arabirimi FormView aracılığıyla akıllı etiketinde aşağı açılan listeden bir veri kaynağına bağlanırken oluşturur.

Bir FormView'da için ekleyerek bu teknikler göstermek için başlangıç `Basics.aspx` sayfasında ve isteğe bağlı olarak FormView akıllı etiketi, önceden oluşturulmuş ObjectDataSource bağlayın. Bu oluşturacak bir `EditItemTemplate`, `InsertItemTemplate`, ve `ItemTemplate` FormView ile için yeni toplama, kullanıcının giriş ve düğme Web denetimleri için TextBox Web denetimleri için düzenleme, silme, INSERT, Update ve İptal düğmeleri. Ayrıca, FormView `DataKeyNames` özelliği için birincil anahtar alanı ayarlayın (`ProductID`) ObjectDataSource tarafından döndürülen nesne. Son olarak, FormView akıllı etiket sayfalama Etkinleştir seçeneği denetleyin.

Aşağıdaki bildirim temelli biçimlendirme FormView için gösterir `ItemTemplate` FormView için ObjectDataSource bağlandıktan sonra. Varsayılan olarak, her Boole olmayan değer ürün alanı için bağlı `Text` bir etiket Web denetimi sırasında her bir Boole değeri alan özelliği (`Discontinued`) bağlı `Checked` özelliği devre dışı onay kutusu Web denetimi. Yeni, Düzenle ve Sil düğmeleri tıklandığında belirli FormView davranışı tetiklemek için sırada kesinlik temelli, kendi `CommandName` değerleri ayarlanması `New`, `Edit`, ve `Delete`sırasıyla.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Şekil 22 gösterir FormView `ItemTemplate` bir tarayıcıdan görüntülendiğinde. Her ürün alanı altındaki Yeni, Düzenle ve Sil düğmeleri listelenir.


[![Defaut FormView ItemTemplate her ürün alanı yeni birlikte listeler, Düzenle ve Sil düğmeleri](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Şekil 22**: Defaut FormView `ItemTemplate` listeler her ürün alanı boyunca yeni, Düzenle ve Sil düğmeleri ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


GridView ve Sil düğmesine veya tüm düğme, LinkButton veya ImageButton tıklatarak DetailsView, ile gibi `CommandName` özelliği bir geri gönderme silme nedenler ayarlandıysa, ObjectDataSource doldurur `DeleteParameters` FormView üzerinde tabanlı`DataKeyNames`değeri ve ObjectDataSource çağırır `Delete()` yöntemi.

Düzenle düğmesine tıklandığında bir geri gönderme ensues ve veriler için DataSet'e `EditItemTemplate`, düzenleme arabirimi çizmek için sorumlu olduğu. Bu arabirim, düzenleme güncelleştirme ve İptal düğmeleri yanı sıra veri Web denetimleri içerir. Varsayılan `EditItemTemplate` tarafından oluşturulan Visual Studio her otomatik artış alanlar için bir etiket içerir (`ProductID`), metin kutusu her bir Boole olmayan değer alan için bir ve onay kutusu her bir Boole değeri alan için bir. Bu davranış GridView ve DetailsView denetimlerini içinde otomatik olarak oluşturulan BoundFields çok benzer.

> [!NOTE]
> FormView otomatik olarak oluşturulmasını küçük bir sorun `EditItemTemplate` , TextBox Web, denetimleri salt okunur olduğu gibi bu alanlar için işler olan `CategoryName` ve `SupplierName`. Bu hesap nasıl görüyoruz kısa bir süre.


TextBox denetimleri içinde `EditItemTemplate` sahip kullanıcıların `Text` özelliğe karşılık gelen kendi veri alanını kullanarak değerine *çift yönlü veri bağlama*. İki yönlü veri bağlama, belirtilen tarafından `<%# Bind("dataField") %>`, veri bağlama iki şablona veri bağlama sırasında ve ObjectDataSource parametre ekleme veya kayıt düzenleme doldurulurken gerçekleştirir. Diğer bir deyişle, kullanıcı Düzenle düğmesini tıkladığında `ItemTemplate`, `Bind()` yöntemi, belirtilen veri alan değeri döndürür. Kullanıcı değişikliklerini sağlar ve güncelleştirme tıkladığında sonra değerleri kullanarak belirtilen veri alanları için karşılık gelen arka gönderilen `Bind()` ObjectDataSource uygulanan `UpdateParameters`. Alternatif olarak, tek yönlü veri bağlama, belirtilen tarafından `<%# Eval("dataField") %>`, yalnızca şablon için veri bağlama sırasında veri alanı değerlerini alır ve mu *değil* kullanıcı tarafından girilen değerlerin geri göndermede veri kaynağının parametrelerini döndürür.

FormView aşağıdaki bildirim temelli biçimlendirmeyi gösterir `EditItemTemplate`. Unutmayın `Bind()` yöntemi burada veri bağlama söz diziminde kullanılır ve güncelleştir ve iptal düğmesi Web denetimleri kendi `CommandName` özellikleri uygun şekilde ayarlayabilirsiniz.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Bizim `EditItemTemplate`, bu noktası, bir özel kullanmaya çalışırsanız durum neden olur. Sorun `CategoryName` ve `SupplierName` alanları TextBox Web denetimlerini gibi işlenir `EditItemTemplate`. Ya da bu metin kutuları için etiketleri değiştirebilir veya tamamen kaldırmak ihtiyacımız var. Şimdi yalnızca tamamen silebilirsiniz `EditItemTemplate`.

Ayrıntılarını Düzenle düğmesine tıkladıktan sonra çıkan şekil 23 bir tarayıcıda FormView gösterir. Unutmayın `SupplierName` ve `CategoryName` gösterilen alanlar `ItemTemplate` bunlardan yalnızca kaldırıldı olarak artık mevcut olmayan `EditItemTemplate`. FormView güncelleştir düğmesine tıklandığında GridView ve DetailsView denetimlerini aynı adımlar dizisini aracılığıyla devam eder.


[![Varsayılan olarak EditItemTemplate her düzenlenebilir ürün alanı olarak bir metin kutusu veya onay kutusunu gösterir.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Şekil 23**: varsayılan olarak `EditItemTemplate` gösterir her düzenlenebilir ürün alanı olarak bir metin kutusu veya onay kutusunu ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Ne zaman Ekle düğmesine tıklandığında FormView `ItemTemplate` ensues bir geri gönderme. Ancak, yeni bir kayıt eklendiğinden veri için FormView bağlıdır. `InsertItemTemplate` Arabirimi ekleme ve İptal düğmeleri birlikte yeni bir kayıt eklemek için Web denetimleri içerir. Varsayılan `InsertItemTemplate` tarafından oluşturulan Visual Studio içeren her bir Boole olmayan değer alan için bir metin kutusu ve bir onay kutusu otomatik olarak oluşturulan için benzer her Boole değeri alan `EditItemTemplate`kullanıcının arabirim. TextBox denetimine sahip kullanıcıların `Text` özelliğe değeri çift yönlü veri bağlamasını kullanma, karşılık gelen bir veri alanı olarak.

FormView aşağıdaki bildirim temelli biçimlendirmeyi gösterir `InsertItemTemplate`. Unutmayın `Bind()` yöntemi burada veri bağlama söz diziminde kullanılır ve ekleme ve iptal düğmesi Web denetimleri kendi `CommandName` özellikleri uygun şekilde ayarlayabilirsiniz.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

FormView otomatik olarak oluşturulmasını içeren bir subtlety yoktur `InsertItemTemplate`. Özellikle, metin kutusu Web denetimleri bile salt okunur olduğu gibi alanlar için oluşturulan `CategoryName` ve `SupplierName`. İle gibi `EditItemTemplate`, bu kutularındaki metinleri kaldırın ihtiyacımız `InsertItemTemplate`.

Şekil 24 FormView Acme kahve yeni bir ürün eklerken bir tarayıcıda gösterir. Unutmayın `SupplierName` ve `CategoryName` gösterilen alanlar `ItemTemplate` yalnızca bunları kaldırıldı olarak artık mevcut değil. DetailsView denetiminde aynı adımlar dizisini aracılığıyla FormView kazançlar Ekle düğmesine tıklandığında yeni bir kayda ekleme `Products` tablo. Bunu eklendikten sonra Şekil 25 FormView'da Acme kahve ürünün ayrıntılarını gösterir.


[![FormView ekleme arabirimi InsertItemTemplate belirler.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Şekil 24**: `InsertItemTemplate` FormView ekleme arabirimi belirler ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Yeni ürün, GDB kahve ayrıntılarını FormView'da görüntülenir](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Şekil 25**: yeni ürün, GDB kahve ayrıntılarını FormView'da görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Salt okunur ayırarak, düzenleme ve üç ayrı şablonlara arabirimleri ekleme FormView zahmetli bu arabirimleri üzerinde denetim GridView ve DetailsView daha sağlar.

> [!NOTE]
> DetailsView, FormView's gibi `CurrentMode` özelliği görüntülenmesini arabirimi gösterir ve kendi `DefaultMode` ekleme tamamlandı veya özelliği bir düzenlemeden sonra FormView döndürür modunu gösterir.


## <a name="summary"></a>Özet

Bu öğreticide, biz ekleme, düzenleme ve GridView, DetailsView ve FormView kullanarak veri silme temellerini incelenir. Bu denetimlerin üç belirli bir düzeyde veri Web denetimleri ve ObjectDataSource sayesinde ASP.NET sayfasında tek bir satır kod yazmadan yararlanılabilir yerleşik veri değiştirme özelliklerini sağlar. Ancak, basit üzerine gelin ve oldukça frail teknikleri işleme ve naïve veri değişikliği kullanıcı arabirimini'ı tıklayın. Doğrulama sağlamak için programlı değerler ekleme, düzgün olarak işleyebileceğiniz özel durumları, kullanıcı arabirimini özelleştirme ve bu şekilde bir sonraki birkaç öğreticiler açıklanan teknikleri bevy etmenin gerekir.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
