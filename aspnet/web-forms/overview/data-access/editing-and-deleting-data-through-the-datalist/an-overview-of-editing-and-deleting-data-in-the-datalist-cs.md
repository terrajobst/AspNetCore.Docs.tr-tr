---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Genel Bakış (C#) DataList'te verileri düzenleme ve silmeye | Microsoft Docs
author: rick-anderson
description: DataList yerleşik düzenleme ve silme özelliklerini azaltır, ancak bu öğreticide, düzenleme ve silme o destekleyen bir DataList oluşturma görüyoruz...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e9268a2ca805bfae2f77e72a131968e09a92b31
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752344"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>(C#) DataList'te verileri düzenleme ve silmeye genel bakış
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) veya [PDF olarak indirin](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> DataList yerleşik düzenleme ve silme özelliklerini azaltır, ancak bu öğreticide, temel alınan verileri düzenleme ve silmeye destekleyen bir DataList oluşturma göreceğiz.


## <a name="introduction"></a>Giriş

İçinde [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) ekleme, güncelleştirme ve uygulama mimarisi, bir ObjectDataSource ve GridView, DetailsView ve FormView kullanarak veri silme nasıl incelemiştik Öğreticisi denetimler. ObjectDataSource ve bu üç veri Web denetimleri sayesinde basit veri değişikliğine arabirimlerini uygulayan bir ek oldu ve yalnızca bir onay akıllı etiket yolunda söz konusu. Yazılması gereken kodu yok.

Ne yazık ki, düzenleme ve silme özelliklerini GridView denetiminde devralınan yerleşik DataList eksik. Bu eksik bildirim temelli bir veri kaynağı denetimleri ve kod kullanmadan veri değişikliği sayfaları kullanılamaz olduğunda DataList ASP.NET, önceki bir sürümünden bir relic olduğunu olgu bölümü son bir işlevdir. DataList ASP.NET 2.0 hazır aynı veri değişikliği özellikleri GridView sağlamaz, ancak Biz bu işlevselliği eklemek için ASP.NET 1.x teknikleri kullanabilirsiniz. Bu yaklaşım bir bit kod gerektirir. ancak, bu öğreticide anlatıldığı gibi DataList bazı olayları ve özelliklerini bu işlemde yardımcı olacak bir yerde sahiptir.

Bu öğreticide, temel alınan verileri düzenleme ve silmeye destekleyen bir DataList oluşturma göreceğiz. Sonraki öğreticiler, daha gelişmiş düzenleme ve silme senaryoları, giriş alan doğrulama dahil olmak üzere, düzgün bir şekilde veri erişimi veya iş mantığı katmanı ve benzeri harekete geçirilen özel durum işleme inceleyeceksiniz.

> [!NOTE]
> DataList gibi Repeater denetiminde ekleme, güncelleme veya silme işlevselliği dışı eksik. Bu işlevselliğin eklenebilir, ancak DataList özelliklerini ve aşağıdakiler gibi özelliklerinden eklenmesini basitleştirmek Yineleyicideki bulunamadı olaylarını içerir. Bu nedenle, Bu öğretici ve düzenleme ve silme bakın gelecekteki vm'lere kesinlikle DataList'te odaklanır.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>1. adım: düzenleme ve silme öğreticiler Web sayfaları oluşturma

Nasıl güncelleştirmek ve DataList verileri silmek keşfetmek başlamadan önce ilk yapmamız gereken Bu öğretici ve sonraki birkaç olanlar için Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırarak s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `EditDeleteDataList`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![ASP.NET sayfaları için öğreticiler Ekle](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Şekil 1**: ASP.NET sayfaları için öğreticiler Ekle


Diğer klasörler gibi `Default.aspx` içinde `EditDeleteDataList` klasör, alt bölümde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Son olarak, girişleri olarak istediğiniz sayfaları eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme DataList ve Repeater ile ana/ayrıntı raporları sonra eklemeniz `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, düzenleme ve silme öğreticiler DataList artık öğeleri içerir.


![Site Haritası girişleri düzenleme ve silme öğreticiler DataList artık içerir.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Şekil 3**: Site Haritası girişleri düzenleme ve silme öğreticiler DataList artık içerir.


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>2. adım: Teknikleri için güncelleştirme ve silme verileri İnceleme

Aslında, GridView ve ObjectDataSource birlikte çalıştığından, GridView ile verileri düzenleme ve silmeye çok kolay. Bölümünde açıklandığı gibi [ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) öğretici, bir satır s güncelleştir düğmesine tıklandığında GridView otomatik olarak iki yönlü veri bağlama kullanılanalanlarındanatar`UpdateParameters` kendi ObjectDataSource koleksiyonu ve ardından bu ObjectDataSource s çağırır `Update()` yöntemi.

Sadly, DataList bu yerleşik işlevleri sağlamaz. Bizim sorumluluk ObjectDataSource s parametreleri ve, kullanıcı s değerleri atandığından emin olup, `Update()` yöntemi çağrılır. Bu çaba bizi yardımcı olması için aşağıdaki özellikleri ve olayları DataList sağlar:

- **[ `DataKeyField` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  güncelleştirmeye veya silmeye, biz DataList'te her öğeyi benzersiz olarak tanımlanabilmesi gerekir. Görüntülenen verileri birincil anahtar alanı için bu özelliği ayarlayın. Bunun yapılması s DataList doldurmak [ `DataKeys` koleksiyon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) belirtilen `DataKeyField` her DataList öğesi için değeri.
- **[ `EditCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  düğme, LinkButton veya ImageButton gerektiğinde ateşlenir olan `CommandName` özelliği düzenleme tıklandığında.
- **[ `CancelCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  düğme, LinkButton veya ImageButton gerektiğinde ateşlenir olan `CommandName` özelliği iptal tıklandığında.
- **[ `UpdateCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  düğme, LinkButton veya ImageButton gerektiğinde ateşlenir olan `CommandName` özelliği güncelleştirme tıklandığında.
- **[ `DeleteCommand` Olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  düğme, LinkButton veya ImageButton gerektiğinde ateşlenir olan `CommandName` özelliği silme tıklandığında.

Bu özellikleri ve olayları kullanarak, güncelleştirme ve DataList verileri silmek için kullanabiliriz dört yaklaşım vardır:

1. **ASP.NET 1.x teknikleri kullanarak** DataList ASP.NET 2.0 ve ObjectDataSources önce vardı ve güncelleştirme ve programlı anlamına gelir verilerine tamamen silmek kullanabilirsiniz. Bu teknik ObjectDataSource tamamen ditches ve biz veri DataList için görüntülenecek veri alma hem de iş mantığı katmanı doğrudan bağlamanızı gerektirir ve kayıt silme veya güncelleştirme.
2. **Sayfasında tek bir ObjectDataSource Denetimi seçme, güncelleştirme ve silme için kullanarak** DataList GridView s devralınan düzenleme ve silme özelliklerini azaltır, ancak orada s t yapabiliriz gerektirecek bir neden ekleyin kendimize içinde. Bu yaklaşımla, biz bir ObjectDataSource GridView örneklerde olduğu gibi kullanın, ancak s DataList için bir olay işleyicisi oluşturmanız gerekir `UpdateCommand` burada ayarlarız ObjectDataSource s parametreler ve çağrı olay kendi `Update()` yöntemi.
3. **ObjectDataSource Denetimi seçme, ancak güncelleştirme ve silme doğrudan karşı BLL kullanarak** 2 seçeneği kullanılırken, biraz kod yazmak ihtiyacımız `UpdateCommand` olay, parametre değerleri ve benzeri atama. Bunun yerine, biz ObjectDataSource seçmek için kullanmaya devam edin ancak güncelleştirme ve silme (1 seçeneği gibi) doğrudan BLL karşı aramalarda. ObjectDataSource s atama değerinden daha okunabilir bir kod için müşteri adaylarını doğrudan BLL ile arabirim oluşturularak verileri güncelleştirme tam olarak `UpdateParameters` ve çağırma kendi `Update()` yöntemi.
4. **Birden çok ObjectDataSources aracılığıyla bildirime kullanarak** tüm önceki üç yaklaşımların bir bit kod gerektirir. D, bunun yerine mümkün olduğunca çok bildirim temelli söz dizimi olarak kullanmaya devam etmek, son seçenek sayfasında birden fazla ObjectDataSources eklemektir. İlk ObjectDataSource BLL verileri alır ve DataList için bağlar. Güncelleştirmek için başka bir ObjectDataSource eklendi, ancak doğrudan DataList içinde s eklenen `EditItemTemplate`. Silme desteği eklemek için henüz başka bir ObjectDataSource içinde gerekli olduğu `ItemTemplate`. Bu yaklaşımda, bunlar ObjectDataSource s kullanım katıştırılmış `ControlParameters` bildirimli olarak kullanıcı girişi denetimlerinden ObjectDataSource s parametreleri bağlamak için (bunları s DataList içinde program aracılığıyla belirtmek zorunda yerine `UpdateCommand` olay işleyicisi). Bu yaklaşım, yine de ihtiyacımız katıştırılmış ObjectDataSource s çağırmak için kod bir bit gerektirir `Update()` veya `Delete()` diğer üç yaklaşıyor değerinden komutu ancak gerektirir kadar. Burada dezavantajı, genel okunabilirliğini detracting page up, birden çok ObjectDataSources dağıtmayı, ' dir.

Yalnızca Bu yaklaşımlardan birini kullanmanız durumunda, en üst düzeyde esneklik sağladığından ve DataList bu düzen uyum sağlamak için ilk olarak tasarlandığından miyim d 1 seçeneğini belirleyin. DataList ASP.NET 2.0 veri kaynağı denetimleri ile çalışmak için genişletilmişse, ancak tüm genişletilebilirlik noktaları veya resmi ASP.NET 2.0 veri Web denetimleri (GridView DetailsView ve FormView) özellikleri yok. Seçenek 2 ile 4 arasındaki merit, ancak değildir.

Bu gelecekteki düzenleme ve silme öğreticiler bir ObjectDataSource veri almak için görüntülemek ve güncelleştirmek ve verileri (seçeneği 3) silmek için BLL aramaları yönlendirmek için kullanır.

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>3. adım: DataList ekleyerek ve kendi ObjectDataSource yapılandırma

Bu öğreticide, ürün bilgilerini listeler ve her ürün için kullanıcı adını ve fiyat düzenleyin ve ürünü silmek için olanağı sağlar. bir DataList oluşturacağız. Özellikle, eder bir ObjectDataSource kullanarak kayıtları almak ancak güncelleştirmeyi gerçekleştirmek ve doğrudan BLL ile arabirim oluşturularak eylemleri sil. DataList düzenleme ve silme olanağı'ı uygulama hakkında endişe önce ilk salt okunur arabiriminde ürünleri görüntülemek için sayfanın alın s olanak tanır. Biz bu yana adımları ve denetlenen önceki öğreticilerde, miyim aracılığıyla hızlı bir şekilde devam.

Başlangıç açarak `Basics.aspx` sayfasını `EditDeleteDataList` klasörü ve Tasarım Görünümü'nden bir DataList sayfaya ekleyin. Ardından, yeni ObjectDataSource DataList s akıllı etiketten oluşturun. Ürün verilerle çalışıyoruz olduğundan, bunu kullanacak şekilde yapılandırmanız `ProductsBLL` sınıfı. Alınacak *tüm* ürünleri seçin `GetProducts()` seçme sekmesinde yöntemi.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![GetProducts() yöntemi kullanılarak ürün bilgilerini döndürür](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Şekil 5**: ürün bilgileri kullanarak dönüş `GetProducts()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


DataList GridView gibi yeni veri eklemek için tasarlanmamıştır; Bu nedenle, seçin (hiçbiri) Ekle sekmesine aşağı açılan listeden seçeneği. Ayrıca güncelleştirme ve silme ile BLL programlı olarak gerçekleştirilecek bu yana (hiçbiri) için UPDATE ve DELETE sekmeleri seçin.


[![Açılan listeler ObjectDataSource s ekleme, güncelleştirme ve silme sekmeler (hiçbiri) ayarlandığını doğrulayın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Şekil 6**: aşağı açılan listeler ObjectDataSource s ekleme, güncelleştirme ve silme sekmeler (hiçbiri) ayarlandığını onaylayın ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


ObjectDataSource yapılandırdıktan sonra tasarımcıya döndüren Son'a tıklayın. Olarak oluşturur ve ObjectDataSource yapılandırmasına, Visual Studio otomatik olarak tamamlarken son örneklerde görülen bir `ItemTemplate` DropDownList için her veri alanı görüntüleme. Bunun yerine `ItemTemplate` yalnızca s ürün adı ve fiyatı görüntüleyen bir. Ayrıca, `RepeatColumns` özelliği 2.

> [!NOTE]
> Bölümünde açıklandığı gibi *genel bakış, ekleme, güncelleştirme ve silme veri* mimarimiz gerektirir emin ediyoruz kaldırmak ObjectDataSource kullanarak verileri değiştirirken öğretici `OldValuesParameterFormatString` ObjectDataSource s özelliği bildirim temelli biçimlendirme (veya varsayılan değerine sıfırlayın `{0}`). Bu öğreticide, ancak ObjectDataSource yalnızca veri almak için kullanıyoruz. Bu nedenle, biz ObjectDataSource s değiştirmek gerekmez `OldValuesParameterFormatString` özellik değeri (olsa da, eklenmemişse t olumsuz Bunu yapmak için).


DataList varsayılan değiştirdikten sonra `ItemTemplate` özelleştirilmiş bir, bildirim temelli biçimlendirme sayfanızda aşağıdakine benzer görünmelidir:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 7 gösterildiği gibi DataList iki sütunlarda her ürün için ürün adını ve birim fiyatı gösterir.


[![Fiyatlar ve ürün adları iki sütunlu DataList görüntülenir](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Şekil 7**: ürün adları ve fiyatlar iki sütunlu DataList görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> DataList güncelleştirme ve silme işlemi için gerekli olan özellikleri vardır ve bu değerleri görünüm durumuna depolanır. Bu nedenle bir DataList oluşturma, düzenleme veya veri silme desteklediğinde, DataList s Görünüm durumunun etkin olmasını gerekli olur.  
>   
>  Kurnaz Okuyucu, biz düzenlenebilir GridViews, DetailsViews ve FormViews oluştururken görünüm durumu devre dışı hatırlayabilirsiniz. ASP.NET 2.0 Web denetimleri içerebilir olmasıdır *denetim durumu*, hangi durumu gibi görünüm durumu, ancak gerekli olarak kabul Geri göndermeler arasında kalıcıdır.


GridView durumda yalnızca Önemsiz durum bilgilerini atlar, ancak (kod düzenleme ve silme için gerekli bir durumu içerir) denetim durumunu koruyan görünümü devre dışı bırakılıyor. DataList ASP.NET 1.x profilleri'nde oluşturduğum denetim durumu kullanmaz ve bu nedenle Görünüm durumunun etkin olması gerekir. Bkz: [denetim durumu vs. Görünüm durumu](https://msdn.microsoft.com/library/1whwt1k7.aspx) denetim durumu ve görünüm durumu benzerlikleri amacı hakkında daha fazla bilgi için.

## <a name="step-4-adding-an-editing-user-interface"></a>4. adım: düzenleme kullanıcı arabirim ekleme

GridView denetiminde (BoundFields, CheckBoxFields, TemplateField ve benzeri) alanlarının bir koleksiyonu oluşur. Bu alanlar, kendi biçimlendirmenin modlarını bağlı olarak ayarlayabilirsiniz. Örneğin, salt okunur modda olduğunda, metin olarak veri alan değeri bir BoundField görüntüler; düzenleme modundayken, bir metin kutusu Web işler özelliği kontrol `Text` özelliği, veri alan değeri atanır.

DataList, diğer taraftan, şablonlarını kullanarak, bir öğe işler. Salt okunur öğeler kullanarak oluşturulur `ItemTemplate` düzenleme modunda öğeleri aracılığıyla işlenir ancak `EditItemTemplate`. Bu noktada, bizim DataList yalnızca sahip bir `ItemTemplate`. İhtiyacımız eklemek için öğe düzeyinde düzenleme işlevleri desteklemek için bir `EditItemTemplate` düzenlenebilir öğe için gösterilecek biçimlendirme içeriyor. Bu öğretici için TextBox Web denetimleri s ürün adı ve birim fiyatı düzenleme için kullanacağız.

`EditItemTemplate` Bildirimli olarak veya Tasarımcısı aracılığıyla (DataList s akıllı etiketten Şablonları Düzenle seçeneğini belirterek) oluşturulabilir. Şablonları Düzenle seçeneğini kullanmak için önce akıllı etiket Şablonları Düzenle bağlantısına tıklayın ve ardından `EditItemTemplate` aşağı açılan listeden bir öğe.


[![DataList s EditItemTemplate çalışmak için iyileştirilmiş](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Şekil 8**: s DataList çalışmak için iyileştirilmiş `EditItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Sonra ürün adını yazın: ve fiyat: ve iki TextBox denetimi araç kutusundan sürükleyin `EditItemTemplate` Tasarımcı arabirimi. Metin kutuları kümesi `ID` özelliklerine `ProductName` ve `UnitPrice`.


[![Fiyat ve ürün adı için bir metin kutusu ekleme](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Şekil 9**: Ürün s ad metin kutusu ve fiyat ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


İlgili ürün veri alan değerlerini bağlamak ihtiyacımız `Text` iki metin kutuları özellikleri. Metin kutuları akıllı etiketleri, veri bağlamaları Düzenle bağlantısına tıklayın ve ardından uygun veri alanıyla ilişkilendirmek `Text` Şekil 10'da gösterildiği gibi özelliği.

> [!NOTE]
> Bağlama sırasında `UnitPrice` veri alanı için metin kutusu s fiyatı `Text` alanı biçimlendirdiğiniz, bir para birimi değeri olarak (`{0:C}`), genel bir sayı (`{0:N}`), veya biçimlendirilmemiş bırakın.


![UnitPrice veri alanları ve ProductName yöneltmek için metin özelliklerini bağlama](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Şekil 10**: bağlama `ProductName` ve `UnitPrice` veri alanları için `Text` özelliklerine metin kutuları


Şekil 10 veri bağlamaları Düzenle iletişim kutusunda nasıl yaptığını fark *değil* GridView veya DetailsView bir TemplateField ya da bir şablon FormView'da düzenlerken mevcut olan iki yönlü veri bağlama onay kutusunu içerir. İki yönlü veri bağlama özelliğini karşılık gelen ObjectDataSource s için otomatik olarak atanacak giriş Web denetimine girilen değer izin verilen `InsertParameters` veya `UpdateParameters` ekleme ya da veri güncelleştirilirken. DataList daha sonra Bu öğreticide, her değiştirir ve verileri güncelleştirmek hazır yaptığında sonra anlatıldığı gibi çift yönlü veri bağlama desteklemiyor, bu metin kutuları programlı olarak erişmek ihtiyacımız `Text` özellikleri ve değerlerine geçirin uygun `UpdateProduct` yönteminde `ProductsBLL` sınıfı.

Son olarak, güncelleştirme eklemek ihtiyacımız ve İptal düğmeleri `EditItemTemplate`. İçinde gördüğümüz gibi [ana/ayrıntı bir Ayrıntılar DataList'i ile bir madde işaretli liste, ana kayıtları kullanarak](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) öğretici, bir düğme, LinkButton veya ImageButton olan `CommandName` özelliği ayarlanmış bir yineleyici veya DataList içinde gelen sonuna tıklatıldığında Yineleyici veya DataList s `ItemCommand` olayı oluşturulur. DataList için ise `CommandName` özelliği, belirli bir değere ayarlandığında, ek bir olay da oluşturulabilir. Özel `CommandName` özellik değerlerini içerir, diğerlerinin yanı sıra:

- Harekete geçirirse iptal `CancelCommand` olay
- Harekete geçirirse Düzenle `EditCommand` olay
- Harekete geçirirse güncelleştirme `UpdateCommand` olay

Bu olaylar oluşturulur akılda tutulması *ek olarak* `ItemCommand` olay.

Ekleme `EditItemTemplate` iki düğme Web denetimleri, olan `CommandName` güncelleştir ve iptal etmek için ayarlama s ayarlanır. Bu iki düğme Web denetimi ekledikten sonra Tasarımcı aşağıdakine benzer görünmelidir:


[![Güncelleştirme ekleme ve İptal düğmeleri EditItemTemplate için](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Şekil 11**: Ekle Güncelleştir ve İptal düğmeleri `EditItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


İle `EditItemTemplate` tam DataList s, bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>5. adım: Düzenleme moduna girmek için tesisat ekleme

Bu noktada bizim DataList aracılığıyla tanımlanan bir düzenleme arabirimi olan kendi `EditItemTemplate`; Bununla birlikte, burada s s ürün bilgilerini düzenlemek istediği belirtmek için şu anda bir yolu sayfamızı ziyaret bir kullanıcı. Düzenle düğmesine tıklandığında, işler, DataList düzenleme modunda öğesi, her bir ürün eklemek ihtiyacımız var. Başlamak için bir düzenleme düğmesi ekleyerek `ItemTemplate`, tasarımcı aracılığıyla veya bildirimli olarak. Düzenle düğmesine s ayarlanacak emin olmanız `CommandName` düzenleme özelliği.

Bu Düzenle düğmesine ekledikten sonra bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. Bu eklenmesiyle, Düzenle düğmesine her ürün listesini içermelidir.


[![Güncelleştirme ekleme ve İptal düğmeleri EditItemTemplate için](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Şekil 12**: Ekle Güncelleştir ve İptal düğmeleri `EditItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Düğmeye tıklandığında geri göndermeye neden olur, ancak mu *değil* düzenleme moduna listeleyen ürün getirin. Ürün düzenlenebilir hale getirmek için biz gerekir:

1. DataList s ayarlamak [ `EditItemIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) dizin değerine `DataListItem` , Düzenle düğmesine yalnızca tıkladı.
2. DataList verileri yeniden bağlayın. DataList yeniden işlenmiş olduğunda `DataListItem` olan `ItemIndex` s DataList'i ile karşılık gelen `EditItemIndex` kullanarak işlenir, `EditItemTemplate`.

DataList s beri `EditCommand` harekete Düzenle düğmesine tıklandığında, oluşturun bir `EditCommand` olay işleyicisi aşağıdaki kod ile:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

`EditCommand` Olay işleyicisi, türünde bir nesne geçirilir `DataListCommandEventArgs` ikinci girdi parametresi, bir başvuru içeren `DataListItem` , Düzenle düğmesine tıkladı (`e.Item`). Olay işleyicisi ilk s DataList ayarlar `EditItemIndex` için `ItemIndex` düzenlenebilir, `DataListItem` ve sonra s DataList çağırarak verileri DataList rebinds `DataBind()` yöntemi.

Bu olay işleyici ekledikten sonra bir tarayıcıda sayfayı yeniden ziyaret edin. Şimdi düzenle düğmesine tıklayarak tıklandı ürün düzenlenebilir (bkz: Şekil 13) sağlar.


[![Düzenle düğmesi yapar düzenlenebilir ürün tıklayarak](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Şekil 13**: Düzenle düğmesine tıklayarak ürün düzenlenebilir yapar ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>6. adım: kullanıcı s değişiklikler kaydediliyor

Düzenlenen ürün s güncelleştirme veya İptal düğmeleri tıklayarak hiçbir şey bu noktada yapmaz; DataList s için olay işleyicilerini oluşturma ihtiyacımız bu işlevselliği eklemek için `UpdateCommand` ve `CancelCommand` olayları. Oluşturarak başlayın `CancelCommand` düzenlenen ürün s iptal düğmesine tıklandığında ve DataList önceden düzenleme durumuna döndürerek ile görevli yürütecek olay işleyicisi.

Tüm öğeleri salt okunur modda işleme DataList sağlamak için biz gerekir:

1. DataList s ayarlamak [ `EditItemIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) var olmayan bir dizine `DataListItem` dizini. `-1` güvenli bir seçenek başlangıcı olduğu `DataListItem` dizinleri başlar `0`.
2. DataList verileri yeniden bağlayın. Hayır beri `DataListItem` `ItemIndex` es karşılık gelen bir DataList s `EditItemIndex`, tüm DataList salt okunur modunda işlenir.

Bu adımları aşağıdaki olay işleyicisine kod ile yapılabilir:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Bu ekleme ile önceden düzenleme durumuna iptal düğmesi döndürür DataList'yı tıklatın.

Son olay işleyicisi tamamlamak için ihtiyacımız olan `UpdateCommand` olay işleyicisi. Bu olay işleyicisi gerekir:

1. Düzenlenen ürün s yanı sıra kullanıcı tarafından girilen ürün adı ve fiyat programlamayla erişme `ProductID`.
2. Uygun çağırarak güncelleştirme işlemini başlatan `UpdateProduct` de aşırı `ProductsBLL` sınıfı.
3. DataList s ayarlamak [ `EditItemIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) var olmayan bir dizine `DataListItem` dizini. `-1` güvenli bir seçenek başlangıcı olduğu `DataListItem` dizinleri başlar `0`.
4. DataList verileri yeniden bağlayın. Hayır beri `DataListItem` `ItemIndex` es karşılık gelen bir DataList s `EditItemIndex`, tüm DataList salt okunur modunda işlenir.

Adım 1 ve 2 kullanıcı s değişiklikleri kaydetmek için sorumludur; Adım 3 ve 4 değişiklikler kaydedildi ve başlığında gerçekleştirilen adımlar aynıdır sonra bu DataList önceden düzenleme durumuna döndürmek `CancelCommand` olay işleyicisi.

Fiyat ve güncelleştirilen ürün adını almak için kullanılacak ihtiyacımız `FindControl` program aracılığıyla metin Web denetimleri içinde başvuru yönteme `EditItemTemplate`. Biz de düzenlenen ürün s iletişime geçmeniz `ProductID` değeri. Biz başlangıçta ObjectDataSource DataList için bağlı olduğunda, Visual Studio s DataList atanan `DataKeyField` birincil anahtar değeri bir özelliğine veri kaynağından (`ProductID`). Bu değer daha sonra s DataList alınabilir `DataKeys` koleksiyonu. Emin olmak için birkaç dakikanızı `DataKeyField` özelliği ayarlanmış gerçekten `ProductID`.

Aşağıdaki kod, dört adımı gerçekleştirir:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Olay işleyicisi düzenlenen ürün s okuyarak başlar `ProductID` gelen `DataKeys` koleksiyonu. İki metin kutuları, İleri `EditItemTemplate` başvurulur ve bunların `Text` yerel değişkenlerinde depolanan özellikleri `productNameValue` ve `unitPriceValue`. Kullandığımız `Decimal.Parse()` değerinden okumak için yöntem `UnitPrice` olması durumunda girilen değerin için metin kutusu olan bir para birimi sembolü, yine de düzgün olarak dönüştürülebilir bir `Decimal` değeri.

> [!NOTE]
> Değerleri `ProductName` ve `UnitPrice` metin kutuları metin özellikleri belirtilen bir değeri varsa metin kutuları productNameValue ve unitPriceValue değişkenlere yalnızca atanmış. Aksi takdirde, değeri `Nothing` değişkenleri, bir veritabanı ile veri güncelleştirme etkisi kullanılan `NULL` değeri. Diğer bir deyişle, Kodumuzun dönüştürür değerlendirir boş dizelere veritabanına `NULL` değerleri FormView GridView ve DetailsView denetimlerini düzenleme arabiriminde varsayılan davranışıdır.


Nokta değerlerini okuma sonra `ProductsBLL` s sınıfı `UpdateProduct` yöntemi çağrıldığında, ürün s adında geçirme, fiyat, ve `ProductID`. Olay işleyicisi olarak tam aynı mantığı kullanarak önceden düzenleme durumuna DataList döndürerek tamamlandıktan `CancelCommand` olay işleyicisi.

İle `EditCommand`, `CancelCommand`, ve `UpdateCommand` olay işleyicileri tamamlamak, ziyaretçi ürünün fiyatı ve adını düzenleyebilirsiniz. Şekil 14-16 Bu düzenleme iş akışı eylemi göster.


[![Sayfa ilk ziyaret, tüm ürünler salt okunur modunda olduğunda](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Şekil 14**: sayfa ilk ziyaret edildiğinde, salt okunur modunda olan tüm ürünler ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Bir ürün adı veya fiyat s güncelleştirmek için Düzenle düğmesini tıklatın.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Şekil 15**: bir ürün adı s ya da fiyat güncelleştirmek için Düzenle düğmesini tıklatın. ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Değer değiştirdikten sonra salt okunur moduna güncelleştirmeye tıklayın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Şekil 16**: sonra değiştirme değeri, salt okunur moduna geri dönmek için Güncelleştir'e tıklayın ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>7. adım: Sil yetenekleri ekleme

DataList için delete yetenekleri ekleme adımlarını düzenleme özellikleri eklemeye benzerdir. Kısacası, silme düğme eklemek için ihtiyacımız `ItemTemplate` , tıklandığında:

1. İlgili ürün s okur `ProductID` aracılığıyla `DataKeys` koleksiyonu.
2. Çağırarak silme gerçekleştirir `ProductsBLL` s sınıfı `DeleteProduct` yöntemi.
3. DataList verileri rebinds.

Sil düğmesini ekleyerek başlayın s izin `ItemTemplate`.

Tıklandığında, bir düğme olan `CommandName` düzenleme, güncelleştirme, ya da iptal s DataList başlatır `ItemCommand` ek bir olay ile birlikte olay (örneğin, Düzen kullanırken `EditCommand` olayı de oluşturulur). Benzer şekilde, herhangi bir düğme, LinkButton veya ImageButton DataList'te olan `CommandName` özelliği nedenleri silmek için ayarlanmış `DeleteCommand` olayının ateşlenmesine neden (ile birlikte `ItemCommand`).

Düzenle düğmesinin yanındaki Sil düğmesi ekleme `ItemTemplate`, ayar, `CommandName` silme özelliği. Bu düğme denetimi ekledikten sonra s DataList `ItemTemplate` bildirim temelli söz dizimi gibi görünmelidir:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Ardından, s DataList için bir olay işleyicisi oluşturun `DeleteCommand` olay, aşağıdaki kodu kullanarak:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Geri göndermeye neden olur ve DataList s harekete Sil düğmesine tıklanarak `DeleteCommand` olay. Olay işleyicisinde tıklandı ürün s `ProductID` değeri erişilen `DataKeys` koleksiyonu. Ardından, ürün çağırarak silinir `ProductsBLL` s sınıfı `DeleteProduct` yöntemi.

Ürün sildikten sonra biz DataList verileri rebind önemli s (`DataList1.DataBind()`), aksi takdirde DataList yalnızca silindi ürün göstermeye devam edecektir.

## <a name="summary"></a>Özet

DataList noktası eksik ve düzenleme ve silme desteği tarafından GridView keyif tıklatın kısa bir bit kod, bu özellikleri içerecek şekilde geliştirilebilir. Bu öğreticide, silinebilir ve ad ve fiyat düzenlenemiyor ürünleri iki sütunlu listesini oluşturmak nasıl gördük. Düzenleme ve silme desteği ekleme olan uygun Web denetimleri de dahil olmak üzere birkaç `ItemTemplate` ve `EditItemTemplate`, karşılık gelen olay işleyicileri oluşturma, kullanıcı tarafından girilen ve birincil anahtar değerlerini okuma ve iş yazılmamış Mantığı katmanı.

Temel düzenleme ve DataList özelliklerini silme ekledik ancak daha gelişmiş özellikleri eksik. Örneğin, giriş alanını doğrulama yoktur - bir kullanıcı çok fiyatı girerse pahalı bir özel durum tarafından oluşturulur `Decimal.Parse` çok dönüştürmek çalışırken içine pahalı bir `Decimal`. Benzer şekilde, bir sorun varsa iş mantığı veri veya veri erişim katmanları güncelleştirilirken bir kullanıcı standart hata ekranı sunulur. Bir ürün yanlışlıkla silme herhangi bir tür Sil düğmesine onayının tüm çok olasılığı yüksektir.

Gelecekte düzenleme kullanıcı geliştirmek nasıl görüyoruz öğreticileri deneyin.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Zack Jones, Ken Pespisa ve Randy Etikan yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](performing-batch-updates-cs.md)
