---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: Ekleme, güncelleştirme ve verileri (VB) silme bir genel bakış | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bir ObjectDataSource'nın INSERT(), Update(), eşleme göreceğiz ve configu nasıl yanı sıra BLL yöntemlerinin Delete() yöntemlere sınıfları...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: db77d9ec5b0d4b27259023363e786b26fe736d7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890827"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>Ekleme, güncelleştirme ve verileri (VB) silme bir genel bakış
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe) veya [PDF indirin](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> Bu öğreticide bir ObjectDataSource'nın INSERT(), Update(), eşleme göreceğiz ve BLL yöntemlerinin Delete() yöntemlere sınıfları, veri değişikliği özellikleri sağlamak üzere GridView, DetailsView ve FormView denetimleri yapılandırmak nasıl yanı sıra.


## <a name="introduction"></a>Giriş

Son birkaç öğreticileri biz GridView, DetailsView ve FormView denetimleri kullanarak, ASP.NET sayfası verileri görüntülemek nasıl anlatılmaktadır. Bu denetimlerin yalnızca kendilerine sağlanan verilerle çalışır. Genellikle, bu denetimlerin ObjectDataSource gibi bir veri kaynağı denetimi kullanımı ile veri erişim. ObjectDataSource ASP.NET sayfası ve temel alınan veri arasındaki bir proxy olarak nasıl ele alacağını gördük. GridView, verileri görüntülemek gerektiğinde kendi ObjectDataSource's çağırır `Select()` buna karşılık gelen bizim iş mantığı katmanı (uygun veri erişim katmanı ' 's (DAL) bir yöntemi çağırır BLL), bir yöntemi çağırır yöntemi sırayla gönderir TableAdapter bir `SELECT` Northwind veritabanına sorgu.

Biz TableAdapters DAL oluşturduğunuzda, geri çağırma [ilk öğreticimizi](../introduction/creating-a-data-access-layer-cs.md), Visual Studio otomatik güncelleştirme, ekleme, yöntemleri eklenir ve arka plandaki silme verileri veritabanı tablosu. Ayrıca, içinde [bir iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-vb.md) Biz bu veri değişikliği DAL yöntemlere adlı BLL yöntemleri tasarlanmıştır.

Ek olarak kendi `Select()` yöntemi, ObjectDataSource de sahip `Insert()`, `Update()`, ve `Delete()` yöntemleri. Gibi `Select()` yöntemi, bu üç yöntem yöntemleri temel alınan nesnede eşlenebilir. Ekle, Güncelleştir veya veri silmek için yapılandırıldığında, GridView, DetailsView ve FormView denetimleri temel alınan veri değiştirmek için bir kullanıcı arabirimi sağlar. Bu kullanıcı arabirimini çağıran `Insert()`, `Update()`, ve `Delete()` nesnesini çağıran ObjectDataSource yöntemlerinin (bkz. Şekil 1) yöntemleri ilişkili.


[![ObjectDataSource'nın INSERT(), Update() ve Delete() yöntemleri bir Proxy olarak BLL sunar.](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**Şekil 1**: ObjectDataSource's `Insert()`, `Update()`, ve `Delete()` yöntemleri BLL içine bir Proxy olarak hizmet verir ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


Bu öğreticide ObjectDataSource's eşlemek nasıl göreceğiz `Insert()`, `Update()`, ve `Delete()` veri değişikliği sağlamak için GridView, DetailsView ve FormView denetimleri yapılandırmak nasıl yanı sıra BLL sınıflarda yöntemlerinin yöntemleri yetenekleri.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>1. adım: INSERT, Update ve Delete öğreticileri Web sayfaları oluşturma

Şimdi eklemek, güncelleştirmek ve verileri silmek nasıl keşfetme başlamadan önce ilk biz Bu öğreticide ve sonraki birkaç olanlar için gerekir, Web sitesi projesindeki ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın. Adlı yeni bir klasör eklemeye başlayın `EditInsertDelete`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Veri değişikliği ilgili öğreticileri için ASP.NET sayfaları ekleme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**Şekil 2**: veri değişikliği ilgili öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `EditInsertDelete` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` sayfanın Tasarım görünümü Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**Şekil 3**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


Son olarak, girişleri olarak sayfaları eklemek `Web.sitemap` dosya. Özellikle, özelleştirilmiş biçimlendirme sonra aşağıdaki biçimlendirme eklemek `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık düzenleme, ekleme ve öğreticiler silmek için öğeler içerir.


![Site Haritasını şimdi düzenleme, ekleme ve silme öğreticileri için girişleri içerir](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**Şekil 4**: Site Haritası şimdi düzenleme, ekleme ve silme öğreticileri için girişleri içerir


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>2. adım: Ekleme ve ObjectDataSource Denetimi yapılandırma

GridView, DetailsView ve her farklı kendi veri değişikliği özellikleri ve düzeni FormView itibaren her biri ayrı ayrı inceleyelim. Kendi ObjectDataSource kullanarak her denetiminiz yerine ancak, yalnızca tüm üç denetimine örnekler paylaşabilirsiniz tek bir ObjectDataSource oluşturalım.

Açık `Basics.aspx` sayfasında bir ObjectDataSource tasarımcıya araç çubuğuna sürükleyin ve akıllı etiket yapılandırma veri kaynağı bağlantısını tıklayın. Bu yana `ProductsBLL` düzenleme, ekleme ve silme yöntemleri, bu sınıfın kullanılacak ObjectDataSource yapılandırma sağlayan tek BLL sınıftır.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**Şekil 5**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


Sonraki ekranda biz hangi yöntemleri belirtebilirsiniz `ProductsBLL` sınıfı ObjectDataSource için 's eşlendi `Select()`, `Insert()`, `Update()`, ve `Delete()` uygun sekmeyi seçerek ve aşağı açılan listeden yöntemi seçme. Artık tanıdık gelecektir, ObjectDataSource'nın eşlemeleri Şekil 6 `Select()` yönteme `ProductsBLL` sınıfının `GetProducts()` yöntemi. `Insert()`, `Update()`, Ve `Delete()` üstünde listeden uygun sekmeyi seçerek yöntemleri yapılandırılabilir.


[![Sahip ObjectDataSource dönüş tüm ürünleri](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**Şekil 6**: ObjectDataSource dönüş tüm ürünlerin sahip ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


Şekil 7, 8 ve 9 ObjectDataSource'nın güncelleştirme, INSERT ve DELETE Göster sekmeleri. Bu sekmeleri yapılandırmak için `Insert()`, `Update()`, ve `Delete()` yöntemleri çağırma `ProductsBLL` sınıfının `UpdateProduct`, `AddProduct`, ve `DeleteProduct` yöntemleri, sırasıyla.


[![Map ProductBLL sınıfının UpdateProduct yönteme ObjectDataSource'nın Update() yöntemi](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**Şekil 7**: ObjectDataSource's eşleme `Update()` yönteme `ProductBLL` sınıfının `UpdateProduct` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![Map ObjectDataSource'nın INSERT() yöntemi ProductBLL sınıfının AddProduct yöntemi](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**Şekil 8**: ObjectDataSource's eşleme `Insert()` yönteme `ProductBLL` sınıfının Ekle `Product` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![Map ProductBLL sınıfının DeleteProduct yönteme ObjectDataSource'nın Delete() yöntemi](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**Şekil 9**: ObjectDataSource's eşleme `Delete()` yönteme `ProductBLL` sınıfının `DeleteProduct` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


Güncelleştirme, ekleme ve silme sekmeleri açılan listelerde seçili bu yöntemleri olduğunu fark etmiş olabilirsiniz. Bu bizim sayesinde kullanımıdır `DataObjectMethodAttribute` yöntemlerinin süsler `ProducstBLL`. Örneğin, aşağıdaki imzası DeleteProduct yöntemi vardır:


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

`DataObjectMethodAttribute` Öznitelik her yöntemin amacı seçme, ekleme, güncelleştirme, ya da silme ve desteklemediğini varsayılan değer: için olup olmadığını gösterir. BLL sınıflarınızı oluştururken bu öznitelikler atlanırsa, yöntemleri UPDATE'ten el ile seçmek için Ekle ve gerekir sekmeleri SİLİN.

Uygun olduktan `ProductsBLL` yöntemleri ObjectDataSource için 's eşlendi `Insert()`, `Update()`, ve `Delete()` yöntemleri, Sihirbazı tamamlamak için Son'u tıklatın.

## <a name="examining-the-objectdatasources-markup"></a>ObjectDataSource'nın biçimlendirme inceleniyor

ObjectDataSource kendi Sihirbazı'nı yapılandırdıktan sonra oluşturulan bildirim temelli biçimlendirme incelemek için kaynak görünümüne gidin. `<asp:ObjectDataSource>` Etiketi nesnesini ve Çağırma yöntemlerini belirtir. Ayrıca, `DeleteParameters`, `UpdateParameters`, ve `InsertParameters` , eşleme giriş parametreleri için `ProductsBLL` sınıfının `AddProduct`, `UpdateProduct`, ve `DeleteProduct` yöntemleri:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource her giriş parametreleri için listesi gibi ilişkili yöntemlerinden biri için bir parametre içeren `SelectParameter` s'dir giriş parametresi beklediği seçme yöntemini çağırmak için ObjectDataSource yapılandırıldığında var ( gibi`GetProductsByCategoryID(categoryID)`). Kısa bir süre sonra bu değerleri anlatıldığı `DeleteParameters`, `UpdateParameters`, ve `InsertParameters` otomatik olarak GridView, DetailsView ve FormView ObjectDataSource's çağırmadan önce ayarlanır `Insert()`, `Update()`, veya `Delete()` yöntem. Sonraki öğreticide ele alacağız olarak bu değerleri de gerektiği şekilde, program aracılığıyla ayarlanabilir.

ObjectDataSource için Yapılandırma Sihirbazı'nı kullanarak bir yan etkisi olan Visual Studio ayarlar [ObjectDataSource'taki özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) için `original_{0}`. Bu özellik değeri düzenlenen verilerin orijinal değerleri dahil etmek için kullanılır ve iki senaryolarda kullanışlıdır:

- Bir kayıt düzenlerken, kullanıcıların birincil anahtar değerini değiştirmek tamamlayabilirseniz. Böylece özgün birincil anahtar değerini içeren kayıt bulunamadı ve buna bağlı olarak güncelleştirildi onun değerine sahip, bu durumda, yeni birincil anahtar değerine ve özgün birincil anahtar değeri sağlanmalıdır.
- İyimser eşzamanlılık kullanırken. İyimser eşzamanlılık iki emin olmak için bir tekniktir eşzamanlı kullanıcıların başka birinin değişikliklerin üzerine yazmayın ve gelecekteki bir öğretici için konudur.

`OldValuesParameterFormatString` Özelliği, temel alınan nesnenin güncelleştirme ve silme yöntemleri özgün değerler için giriş parametrelerinde adını belirtir. Biz iyimser eşzamanlılık geçirirken Biz bu özellik ve amacı daha ayrıntılı ele alacağız. I Şimdi, ancak bizim BLL'ın yöntemleri özgün değerler beklemediğiniz ve bu nedenle Biz bu özelliği kaldırmak önemlidir çünkü duruma getirmek. Bırakarak `OldValuesParameterFormatString` varsayılan dışındaki herhangi bir şey olarak ayarlanan özelliği (`{0}`) ObjectDataSource's çağrılacak veri Web denetimi çalıştığında bir hata neden `Update()` veya `Delete()` yöntemleri ObjectDataSource olur çünkü ikisi de geçirmek girişimi `UpdateParameters` veya `DeleteParameters` yanı sıra özgün değer parametresi belirtilmemiş.

Bu juncture en bu çok açık değilse, endişelenmeyin, biz bu özellik ve kendi yardımcı programı bir sonraki öğreticide inceleyeceğiz. Şu an için yalnızca bu özellik bildirimi tamamen Tanımlayıcı Sözdizimi kaldırın ya da varsayılan değere ({0}) değeri emin olun.

> [!NOTE]
> Yalnızca silerseniz `OldValuesParameterFormatString` özellik değeri Tasarım görünümünde, özellik Özellikler penceresinden bildirim temelli sözdiziminde var olmaya devam edecek ancak boş bir dize olarak ayarlanabilir. Bu, ne yazık ki, hala yukarıda açıklanan aynı soruna neden olur. Bu nedenle, kaldırın veya özelliği tamamen tanımlayıcı sözdizimi veya, Özellikler penceresini değeri varsayılan olarak, Ayarla `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>3. adım: bir veri Web denetimi ekleme ve veri değişikliği için yapılandırma

ObjectDataSource sayfaya eklenen ve yapılandırılmış sonra size hem verileri görüntülemek ve değiştirmek son kullanıcı için bir yol sağlamak için sayfaya veri Web denetimleri ekleme yapmaya hazırsınız demektir. Bu verileri Web denetimleri kendi veri değişikliği özellikleri ve yapılandırma farklı biz GridView, DetailsView ve FormView ayrı ayrı göreceğiz.

Bu makalede, geri kalanı çok temel düzenleme, ekleme ve GridView, DetailsView, desteğini silme ekleme göreceğiz ve FormView denetimleri gibi birkaç onay kutularını denetimi olarak gerçekten kadar basittir. Birçok subtleties ve gerçek yalnızca üzerine ve çok daha karmaşık tür işlevsellik sağlayan olun durumlarda sınır vardır. Bu öğretici, ancak yalnızca simplistic veri değişikliği özellikleri kanıtlayan üzerinde odaklanmıştır. Gelecekteki öğreticileri gerçek ayarında kuşkusuz çıkabilecek sorunları inceleyeceksiniz.

## <a name="deleting-data-from-the-gridview"></a>GridView silme verileri

Araç kutusu tasarımcıya GridView sürükleyerek başlatın. Ardından, ObjectDataSource GridView'ın akıllı etiket aşağı açılan listeden seçerek GridView bağlayın. Bu noktada GridView'ın bildirim temelli biçimlendirme olacaktır:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

Akıllı etiket aracılığıyla ObjectDataSource GridView bağlama iki faydası vardır:

- BoundFields ve CheckBoxFields ObjectDataSource tarafından döndürülen alanların her biri için otomatik olarak oluşturulur. Ayrıca, temel alan meta BoundField ve CheckBoxField'ın özellikleri göre ayarlanır. Örneğin, `ProductID`, `CategoryName`, ve `SupplierName` alanları olarak salt okunur olarak işaretlenmiş `ProductsDataTable` ve bu nedenle düzenlerken güncelleştirilebilir olmamalıdır. Bu, bu BoundFields uyum sağlayacak şekilde [salt okunur özellikler](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) ayarlanır `True`.
- [DataKeyNames özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) nesnesini birincil anahtar alanları için atanır. Bu gerekli olduğunda bu benzersiz her kaydı tanımlayan alan (veya alanlar kümesi) Bu özellik gösterir şekilde düzenleme veya verileri silmek için GridView kullanarak. Daha fazla bilgi için `DataKeyNames` özelliği, geri başvurmak [ana/Ayrıntılar DetailView ile seçilebilir ana GridView kullanarak ayrıntı](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Öğreticisi.

GridView ObjectDataSource Özellikler penceresini veya tanımlayıcı sözdizimi ile ilişkili olabilir, ancak bunun yapılması uygun BoundField el ile eklemeniz gerekir ve `DataKeyNames` biçimlendirme.

GridView denetimi, satır düzeyi düzenleme ve silme için yerleşik destek sağlar. GridView silme destekleyecek şekilde yapılandırma silme düğmeleri bir sütun ekler. Son kullanıcının belirli bir satır için de Sil düğmesini tıklattığında geri gönderimin ensues ve GridView aşağıdaki adımları gerçekleştirir:

1. ObjectDataSource's `DeleteParameters` değerler atanır
2. ObjectDataSource's `Delete()` yöntemi çağrıldığında, belirtilen kayıt silme
3. GridView kendisi için ObjectDataSource çağırarak rebinds kendi `Select()` yöntemi

Atanan değerlerin `DeleteParameters` değerleri `DataKeyNames` , Sil düğmesine tıklanana satır için alan. Bu nedenle önemlidir, GridView's `DataKeyNames` özelliği doğru olarak ayarlanmış. Eksik olması durumunda `DeleteParameters` değeri atanacak `Nothing` adım 1'de, hangi sırayla değil neden olur silinmiş kayıtları 2. adım.

> [!NOTE]
> `DataKeys` Koleksiyonu depolanan GridView s denetim durumunda, yani `DataKeys` değerleri hatırlanan geri gönderme arasında GridView s görünüm durumu devre dışı bırakılmış olsa bile. Ancak, Görünüm durumu, düzenleme veya silme (varsayılan davranış) destekleyen GridViews etkin kalmasını çok önemlidir. GridView s ayarlarsanız `EnableViewState` özelliğine `false`, düzenleme ve silme davranışı tek bir kullanıcı için sorunsuz çalışır, ancak verileri silme eşzamanlı kullanıcı varsa, bu eşzamanlı kullanıcıların yanlışlıkla olabilir olasılığı vardır silme veya düzenleme kaydeder, bunlar etmedi t düşündüğünüz. My blog girişine bakın [Uyarı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews Bu destek düzenleme ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/posts/10054.aspx), daha fazla bilgi için.


Bu aynı uyarı DetailsViews ve FormViews için de geçerlidir.

GridView için silme özellikleri eklemek için akıllı etiket gidin ve silme etkinleştir onay kutusunu işaretleyin.


![Onay kutusu silme etkinleştir denetleyin](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**Şekil 10**: Checkbox silme etkinleştir denetleyin


Akıllı etiket gelen Enable Deleting onay kutusu denetimi bir CommandField GridView ekler. Bir veya daha fazla aşağıdaki görevleri gerçekleştirmek için düğmelerle GridView sütununda CommandField oluşturur: bir kayıt seçerek, bir kaydını düzenleme ve kayıt silme. Kayıtları seçme ile uygulamada CommandField daha önce gördüğümüz [ana/Ayrıntılar DetailView ile seçilebilir ana GridView kullanarak ayrıntı](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Öğreticisi.

Bir dizi CommandField içeren `ShowXButton` düğmeleri hangi dizi CommandField görüntülenen belirtmek özellikleri. Enable Deleting onay kutusu bir CommandField denetleyerek, `ShowDeleteButton` özelliği `True` GridView'ın sütun koleksiyonuna eklendi.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

Bu noktada, believe, veya değil, GridView silme desteği ekleme ile bitmek! Şekil 11 gösterildiği gibi bir sütun silme düğmeleri tarayıcı üzerinden bu sayfasını ziyaret mevcut olduğunda.


[![CommandField silme düğmeleri bir sütun ekler](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**Şekil 11**: CommandField bir sütun, silme düğmeleri ekler ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


Bu öğretici sıfırdan, kendi tıklatarak bu sayfayı test edilirken oluşturuluyorsa Sil düğmesini bir özel durum oluşturacak. Neden bu özel durumları ortaya çıktı ve bunları gidermek nasıl konusunda bilgi edinmek için okumaya devam edin.

> [!NOTE]
> Bu öğretici ile birlikte gelen yükleme kullanarak boyunca izlemekte varsa, bu sorunları zaten karşıladığından. Ancak, ı ortaya çıkabilecek sorunları ve uygun geçici çözümler tanımlamaya yardımcı olmak için aşağıda listelenen ayrıntılarını okumak için öneririz.


Bir ürün silinmeye çalışılırken bir özel durum, ileti benzer alırsanız, "*ObjectDataSource 'ObjectDataSource1' genel olmayan yöntemi 'parametrelere sahip DeleteProduct' bulamadı: ProductID, özgün\_ ProductID*, "kaldırmak büyük olasılıkla unuttunuz `OldValuesParameterFormatString` ObjectDataSource özelliğinden. İle `OldValuesParameterFormatString` özelliği belirtilen, ObjectDataSource çalışır hem de geçirmek `productID` ve `original_ProductID` giriş parametreleri için `DeleteProduct` yöntemi. `DeleteProduct`, ancak yalnızca bir tek giriş parametresi, bu nedenle kabul özel durum. Kaldırma `OldValuesParameterFormatString` özelliği (veya ayarlamak `{0}`) özgün giriş parametresinde geçirmek denememeniz ObjectDataSource bildirir.


[![ObjectDataSource'taki özelliği temizlenmiş emin olun](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**Şekil 12**: emin `OldValuesParameterFormatString` özelliği sahip olan temizlenmiş Out ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


Kaldırılan olsa bile `OldValuesParameterFormatString` özelliği, hala alırsınız bir özel durum iletisi ürünle silinmeye çalışılırken zaman: "*DELETE deyimi başvuru kısıtlaması ile çakıştı ' FK\_sipariş\_ayrıntıları \_Ürünlerin*. " Northwind veritabanı arasında yabancı anahtar kısıtlamasını içeriyor `Order Details` ve `Products` tablo içinde için bir veya daha fazla kayıt varsa bir ürün sistemden silinemez anlamına gelir, `Order Details` tablo. Her ürün Northwind veritabanı içinde en az bir kayıt olduğundan `Order Details`, biz öncelikle ürünün ilişkili sipariş ayrıntıları kayıtları silene kadar ürünlerden silemiyoruz.


[![Bir yabancı anahtar kısıtlaması ürünleri silinmesini engelliyor](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**Şekil 13**: yabancı anahtar kısıtlaması, silme ürünleri engeller ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


Bizim öğretici için şimdi yalnızca tüm kayıtları silin `Order Details` tablo. Gerçek dünya uygulamada biz ya da gerekir:

- Sipariş ayrıntılarını bilgilerini yönetmek için başka bir ekrana sahip
- Büyütmek `DeleteProduct` belirtilen ürünün sipariş ayrıntılarını silmek için mantığı içerecek şekilde yöntemi
- Belirtilen ürünün sipariş ayrıntılarını silinmesini içerecek şekilde TableAdapter tarafından kullanılan SQL sorgusu değiştirme

Şimdi yalnızca tüm kayıtları silin `Order Details` yabancı anahtar kısıtlaması aşmak için tablo. Visual Studio'da Sunucu Gezgini gidin, sağ tıklayın `NORTHWND.MDF` düğümü ve yeni bir sorgu seçin. Ardından, sorgu penceresinde, şu SQL ifadesini çalıştırın: `DELETE FROM [Order Details]`


[![Sipariş ayrıntılarını tablodan tüm kayıtları silme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**Şekil 14**: tüm kayıtları silin `Order Details` tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


Temizleme sonra `Order Details` Sil düğmesini tıklatarak tablo ürün hatasız siler. Sil düğmesini tıklatarak ürün silmez GridView's emin olmak için kontrol edin `DataKeyNames` özelliği ayarlanmış birincil anahtar alanı (`ProductID`).

> [!NOTE]
> Sil düğmesini tıklatarak geri gönderimin ensues ve kaydı silinir. Yanlışlıkla yanlış sıranın Sil düğmesine tıklayın kolay olduğundan bu tehlikeli olabilir. Sonraki öğreticide bir kaydı silinirken bir istemci tarafı doğrulama ekleme göreceğiz.


## <a name="editing-data-with-the-gridview"></a>GridView ile verileri düzenleme

GridView denetimini silme yanı sıra yerleşik satır düzeyi düzenleme desteği de sağlar. GridView düzenlemeyi desteklemek üzere yapılandırma Düzenle düğmesi bir sütun ekler. Düzenlenebilir olmasını satır satırın Düzenle düğmesi nedenler tıklatarak son kullanıcının açısından bakıldığında, var olan değerleri içeren ve güncelleştirme Düzenle düğmesi ve İptal düğmeleri değiştirme metin kutuları hücreleri kapatılıyor. Kendi istediğiniz değişiklikleri yaptıktan sonra son kullanıcı değişiklikleri kaydetmek için güncelleştirme veya bunları atmak için İptal'i düğmesini tıklatabilirsiniz. Her iki durumda da, güncelleştirme veya İptal'i tıklattıktan sonra GridView önceden düzenleme durumuna geri döner.

Bizim açısından sayfasında geliştirici olarak son kullanıcının belirli bir satır için Düzenle düğmesini tıklattığında geri gönderimin ensues ve GridView aşağıdaki adımları gerçekleştirir:

1. GridView's `EditItemIndex` özelliği, Düzenle düğmesine tıklanana satırın dizini atanır
2. GridView kendisi için ObjectDataSource çağırarak rebinds kendi `Select()` yöntemi
3. Eşleşen satır dizini `EditItemIndex` "düzenleme modunda." işlenir Bu modda, güncelleştirme ve İptal düğmeleri ve BoundFields Düzenle düğmesini almıştır, `ReadOnly` özelliklerdir False (varsayılan), metin kutusuna Web denetimleri olarak çizilir `Text` özellikleri, veri alanları değerler atanır.

Bu noktada sıranın veri herhangi bir değişiklik yapmak son kullanıcı izin vererek tarayıcıya işaretleme döndürülür. Kullanıcı Güncelleştir düğmesini tıklattığında geri gönderimin oluştuğu ve GridView aşağıdaki adımları gerçekleştirir:

1. ObjectDataSource's `UpdateParameters` değerleri GridView'ın düzenleme arabirimine son kullanıcı tarafından girilen değerler atanır
2. ObjectDataSource's `Update()` yöntemi çağrıldığında, belirtilen kayıt güncelleştiriliyor
3. GridView kendisi için ObjectDataSource çağırarak rebinds kendi `Select()` yöntemi

Atanan birincil anahtar değerlerinin `UpdateParameters` adım 1'de belirtilen değerleri gelen `DataKeyNames` özelliği, birincil olmayan anahtar değerleri düzenlenen satır TextBox Web denetimleri metinde alınması ise. Silme ile önemli olduğu gibi GridView's `DataKeyNames` özelliği doğru olarak ayarlanmış. Eksik olması durumunda `UpdateParameters` birincil anahtar değerine değeri atanacak `Nothing` adım 1'de, hangi sırayla değil neden olur. Adım 2'deki güncelleştirilmiş kayıtları.

Düzenleme işlevselliği GridView'ın akıllı etiket düzenlemeyi etkinleştir onay kutusunu işaretleyerek etkin hale getirilebilir.


![Onay kutusu düzenlemeyi etkinleştir denetleyin](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**Şekil 15**: Checkbox düzenlemeyi etkinleştir denetleyin


Düzenlemeyi Etkinleştir onay kutusu, bir CommandField eklenir, (gerekirse) denetleme ve kümesi kendi `ShowEditButton` özelliğine `True`. Daha önce gördüğümüz gibi bir dizi CommandField içeren `ShowXButton` düğmeleri hangi dizi CommandField görüntülenen belirtmek özellikleri. Düzenlemeyi Etkinleştir onay kutusu denetimi ekler `ShowEditButton` varolan CommandField özelliğine:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

Tüm olan ilkel düzenleme desteği eklemek için yoktur. Düzenleme arabirimi Figure16 gösterildiği gibi bunun yerine kaba her BoundField, `ReadOnly` özelliği ayarlanmış `False` (varsayılan) bir metin işlenir. Bu gibi alanları içerir `CategoryID` ve `SupplierID`, diğer tablolarla anahtarları olduğu.


[![Chai s Düzenle düğmesi tıklatıldığında satır düzenleme modunda görüntüler.](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**Şekil 16**: Düzenle düğmesini tıklatarak Chai s düzenleme modunda satır görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


Yabancı anahtar değerleri doğrudan düzenlemek için kullanıcıların isteyen yanı sıra düzenleme arabiriminin arabirimi aşağıdaki yollarla bulunmaması:

- Kullanıcı girerse bir `CategoryID` veya `SupplierID` veritabanında yok `UPDATE` oluşturulması için bir özel duruma neden olan bir yabancı anahtar kısıtlaması ihlal.
- Düzenleme arabirimi tüm doğrulama içermez. Gerekli değeri sağlamıyorsa (gibi `ProductName`), veya sayısal bir değer ("Çok!" girme gibi beklenirken bir dize değeri girin içine `UnitPrice` metin kutusu), bir özel durum. Gelecekteki bir öğretici için düzenleme kullanıcı arabirimi doğrulama denetimleri ekleme inceleyeceksiniz.
- Şu anda *tüm* salt okunur olmayan ürün alanları GridView eklenmesi gerekir. Biz GridView bir alanı kaldırmak için olsaydı, söyleyin `UnitPrice`GridView verileri güncelleştirme ayarı yapılmadı, `UnitPrice` `UpdateParameters` veritabanı kaydının değişeceğinden değeri `UnitPrice` için bir `NULL` değeri. Benzer şekilde, gerekli bir alan varsa, gibi `ProductName`, kaldırılır GridView update ile aynı başarısız olur "*'ProductName' sütunu null değerlere izin vermiyor*" özel durum bahsedilen üstünde.
- Düzenleme arabirimi biçimlendirme istenen için çok bırakır. `UnitPrice` Dört ondalık noktalarıyla gösterilir. İdeal olarak `CategoryID` ve `SupplierID` değerleri, kategoriler ve tedarikçimiz sistemde listesinde DropDownLists içerecektir.

Biz ile şimdi, ancak işlem için Canlı gerekecek tüm eksik bunlar ele sonraki öğreticilerde.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Ekleme, düzenleme ve DetailsView verilerle silme

Önceki eğitimlerine anlatıldığı gibi DetailsView denetimi aynı anda ve GridView gibi bir kayıt görüntüler, düzenleme ve şu anda görüntülenen kaydını silme olanağı sağlar. Düzenleme ve bir DetailsView ve ASP.NET yan iş akışını öğeleri silme ile hem son kullanıcı deneyimi, GridView aynıdır. Burada DetailsView GridView farklıdır, ayrıca yerleşik ekleme desteği sağlamasıdır.

GridView veri değişikliği özelliklerini göstermek için bir DetailsView'a ekleyerek başlayın `Basics.aspx` sayfasında varolan GridView ve varolan ObjectDataSource DetailsView'un akıllı etiket üzerinden bağlamak. Sonraki DetailsView'un Temizle `Height` ve `Width` özellikleri ve akıllı etiket etkinleştirmek disk belleği seçeneğinden kontrol edin. Düzenleme etkinleştirmek için ekleme ve silme desteği, yalnızca akıllı etiket düzenlemeyi etkinleştir, etkinleştirmek ekleme ve silme etkinleştir onay kutularını işaretleyin.


![DetailsView düzenleme, ekleme ve silme desteği için yapılandırma](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**Şekil 17**: DetailsView düzenleme, ekleme ve silme desteği için yapılandırma


Olarak GridView ekleme, düzenleme, ekleme veya silme desteği bir CommandField DetailsView'a aşağıdaki bildirim temelli söz dizimini gösterildiği gibi ekler:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

İçin DetailsView CommandField sütunları koleksiyonunun sonuna varsayılan olarak görünür. INSERT sahip bir satır olarak CommandField görünür satırlar olarak işlenen DetailsView'un alanlarını beri düzenleyebilir ve altındaki DetailsView'un düğmelerini silebilirsiniz.


[![DetailsView düzenleme, ekleme ve silme desteği için yapılandırma](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**Şekil 18**: desteği ekleme ve silme, düzenleme DetailsView'a yapılandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


Sil düğmesini tıklatarak olayların aynı sırası ile başlayan gibi GridView: bir geri gönderme; kendi ObjectDataSource's doldurma DetailsView tarafından izlenen `DeleteParameters` göre `DataKeyNames` değerleri; ve onun ObjectDataSource's çağrısı ile tamamlandı `Delete()` gerçekten ürün veritabanından kaldırır yöntemi. DetailsView'da düzenleme de aynı olan GridView bir şekilde çalışır.

Eklemek için son kullanıcı ile yeni sunulan, tıklatıldığında düğmesini "ekleme modu" DetailsView'da işler "INSERT moduyla" Yeni düğmesi ekleme ve İptal düğmeleri ve yalnızca bu BoundFields almıştır, `InsertVisible` özelliği ayarlanmış `True` (varsayılan) görüntülenir. Tanımlanan gibi otomatik artım alanlar, bu veri alanlarını `ProductID`, sahip kendi [InsertVisible özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) kümesine `False` DetailsView akıllı etiket aracılığıyla veri kaynağına bağlanırken.

Akıllı etiket aracılığıyla DetailsView için bir veri kaynağına bağlanırken, Visual Studio ayarlar `InsertVisible` özelliğine `False` yalnızca otomatik artan alanları için. Salt okunur alanları `CategoryName` ve `SupplierName`, sürece "ekleme modu" kullanıcı arabiriminde görüntülenecek kendi `InsertVisible` özelliği ayarlanmış açıkça `False`. Bu iki alan ayarlamak için bir dakikanızı ayırın `InsertVisible` özelliklerine `False`, alanları Düzenle veya DetailsView'un bildirim temelli söz dizimi aracılığıyla bağlantı akıllı etiket. Şekil 19 gösterir ayarı `InsertVisible` özelliklerine `False` üzerinde alanları Düzenle bağlantısını tıklatarak.


[![Northwind Traders artık Acme Çay sunar](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**Şekil 19**: Northwind Traders şimdi sunar Acme Çay ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


Ayar sonra `InsertVisible` özelliklerini, view `Basics.aspx` sayfasında bir tarayıcıda ve yeni düğmesini tıklatın. Şekil 20 gösterir DetailsView yeni içecek eklerken bizim ürün satırına Acme Çay.


[![Northwind Traders artık Acme Çay sunar](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**Şekil 20**: Northwind Traders şimdi sunar Acme Çay ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


Acme Çay için ayrıntıları girerek ve Ekle düğmesine tıkladıktan sonra geri gönderimin ensues ve yeni kayıt eklenir `Products` veritabanı tablosu. Bu DetailsView veritabanı tablosunda oldukları sırayla ürünleri listeler olduğundan, biz ürün yeni ürün görmek için son sayfasında gerekir.


[![Acme Çay ayrıntılarını](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**Şekil 21**: Acme Çay ayrıntılarını ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> DetailsView'un [CurrentMode özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) görüntülenmesini arabirimi gösterir ve aşağıdaki değerlerden biri olabilir: `Edit`, `Insert`, veya `ReadOnly`. [DefaultMode özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) DetailsView sonra bir düzen döndürür veya ekleme modu tamamlanmış ve modu Ekle veya Düzenle kalıcı olarak olup bir DetailsView görüntülemek için yararlı olduğunu gösterir.


GridView onunla aynı sınırlamalara gelen ekleme ve düzenleme DetailsView'un özellikleri tıklatın ve noktası yaşar: kullanıcının var girmesi gerekir `CategoryID` ve `SupplierID` textbox değerlerini; arabirimi oturumda tüm doğrulama mantığını; tüm izin verme ürün alanları `NULL` değerleri veya bir varsayılan yok ekleme arabirimi vb. veritabanı düzeyinde belirtilen değer'in eklenmesi gerekir.

Genişletme ve makaleleri DetailsView denetimin düzenleme ve arabirimleri de ekleme için uygulanabilir GridView'ın düzenleme arabirimi gelecekte geliştirme teknikleri inceleyeceğiz.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>FormView için daha esnek bir veri değişikliği kullanıcı arabirimini kullanarak

FormView ekleme, düzenleme ve verileri silme için yerleşik destek sunar, ancak alanları yerine şablonları kullandığından BoundFields veya veri sağlamak için GridView ve DetailsView denetimleri tarafından kullanılan CommandField eklemek için bir yer yok değişiklik arabirimi. Bunun yerine, kullanıcı toplamak için Web denetimleri yeni bir öğe eklerken giriş veya mevcut bir yeni yanı sıra düzenleme düzenleme, silme, INSERT, Update ve İptal düğmeleri bu arabirim için uygun şablonları el ile eklenmesi gerekir. Neyse ki, Visual Studio otomatik olarak gereken Arabirimi FormView aracılığıyla kendi akıllı etiket aşağı açılan listeden bir veri kaynağına bağlanırken oluşturur.

Bu teknikler göstermek için bir FormView ekleyerek başlayın `Basics.aspx` sayfasında ve FormView'ın akıllı etiketten önceden oluşturulmuş ObjectDataSource bağlayın. Bu oluşturacak bir `EditItemTemplate`, `InsertItemTemplate`, ve `ItemTemplate` FormView kullanıcının giriş ve düğmesi Web denetimleri için yeni toplama TextBox Web denetimleri ile için düzenleme, silme, INSERT, Update ve İptal düğmeleri. Ayrıca, FormView's `DataKeyNames` özelliği ayarlanmış birincil anahtar alanı (`ProductID`) ObjectDataSource tarafından döndürülen nesne. Son olarak, FormView'ın akıllı etiket etkinleştirmek sayfalama seçeneğinde denetleyin.

Aşağıdaki bildirim temelli biçimlendirme FormView için 's gösterir `ItemTemplate` FormView ObjectDataSource bağlı sonra. Varsayılan olarak, her bir Boole olmayan değer ürün alan bağlı `Text` her bir Boolean değerini alan bir etiket Web denetimi özelliği (`Discontinued`) bağlı `Checked` özelliği devre dışı onay kutusu Web denetimi. Tıklatıldığında belirli FormView davranışı tetiklemek yeni, düzenleme ve silme düğmeleri için sırayla kesinlik temelli, kendi `CommandName` değerleri ayarlanabilir `New`, `Edit`, ve `Delete`sırasıyla.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

Şekil 22 gösterir FormView's `ItemTemplate` bir tarayıcıdan görüntülendiğinde. Her ürün alanı altındaki Yeni, düzenleme ve silme düğmeleri ile listelenir.


[![Her ürün alanı birlikte yeni Defaut FormView ItemTemplate listeler, düzenleme ve silme düğmeleri](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**Şekil 22**: Defaut FormView `ItemTemplate` listeler her ürün alanı boyunca yeni, düzenleme ve silme düğmeleri ile ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


GridView ve Sil düğmesini veya tüm düğmesi, LinkButton veya ImageButton tıklatarak DetailsView gibi `CommandName` özelliği Sil nedenler geri gönderimin ayarlanmışsa, ObjectDataSource's doldurur `DeleteParameters` FormView üzerinde 's göre`DataKeyNames`değer ve ObjectDataSource's çağırır `Delete()` yöntemi.

Düzenle düğmesine tıklandığında geri gönderimin ensues ve veriler için DataSet'e `EditItemTemplate`, düzenleme arabirimi çizmek için sorumlu olduğu. Bu arabirim, Güncelleştir ve İptal düğmeleri birlikte verileri düzenleme için Web denetimleri içerir. Varsayılan `EditItemTemplate` tarafından oluşturulan Visual Studio içeren herhangi bir otomatik artım alan için bir etiket (`ProductID`), her bir Boole olmayan değerini alan için metin kutusuna bir ve onay kutusu her bir Boolean değerini alan için bir. Bu davranış, otomatik olarak oluşturulan BoundFields GridView ve DetailsView denetimlerindeki çok benzer.

> [!NOTE]
> FormView'in otomatik olarak oluşturulmasını küçük bir sorun `EditItemTemplate` , metin kutusuna Web denetimleri salt okunur gibi bu alanlar için işler olan `CategoryName` ve `SupplierName`. Bu hesap nasıl göreceğiz kısa süre içinde.


TextBox denetimleri `EditItemTemplate` sahip kendi `Text` özelliği kullanımının bunların karşılık gelen verileri alan değerine bağlı *iki yönlü veri bağlaması*. İki yönlü veri bağlaması, gösterilen tarafından `<%# Bind("dataField") %>`, her iki veri bağlamasını şablona veri bağlama sırasında ve ekleme veya düzenleme kayıtları ObjectDataSource'nın parametreleri doldurulurken gerçekleştirir. Diğer bir deyişle, kullanıcı düzenleme düğmesinden tıkladığında `ItemTemplate`, `Bind()` yöntemi belirtilen veri alan değeri döndürür. Kullanıcı kendi değişiklikleri yapar ve güncelleştirme tıklar sonra değerleri kullanarak belirtilen veri alanları karşılık gelen arka gönderilen `Bind()` ObjectDataSource için 's uygulanan `UpdateParameters`. Alternatif olarak, tek yönlü databinding gösterilen tarafından `<%# Eval("dataField") %>`, yalnızca şablon için veri bağlama sırasında veri alan değerlerini alır ve mu *değil* kullanıcı tarafından girilen değerleri veri kaynağının parametreleri geri göndermede döndürür.

FormView ait aşağıdaki bildirim temelli biçimlendirmeyi gösterir `EditItemTemplate`. Unutmayın `Bind()` yöntemi, burada databinding sözdiziminde kullanılır ve güncelleştirme ve iptal düğmesi Web denetimleriniz kendi `CommandName` özellikleri uygun şekilde ayarlanmış.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

Bizim `EditItemTemplate`, bu işaret, biz bunu kullanmayı denerseniz, durum için bir özel duruma neden olur. Sorun `CategoryName` ve `SupplierName` alanları TextBox Web denetimlerini gibi işlenir `EditItemTemplate`. Biz ya da bu metin kutuları için etiketleri değiştirebilir veya tamamen kaldırmak gerekir. Şimdi yalnızca bütünüyle silene `EditItemTemplate`.

Düzenle düğmesi ayrıntılarını tıklatıldıktan sonra Şekil 23 bir tarayıcıda FormView gösterir. Unutmayın `SupplierName` ve `CategoryName` gösterilen alanları `ItemTemplate` biz yalnızca bunlardan kaldırılmış olarak artık mevcut olmayan `EditItemTemplate`. Güncelleştirme düğmesine tıklandığında FormView GridView ve DetailsView denetimleri aynı sırada adımın üzerinden devam eder.


[![Varsayılan olarak EditItemTemplate her düzenlenebilir ürün alan bir metin kutusu veya onay kutusu olarak gösterir.](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**Şekil 23**: varsayılan olarak `EditItemTemplate` gösterir her düzenlenebilir ürün alanı olarak bir metin kutusu veya onay kutusunu ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


Ne zaman Ekle düğmesine tıklandığında FormView's `ItemTemplate` geri gönderimin ensues. Ancak, yeni bir kayıt eklendiği hiçbir veri FormView bağlıdır. `InsertItemTemplate` Arabirimi ekleme ve İptal düğmeleri birlikte yeni bir kayıt eklemek için Web denetimleri içerir. Varsayılan `InsertItemTemplate` tarafından oluşturulan Visual Studio içeren her bir Boole olmayan değerini alan bir metin kutusu ve her bir Boole değeri alanı, otomatik olarak oluşturulan için benzer bir onay kutusu `EditItemTemplate`kullanıcının arabirim. TextBox denetimleri sahip kendi `Text` özelliği iki yönlü veri bağlamasını kullanma bunların karşılık gelen verileri alan değerine bağlı.

FormView ait aşağıdaki bildirim temelli biçimlendirmeyi gösterir `InsertItemTemplate`. Unutmayın `Bind()` yöntemi, burada databinding sözdiziminde kullanılır ve INSERT ve iptal düğmesi Web denetimleriniz kendi `CommandName` özellikleri uygun şekilde ayarlanmış.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

FormView'in otomatik olarak oluşturulmasını ile subtlety yok `InsertItemTemplate`. TextBox Web denetimleri bile salt okunur gibi alanlar için özellikle, oluşturulan `CategoryName` ve `SupplierName`. İle gibi `EditItemTemplate`, bu kutularındaki metinleri kaldırın ihtiyacımız `InsertItemTemplate`.

Şekil 24 FormView Acme kahve yeni bir ürün eklerken bir tarayıcıda gösterir. Unutmayın `SupplierName` ve `CategoryName` gösterilen alanları `ItemTemplate` biz yalnızca onlara kaldırılmış olarak artık mevcut değil. Aynı adımlar dizisini DetailsView denetimi olarak aracılığıyla FormView kazançlar Ekle düğmesine tıklandığında, yeni bir kayıt ekleme `Products` tablo. Bunu eklendikten sonra Şekil 25 FormView Acme kahve ürünün ayrıntılarını gösterir.


[![FormView'ın ekleme arabirimi InsertItemTemplate belirler.](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**Şekil 24**: `InsertItemTemplate` FormView's ekleme arabirimi belirler ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![Yeni ürün, Acme kahve ayrıntıları FormView görüntülenir](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**Şekil 25**: yeni ürün, Acme kahve ayrıntılarını FormView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


Salt okunur ayırarak, düzenleme ve üç ayrı şablonlara arabirimleri ekleme FormView DetailsView ve GridView daha hassas bir denetime sahip bu arabirimleri sağlar.

> [!NOTE]
> Gibi DetailsView'un, FormView `CurrentMode` özelliği görüntülenmesini arabirimi gösterir ve kendi `DefaultMode` özelliği bir düzen sonra FormView döndürür modunu gösterir veya ekleme işlemi tamamlandı.


## <a name="summary"></a>Özet

Bu öğreticide ekleme, düzenleme ve GridView, DetailsView ve FormView kullanarak verileri silme temelleri incelendi. Bu denetimlerin üç belirli bir düzeyde veri Web denetimleri ve ObjectDataSource sayesinde ASP.NET sayfasındaki tek satırlık bir kod yazmak zorunda kalmadan kullanılabilir yerleşik veri değişikliği özellikleri sağlar. Ancak, basit noktası ve teknikleri işleme oldukça frail ve naïve veri değişikliği kullanıcı arabirimi tıklatın. Doğrulama sağlamak için programlı değerleri ekleme, düzgün biçimde özel durumları işleme, kullanıcı arabirimini özelleştirmek ve vb. Biz bevy sonraki birkaç öğreticileri açıklanan teknikleri kullanır gerekir.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](limiting-data-modification-functionality-based-on-the-user-cs.md)
> [sonraki](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
