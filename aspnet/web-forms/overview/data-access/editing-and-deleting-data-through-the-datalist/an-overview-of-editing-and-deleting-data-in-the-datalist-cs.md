---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: "Düzenleme ve silme (C#) DataList verilerde bir genel bakış | Microsoft Docs"
author: rick-anderson
description: "Yerleşik düzenleme ve yetenekleri silme DataList eksik, ancak bu öğreticide, düzenleme ve o silme destekleyen DataList oluşturma göreceğiz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b3067c5a6bcf81a35f66d43886c9b116a0ef7d8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Düzenleme ve silme (C#) DataList verilerde bir genel bakış
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) veya [PDF indirin](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Yerleşik düzenleme ve yetenekleri silme DataList eksik, ancak bu öğreticide, düzenleme ve silme temel alınan verileri destekleyen DataList oluşturma göreceğiz.


## <a name="introduction"></a>Giriş

İçinde [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) biz Aranan ekleme, güncelleştirme ve uygulama mimarisi, bir ObjectDataSource ve GridView, DetailsView ve FormView kullanarak verileri silmek ne Öğreticisi denetler. ObjectDataSource ve bu üç veri Web denetimleri, basit veri değişikliği arabirimler uygulama bir ek olduğu ve yalnızca bir onay kutusu akıllı etiket gelen ticking söz konusu. Yazılacak gerekli kodu yok.

Ne yazık ki, düzenleme ve silme GridView denetiminde devralınmış özellikleri yerleşik DataList eksik. Bu eksik bildirim temelli veri kaynağı denetimleri ve Kodsuz veri değişikliği sayfaları kullanılamaz olduğunda DataList relic ASP.NET, önceki sürümünden olduğunu olgu bölümü son işlevdir. ASP.NET 2.0 DataList kutunun dışında aynı veri değişikliği özellikleri GridView sunmaz olsa da, biz bu tür işlevleri içerecek şekilde ASP.NET 1.x teknikleri kullanabilirsiniz. Bu yaklaşım biraz kod gerektiriyor ancak bu öğreticide anlatıldığı gibi DataList bazı olaylar ve özellikler bu işlemde yardımcı olmak için yerinde sahiptir.

Bu öğreticide, düzenleme ve silme temel alınan verileri destekleyen DataList oluşturma göreceğiz. Sonraki öğreticileri, daha gelişmiş düzenleme ve senaryoları silme, giriş alanını doğrulama dahil olmak üzere, düzgün biçimde veri erişimi veya iş mantığı katmanları ve benzeri oluşturulan özel durumları işleme inceleyeceksiniz.

> [!NOTE]
> DataList gibi yineleyici denetim kutusu işlevselliği ekleme, güncelleştirme veya silme için dışı eksik. Bu tür işlevselliği eklenebilir, ancak DataList özelliklerini ve aşağıdakiler gibi özelliklerinden eklenmesini basitleştirmek Yineleyicideki bulunamadı olaylarını içerir. Bu nedenle, Bu öğretici ve düzenleme ve silme bakın gelecekteki olanları kesinlikle DataList odaklanır.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>1. adım: oluşturma düzenleme ve silme öğreticileri Web sayfaları

Güncelleştirme ve DataList verileri silme keşfetme başlamadan önce öncelikle Bu öğretici ve sonraki birkaç olanları yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `EditDeleteDataList`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![ASP.NET sayfaları öğreticileri için ekleme](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Şekil 1**: öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `EditDeleteDataList` klasör, alt bölümde öğreticileri listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Son olarak, girişleri olarak sayfaları eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme DataList ve yineleyici sonra ana/ayrıntı raporları eklemeniz `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde öğelerini düzenleme ve silme öğreticileri DataList şimdi içerir.


![Site Haritasını girişleri düzenleme ve silme öğreticileri DataList şimdi içerir.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Şekil 3**: Site Haritası girişleri düzenleme ve silme öğreticileri DataList şimdi içerir.


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>2. adım: Teknikleri için güncelleştirme ve silme verileri inceleniyor.

Perde altında GridView ve ObjectDataSource birlikte çalışmak için düzenleme ve silme GridView verilerle kadar kolaydır. ' Da anlatıldığı gibi [ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) öğretici, bir satır s güncelleştirme düğmesine tıklandığında, GridView otomatik olarak ikiyönlüveribağlamasıkullanılanalanlarıatar`UpdateParameters` kendi ObjectDataSource koleksiyonunu ve o ObjectDataSource s çağırır `Update()` yöntemi.

Sadly, DataList yerleşik Bu işlevlerden herhangi birini sağlamaz. ObjectDataSource s parametreleri ve, kullanıcı s değerleri atandığından emin olmak için bizim sorumluluğu olan kendi `Update()` yöntemi çağrılır. Bu çaba bize yardımcı olmak için aşağıdaki özellikleri ve olayları DataList sağlar:

- **[ `DataKeyField` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  güncelleştirme veya silme, biz DataList her öğe benzersiz şekilde tanımlamak gerekir. Görüntülenen verileri birincil anahtar alanı için bu özelliği ayarlayın. Bunun yapılması s DataList doldurmak [ `DataKeys` koleksiyonu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) belirtilen `DataKeyField` her DataList öğesi için değeri.
- **[ `EditCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  ateşlenir düğme, LinkButton veya ImageButton, `CommandName` özelliği ayarlanmış düzenleme tıklandığında.
- **[ `CancelCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  ateşlenir düğme, LinkButton veya ImageButton, `CommandName` özelliği ayarlanmış iptal tıklandığında.
- **[ `UpdateCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  ateşlenir düğme, LinkButton veya ImageButton, `CommandName` özelliği ayarlanmış güncelleştirme tıklandığında.
- **[ `DeleteCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  ateşlenir düğme, LinkButton veya ImageButton, `CommandName` özelliği ayarlanmış Delete tıklandığında.

Bu özellikleri ve olayları kullanarak, biz güncelleştirmek ve DataList verileri silmek için kullanabileceğiniz dört yaklaşım vardır:

1. **ASP.NET 1.x teknikler kullanılarak** DataList ASP.NET 2.0 ve ObjectDataSources önce vardı ve güncelleştirmek ve verileri programlı bir şekilde aracılığıyla tamamen silmek kullanabilirsiniz. Bu teknik ObjectDataSource tamamen ditches ve biz verilerin DataList görüntülemek için veriler alınırken hem de iş mantığı katmanı doğrudan bağlamak, gerektirir ve güncelleştirme veya kayıt silme.
2. **Seçme, güncelleştirme ve silme için sayfasında tek bir ObjectDataSource denetimi kullanarak** GridView s devralınmış düzenleme ve yetenekleri silme DataList eksik, ancak orada s t geçebiliriz herhangi bir neden eklemek bunları kendisini. Bu yaklaşım biz bir ObjectDataSource GridView örneklerde olduğu gibi kullanın, ancak bir olay işleyicisi DataList s oluşturmalısınız `UpdateCommand` burada ayarlarız ObjectDataSource s parametreleri ve çağrı olay kendi `Update()` yöntemi.
3. **ObjectDataSource Denetimi seçme, ancak güncelleştirme ve silme doğrudan karşı BLL kullanarak** seçenek 2 kullanırken, biraz kod yazmaya ihtiyacımız `UpdateCommand` parametre değerleri vb. atama olay. Bunun yerine, size seçmek için ObjectDataSource kullanarak takılıyor ancak (ile seçenek 1 gibi) doğrudan BLL karşı güncelleştirme ve silme çağrıları yapma. ObjectDataSource s atama değerinden daha okunabilir koduna müşteri adayları BLL ile doğrudan arabirim oluşturarak verileri güncelleştirme tam olarak `UpdateParameters` ve çağırma kendi `Update()` yöntemi.
4. **Birden çok ObjectDataSources aracılığıyla bildirime kullanarak** biraz kod tüm önceki üç yaklaşımlar gerektirir. D, bunun yerine mümkün olduğunca çok tanımlayıcı sözdizimi olarak kullanmaya devam, son seçenek sayfada birden çok ObjectDataSources eklemektir. İlk ObjectDataSource BLL verileri alır ve DataList bağlar. Güncelleştirmek için başka bir ObjectDataSource eklendi, ancak doğrudan DataList içinde s eklenen `EditItemTemplate`. Destek silme dahil, henüz başka bir ObjectDataSource içinde gerekli `ItemTemplate`. Bu yaklaşımda, bunlar ObjectDataSource s kullanım katıştırılmış `ControlParameters` bildirimli olarak kullanıcı giriş denetimlerini ObjectDataSource s parametreleri bağlamak için (bunları program aracılığıyla DataList s belirtmek zorunda yerine `UpdateCommand` olay işleyicisi). Bu yaklaşım hala biraz kod katıştırılmış ObjectDataSource s çağırmak için ihtiyacımız gerektirir `Update()` veya `Delete()` komutu ancak gerektiriyorsa şu ana kadar'den küçük diğer üç yaklaşıyor. Burada dezavantajı genel okunabilirlik detracting page up birden çok ObjectDataSources dağıtmayı emin olur.

Her zaman sadece Bu yaklaşımlardan birini kullanmak için zorunlu, çünkü çoğu esneklik sağlar ve DataList bu deseni uyum sağlamak için ilk olarak tasarlandığından d ı 1 seçeneğini belirleyin. DataList ASP.NET 2.0 veri kaynağı denetimleri ile çalışmak için genişletilmişse olsa da, tüm genişletilebilirlik noktaları veya verilerin resmi ASP.NET 2.0 Web denetimleri (GridView, DetailsView ve FormView) özellikleri yok. Seçenek 2-4 arası merit ancak değildir.

Bu ve gelecekteki düzenleme ve öğreticiler silme bir ObjectDataSource verileri almak için görüntülemek ve güncelleştirmek ve verileri (3. seçenek) silmek için BLL aramaları yönlendirmek için kullanır.

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>3. adım: DataList ekleme ve onun ObjectDataSource yapılandırma

Bu öğreticide, ürün bilgilerini listeler ve her ürün için kullanıcı adını ve fiyat düzenleyin ve ürün tamamen silmek için olanak sağlar. DataList oluşturacağız. Özellikle, biz bir ObjectDataSource kullanarak görüntülemek için kayıtları almak ancak güncelleştirmeyi gerçekleştirir ve Eylemler arabirim oluşturarak BLL ile doğrudan silin. DataList düzenleme ve silme olanağı uygulama hakkında endişelenmeniz önce ilk salt okunur arabiriminde ürünleri görüntülemek için sayfanın alma s olanak tanır. Biz bu yana ullanıcı adımları incelenmesi önceki eğitimlerine ı bunları daha hızlı bir şekilde devam.

Başlangıç açarak `Basics.aspx` sayfasındaki `EditDeleteDataList` klasörü ve Tasarım Görünümü'nden bir DataList sayfasına ekleyin. Ardından, yeni ObjectDataSource DataList s akıllı etiketten oluşturun. Ürün verilerle çalışıyoruz beri kullanacak şekilde yapılandırın `ProductsBLL` sınıfı. Alınacak *tüm* ürünleri seçin `GetProducts()` yöntemi seçme sekmesinde.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![GetProducts() yöntemi kullanılarak ürün bilgilerini döndürür](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Şekil 5**: ürün bilgileri kullanarak dönmek `GetProducts()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


GridView gibi DataList yeni veri eklemek için tasarlanmamıştır; Bu nedenle, seçin (hiçbiri) Ekle sekmesinde aşağı açılan listeden seçeneği. Ayrıca bu yana güncelleştirmeleri ve silme BLL programlı olarak gerçekleştirilir (hiçbiri) UPDATE ve DELETE sekmelerini seçin.


[![Aşağı açılan listeler ObjectDataSource s ekleme, güncelleştirme ve silme sekmeler (hiçbiri) ayarlandığını onaylayın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Şekil 6**: aşağı açılan listeler ObjectDataSource s ekleme, güncelleştirme ve silme sekmeler (hiçbiri) ayarlandığını onaylayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


ObjectDataSource yapılandırdıktan sonra Designer'a döndürme Son'u tıklatın. Biz ObjectDataSource yapılandırma, Visual Studio otomatik olarak tamamlarken son örneklerde görüldüğü ullanıcı oluşturur bir `ItemTemplate` DropDownList, her veri alanlarının görüntüleme. Bunun yerine `ItemTemplate` yalnızca s ürün adı ve fiyat görüntüleyen bir. Ayrıca, `RepeatColumns` özelliğini 2.

> [!NOTE]
> ' Da anlatıldığı gibi *genel bakış, ekleme, güncelleştirme ve silme veri* mimarimizin gerektirir biz kaldırmak ObjectDataSource kullanarak verileri değiştirirken öğretici `OldValuesParameterFormatString` ObjectDataSource s özelliğinden bildirim temelli biçimlendirme (veya varsayılan değer olarak, sıfırlama `{0}`). Bu öğreticide, ancak ObjectDataSource yalnızca veri almak için kullanıyoruz. Bu nedenle, biz ObjectDataSource s değiştirmek zorunda kalmazsınız `OldValuesParameterFormatString` özellik değeri (olsa da, mevcut değil t ölçeklenme Bunu yapmak için).


DataList varsayılan değiştirildikten sonra `ItemTemplate` özelleştirilmiş bir ile bildirim temelli biçimlendirme sayfanızda aşağıdakine benzer görünmelidir:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 7'de görüldüğü gibi DataList iki sütun her ürün için ürün adı ve birim fiyat görüntüler.


[![Ürün adları ve fiyatlarını bir iki sütun DataList görüntülenir](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Şekil 7**: ürün adları ve fiyatlarını bir iki sütun DataList görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> Güncelleştirme ve silme işlemi için gerekli olan bir dizi DataList vardır ve bu değerleri görünüm durumuna depolanır. DataList oluşturma, düzenleme veya verileri silme destekler, bu nedenle, bu DataList s Görünüm durumunun etkin gereklidir.  
>   
>  Akıllı duruma okuyucu düzenlenebilir GridViews, DetailsViews ve FormViews oluştururken, Görünüm durumu devre dışı bulamadığımız hatırlayın. ASP.NET 2.0 Web denetimleri içerebilir olmasıdır *denetim durumu*, hangi durumu görünüm durumu, ancak kabul gerekli gibi Geri göndermeler arasında kalıcıdır.


GridView durumda yalnızca Önemsiz durum bilgilerini atlar ancak (düzenleme ve silme için gerekli durumu içeren) bir denetim durumu tutar görünümü devre dışı bırakılıyor. ASP.NET 1.x zaman çerçevesi içinde oluşturulan DataList denetim durumu kullanmaz ve bu nedenle Görünüm durumunun etkin olması gerekir. Bkz: [denetim durumu vs. Görünüm durumu](https://msdn.microsoft.com/library/1whwt1k7.aspx) amacı hakkında daha fazla bilgi denetim durumu ve nasıl görünüm durumundan farklıdır.

## <a name="step-4-adding-an-editing-user-interface"></a>4. adım: bir düzenleme kullanıcı arabirimi ekleme

GridView denetiminin (BoundFields, CheckBoxFields, TemplateFields ve benzeri) alanlarının bir koleksiyonu oluşur. Bu alanların kendi oluşturulan biçimlendirmenin kullanıcıların modlarını bağlı olarak ayarlayabilirsiniz. Örneğin, salt okunur moddayken, metin olarak veri alan değeri bir BoundField görüntüler; düzenleme modundayken, metin kutusuna Web işler, Denetim `Text` özelliği, veri alan değeri atanır.

DataList diğer yandan öğelerinden şablonlar kullanarak oluşturur. Salt okunur öğeleri kullanılarak işlenir `ItemTemplate` aracılığıyla öğeleri düzenleme modunda işlenir ancak `EditItemTemplate`. Bu noktada, bizim DataList yalnızca sahip bir `ItemTemplate`. İhtiyacımız eklemek için öğe düzeyinde düzenleme işlevleri desteklemek için bir `EditItemTemplate` düzenlenebilir öğe için görüntülenecek biçimlendirme içerir. Bu öğreticide, metin kutusuna Web denetimleri s ürün adı ve birim fiyat düzenleme için kullanacağız.

`EditItemTemplate` Bildirimli olarak veya Tasarımcısı aracılığıyla (DataList s akıllı etiketten Şablonları Düzenle seçeneğini seçerek) oluşturulabilir. Şablonları Düzenle seçeneğini kullanmak için önce akıllı etiket Şablonları Düzenle bağlantıya tıklayın ve ardından `EditItemTemplate` öğeyi aşağı açılan listeden.


[![DataList s EditItemTemplate ile çalışacak biçimde iptal et](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Şekil 8**: s DataList çalışmak için kabul `EditItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Ardından, ürün adı yazın: ve fiyat: araç iki TextBox sürükleyin `EditItemTemplate` Tasarımcı arabirimde. Metin kutuları ayarlamak `ID` özelliklerine `ProductName` ve `UnitPrice`.


[![Ürün adı ve fiyat için metin kutusu ekleyin](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Şekil 9**: bir ürünün s adı metin kutusuna ve fiyat ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


Karşılık gelen ürün veri alan değerlerini bağlamak ihtiyacımız `Text` iki metin kutuları özelliklerini. Metin kutuları akıllı etiketleri, veri bağlamaları Düzenle bağlantısına tıklayın ve ardından uygun veri alanı ile ilişkilendirin `Text` Şekil 10'da gösterildiği gibi özelliği.

> [!NOTE]
> Bağlama sırasında `UnitPrice` TextBox s fiyat veri alanına `Text` alanın biçime, para birimi değeri olarak (`{0:C}`), genel bir sayı (`{0:N}`), veya biçimlendirilmemiş bırakın.


![Metin kutuları metin özelliklerini ProductName ve UnitPrice veri alanları bağlama](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Şekil 10**: bağlama `ProductName` ve `UnitPrice` veri alanları için `Text` özelliklerine metin kutuları


Şekil 10 veri bağlamaları Düzenle iletişim kutusunda nasıl mu fark *değil* TemplateField GridView veya DetailsView ya da FormView şablonda düzenleme olduğunda mevcut iki yönlü veri bağlaması onay kutusunu içerir. İki yönlü veri bağlaması özelliğine karşılık gelen ObjectDataSource s otomatik olarak atanacak giriş Web denetimi girilen değerin izin `InsertParameters` veya `UpdateParameters` ekleme ya da veri güncelleştirilirken. Daha sonra Bu öğreticide, her değiştirir ve verileri güncelleştirmek hazır kullanıcı yapar sonra anlatıldığı gibi DataList iki yönlü veri bağlamayı desteklemez, bu metin kutuları programlı olarak erişmek ihtiyacımız `Text` özellikleri ve değerlerine geçirin uygun `UpdateProduct` yönteminde `ProductsBLL` sınıfı.

Son olarak, güncelleştirme eklemek ihtiyacımız ve İptal düğmeleri `EditItemTemplate`. İçinde gördüğümüz gibi [ana/Ayrıntılar DataList ile bir madde işaretli listesi, ana kayıtları kullanarak ayrıntı](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) öğretici, bir düğme, LinkButton veya ImageButton yüklendiğinde, `CommandName` özelliği ayarlanmış gelen yineleyici ya da DataList içinde tıklandığında Yineleyici veya DataList s `ItemCommand` olayı oluşturulur. DataList için ise `CommandName` özelliği, belirli bir değere ayarlandığında, ek bir olay de oluşturulabilir. Özel `CommandName` özellik değerleri dahil, başka şeylerin yanı sıra:

- Başlatır iptal `CancelCommand` olayı
- Başlatır Düzenle `EditCommand` olayı
- Başlatır güncelleştirme `UpdateCommand` olayı

Bu olaylar oluşturulur göz önünde bulundurmanız *ek olarak* `ItemCommand` olay.

Eklemek `EditItemTemplate` iki düğmesi Web denetimleri, biri olan `CommandName` güncelleştirme ve iptal etmeyi ayarlamak diğer s ayarlanır. Bu iki düğmesi Web denetimleri ekledikten sonra Tasarımcı aşağıdakine benzer görünmelidir:


[![Güncelleştirme ekleyin ve İptal düğmeleri EditItemTemplate için](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Şekil 11**: eklemek güncelleştirmek ve İptal düğmeleri `EditItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


İle `EditItemTemplate` tam, DataList s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>5. adım: Düzenleme moduna geçmek için tesisat ekleme

Bu noktada bizim DataList aracılığıyla tanımlanan düzenleme arabirime sahip kendi `EditItemTemplate`; ancak, burada s ürün s bilgilerini düzenlemek istediği belirtmek için şu anda bir yolu sayfamızı ziyaret eden kullanıcı. Tıklatıldığında, işler, DataList düzenleme modunda öğesi, her ürün için Düzenle düğmesini eklemeniz gerekir. Bir Düzenle düğmesi eklemeye başlayın `ItemTemplate`, Tasarımcısı aracılığıyla veya bildirimli olarak. Düzenle düğmesi s ayarlamak emin olmanız `CommandName` özelliği düzenlenemiyor.

Bu Düzenle düğmesini ekledikten sonra bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. Bu ayrıca ile her ürün listesini Düzenle düğmesi içermelidir.


[![Güncelleştirme ekleyin ve İptal düğmeleri EditItemTemplate için](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Şekil 12**: eklemek güncelleştirmek ve İptal düğmeleri `EditItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Düğmesini tıklatarak geri gönderimin neden olur, ancak mu *değil* düzenleme moduna listeleme ürün getirin. Ürün düzenlenebilir olmasını sağlamak için için ihtiyacımız var:

1. S DataList ayarlamak [ `EditItemIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) dizini için `DataListItem` , Düzenle düğmesi yalnızca tıklattınız.
2. DataList verileri yeniden bağlayın. DataList yeniden işlenmiş olduğunda `DataListItem` , `ItemIndex` s DataList ile karşılık gelen `EditItemIndex` kullanarak kılacak kendi `EditItemTemplate`.

S DataList itibaren `EditCommand` Düzenle düğmesine tıklandığında olay harekete, oluşturma bir `EditCommand` aşağıdaki kod ile olay işleyicisi:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

`EditCommand` Olay işleyicisi türünde bir nesne geçirilen `DataListCommandEventArgs` kendi ikinci giriş parametresi olarak bir başvuru içeren `DataListItem` , Düzenle düğmesine tıklanana (`e.Item`). Olay işleyicisi ilk s DataList ayarlar `EditItemIndex` için `ItemIndex` düzenlenebilir, `DataListItem` ve sonra s DataList çağırarak DataList verileri rebinds `DataBind()` yöntemi.

Bu olay işleyicisi ekledikten sonra sayfasını bir tarayıcıda yeniden ziyaret. Şimdi Düzenle düğmesini tıklatmak, Tıklatılan ürün düzenlenebilir (bkz. Şekil 13) yapar.


[![Düzenle düğmesi yapar düzenlenebilir ürün tıklatarak](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Şekil 13**: Düzenle düğmesini tıklatmak ürün düzenlenebilir yapar ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>6. adım: kullanıcı s değişiklikler kaydediliyor

Düzenlenen ürün s güncelleştirme veya İptal düğmeleri tıklamak hiçbir şey bu noktada yapmaz; ihtiyacımız s DataList için olay işleyicileri oluşturmak için bu işlevsellik eklemek için `UpdateCommand` ve `CancelCommand` olaylar. Başlangıç oluşturarak `CancelCommand` düzenlenen ürün s iptal düğmesine tıklandığında ve DataList önceden düzenleme durumuna döndürme ile görevli yürütecek olay işleyicisi.

Tüm salt okunur modda alt öğelerini işlemek DataList olmasını için ihtiyacımız var:

1. S DataList ayarlamak [ `EditItemIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) mevcut olmayan bir dizine `DataListItem` dizini. `-1`güvenli bir seçenek beri olan `DataListItem` dizinleri Başlat `0`.
2. DataList verileri yeniden bağlayın. Hayır itibaren `DataListItem` `ItemIndex` es karşılık DataList s `EditItemIndex`, tüm DataList bir salt okunur modda işlenir.

Bu adımları aşağıdaki olay işleyicisi kodla gerçekleştirilebilir:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Bu eklenmesiyle öğesini tıklatarak önceden düzenleme durumuna DataList iptal düğmesi döndürür.

İhtiyacımız tamamlamak için son olay işleyicisi `UpdateCommand` olay işleyicisi. Bu olay işleyicisi gerekir:

1. Düzenlenen ürün s yanı sıra kullanıcı tarafından girilen ürün adı ve fiyat programlı olarak erişmek `ProductID`.
2. Uygun çağırarak güncelleştirme işlemini başlatmak `UpdateProduct` içinde aşırı `ProductsBLL` sınıfı.
3. S DataList ayarlamak [ `EditItemIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) mevcut olmayan bir dizine `DataListItem` dizini. `-1`güvenli bir seçenek beri olan `DataListItem` dizinleri Başlat `0`.
4. DataList verileri yeniden bağlayın. Hayır itibaren `DataListItem` `ItemIndex` es karşılık DataList s `EditItemIndex`, tüm DataList bir salt okunur modda işlenir.

Adım 1 ve 2 kullanıcı s değişiklikleri kaydetmek için sorumlu; Adım 3 ve 4 değişiklikleri kaydedildi ve başlığında gerçekleştirilen adımlar aynıdır sonra bu DataList önceden düzenleme durumuna geri döndürmek `CancelCommand` olay işleyicisi.

Güncelleştirilmiş ürün adı ve fiyatı almak için kullanılacak ihtiyacımız `FindControl` program aracılığıyla metin kutusuna Web denetimleri içinde başvurusu yönteme `EditItemTemplate`. Ayrıca düzenlenen ürün s almak ihtiyacımız `ProductID` değeri. Biz başlangıçta ObjectDataSource DataList bağlı, Visual Studio s DataList atanan `DataKeyField` birincil anahtar değerine özelliğine veri kaynağından (`ProductID`). Bu değer daha sonra DataList s alınabilir `DataKeys` koleksiyonu. Emin olmak için bir dakikanızı ayırın `DataKeyField` özelliği ayarlanmış gerçekten `ProductID`.

Aşağıdaki kod, dört adımları uygular:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Olay işleyicisi tarafından düzenlenen üründe s okuma başlatır `ProductID` gelen `DataKeys` koleksiyonu. Ardından, iki metin kutuları içinde `EditItemTemplate` başvurulan ve bunların `Text` yerel değişkenlerde depolanan özellikleri `productNameValue` ve `unitPriceValue`. Kullanırız `Decimal.Parse()` değerini okumaya yöntemi `UnitPrice` olması durumunda girilen değerin böylece metin kutusuna bir para birimi simgesini varsa, bunu hala doğru içine dönüştürülebilir bir `Decimal` değeri.

> [!NOTE]
> Değerleri `ProductName` ve `UnitPrice` metin kutuları metin özellikleri belirtilen bir değeri varsa metin kutuları productNameValue ve unitPriceValue değişkenlere yalnızca atanabilir. Aksi takdirde değerini `Nothing` değişkenleri, verileri bir veritabanıyla güncelleştirme etkisi kullanıldığı `NULL` değeri. Diğer bir deyişle, bizim kodu dönüştürür değerlendirir boş veritabanı dizelere `NULL` GridView, DetailsView ve FormView denetimleri düzenleme arabiriminde, varsayılan davranıştır değerleri.


Okumanızı sonra `ProductsBLL` s sınıfı `UpdateProduct` yöntemi çağrıldığında, ürün s adında geçirme, fiyat, ve `ProductID`. Olay işleyicisi tam aynı mantık olarak kullanarak önceden düzenleme durumuna DataList döndürerek tamamlandıktan `CancelCommand` olay işleyicisi.

İle `EditCommand`, `CancelCommand`, ve `UpdateCommand` olay işleyicileri tamamlamak, ziyaretçi adını ve bir ürünün fiyat düzenleyebilirsiniz. Şekil 14-16 eylemde düzenleme bu iş akışı gösterilmektedir.


[![İlk ziyaret sayfası, tüm ürünleri salt okunur modda olduğunda](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Şekil 14**: ilk sayfasını ziyaret ettiğinizde, tüm ürünleri salt okunur modda olan ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Bir ürün adı veya fiyat s güncelleştirmek için Düzenle düğmesini tıklatın.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Şekil 15**: Ürün s adı ya da fiyat güncelleştirmek için Düzenle düğmesini tıklatın ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Değer değiştirdikten sonra geri dönmek için salt okunur modda güncelleştirme'yi tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Şekil 16**: sonra değiştirme değeri, salt okunur moduna Güncelleştir'i tıklatın ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>7. adım: Sil yetenekleri ekleme

Bir DataList delete yetenekleri ekleme adımlarını düzenleme yetenekleri eklemek için benzerdir. Kısacası, Delete düğme eklemek için ihtiyacımız `ItemTemplate` , tıklatıldığında:

1. Karşılık gelen ürün s okur `ProductID` aracılığıyla `DataKeys` koleksiyonu.
2. Silme çağırarak gerçekleştirir `ProductsBLL` s sınıfı `DeleteProduct` yöntemi.
3. DataList verileri rebinds.

Bir Delete düğmesi ekleyerek başlayın s izin `ItemTemplate`.

Düğme tıklatıldığında, `CommandName` Edit, Update ya da iptal başlatır s DataList `ItemCommand` birlikte ek bir olay Olay (örneğin, Düzen kullanırken `EditCommand` olayı de oluşturulur). Benzer şekilde, herhangi bir düğme, LinkButton veya DataList ImageButton olan `CommandName` özelliği ayarlanmış nedenler silmek için `DeleteCommand` tetiklenecek olay (ile birlikte `ItemCommand`).

Düzenle düğmesi yanındaki Sil düğmesini eklemek `ItemTemplate`, ayar kendi `CommandName` silme özelliği. Bu düğme denetim ekledikten sonra s DataList `ItemTemplate` Tanımlayıcı Sözdizimi gibi görünmelidir:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Ardından, olay işleyicisi DataList s oluşturmak `DeleteCommand` olay, aşağıdaki kodu kullanarak:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Sil düğmesini tıklatarak geri gönderimin neden olur ve s DataList ateşlenir `DeleteCommand` olay. Olay işleyicisinde tıklatılan ürün s `ProductID` değeri erişilen `DataKeys` koleksiyonu. Ardından, ürün çağırarak silinir `ProductsBLL` s sınıfı `DeleteProduct` yöntemi.

Ürün sildikten sonra biz DataList verileri rebind önemli s (`DataList1.DataBind()`), aksi takdirde DataList yalnızca silindi ürün göstermeye devam eder.

## <a name="summary"></a>Özet

DataList noktası eksik ve düzenleme ve desteği tarafından GridView keyif silme tıklatın olsa da, kısa bir bit kod, bu özellikleri içerecek şekilde geliştirilebilir. Bu öğreticide, silinebilir ve adı ve fiyat düzenlenemiyor ürünleri iki sütun listesi oluşturmak nasıl gördük. Olan sağlasa da uygun Web denetimleri de dahil olmak üzere, düzenleme ve silme desteği ekleme `ItemTemplate` ve `EditItemTemplate`karşılık gelen olay işleyicileri oluşturma, kullanıcı tarafından girilen ve birincil anahtar değerlerinin okuma ve iş arabirim Mantığı katmanı.

Temel düzenleme ve DataList olanağı silme ekledik olsa da, daha gelişmiş özellikleri eksik. Örneğin, olup olmadığını hiç giriş alanını doğrulama - çok fiyatı kullanıcının girdiği pahalı, bir özel durum tarafından oluşturulur `Decimal.Parse` çok Dönüştür çalışılırken içine pahalı bir `Decimal`. Benzer şekilde, bir sorun varsa iş mantığı veri veya veri erişim katmanları güncelleştirilirken kullanıcı ile standart hata ekranı sunulur. Onay Sil düğmesine her tür bir ürün yanlışlıkla silinmesi tüm çok olasıdır.

Gelecekte düzenleme kullanıcı iyileştirmek nasıl göreceğiz öğreticileri karşılaşırsınız.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Zack Can, Ken Pespisa ve Randy Etikan yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](performing-batch-updates-cs.md)
