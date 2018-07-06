---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: İçerik sayfasından ana sayfa (VB) ile etkileşim kurma | Microsoft Docs
author: rick-anderson
description: Ana sayfa kodunda özellikleri içerik sayfasının vb. kümeden nasıl yöntemleri çağırmak inceler.
ms.author: aspnetcontent
ms.date: 07/11/2008
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 41cfd5e65fbe0a5ef4930ce3a4c84556cedcdfd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807654"
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>İçerik sayfasından ana sayfa (VB) ile etkileşim kurma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Ana sayfa kodunda özellikleri içerik sayfasının vb. kümeden nasıl yöntemleri çağırmak inceler.


## <a name="introduction"></a>Giriş

Önceki öğreticide içerik sayfası program aracılığıyla kendi ana sayfası ile etkileşim sağlamak nasıl incelenir. Geri çağırma beş en son listelenen bir GridView denetimi içerecek şekilde ana sayfaya güncelleştirdik ürünleri eklendi. Ardından kullanıcının yeni bir ürün ekleyebilirsiniz içerik sayfası oluşturduk. İçerik sayfası, yeni ürün ekledikten sonra böylece yeni eklenen ürün verilebilir, GridView yenilemek için ana sayfa istemek gerekli. Bu işlev, yenilenen verileri GridView'a bağlı olduğunu ana sayfasına genel bir yöntem ekleyerek ve ardından içerik sayfasından bu yöntem çağırma gerçekleştirilmiştir.

İçerik sayfasından içerik ve ana sayfa etkileşim yaygın form kaynaklanır. Ancak, ana sayfanın geçerli içerik sayfası eylemlere rouse mümkündür ve ayrıca içerik sayfasında görüntülenen verileri değiştirmek kullanıcıların kullanıcı arabirimi öğeleri ana sayfaya içeriyorsa, bu işlevselliğin gerekli olabilir. Ürün bilgileri GridView denetimi görüntüler ve bir düğme içeren bir ana sayfa denetlemek, tıklandığında bir içerik sayfasını düşünün, tüm ürünleri fiyatları iki katına çıkar. Örnek önceki öğreticide gibi GridView, böylece yeni fiyatlar görüntüler düğmesine tıklandığında çift fiyat sonra yenilenmesi gerekiyor, ancak isteğe bağlı olarak bu senaryoda, içerik sayfası eylemlere rouse gereken ana sayfa olur.

Bu öğretici, ana sayfa içerik sayfada tanımlı işlevleri çağırmak nasıl açıklar.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Bir olay ve olay işleyicileri aracılığıyla program tabanlı etkileşimlerin instigating

Ana sayfadan içerik sayfası işlevleri çağırma güvenmelidir değerinden daha zor olur. İçerik sayfası, program tabanlı etkileşimlerin içerik sayfasından instigating zaman tek bir ana sayfa olduğundan bizim elden genel yöntemleri ve özellikleri nelerdir biliyoruz. Ancak, bir ana sayfa pek çok farklı içerik sayfası her biri kendi özellikleri ve yöntemleri kümesini sahip olabilir. Nasıl daha sonra kod size hangi içerik sayfası çalışma zamanına kadar çağrılacak bilmediğinizde kendi içerik sayfasındaki bazı eylemleri gerçekleştirmek için ana sayfasında yazabiliriz?

Düğme denetimi gibi bir ASP.NET Web denetimi göz önünde bulundurun. Bir düğme denetimi, ASP.NET sayfaları herhangi bir sayıda görünebilir ve bir mekanizma olarak bu sayfayı tıklanan uyarabilir gerekiyor. Kullanılarak elde edilir *olayları*. Özellikle, düğme denetimini başlatır, `Click` olay tıklandığında; bu bildirim düğmesi içeren ASP.NET sayfasında isteğe bağlı olarak yanıt verebilir bir *olay işleyicisi*.

Bu aynı düzeni, içerik sayfalarını bir ana sayfa tetikleyici işlevselliği sağlamak için kullanılabilir:

1. Bir olay ana sayfaya ekleyin.
2. Ana sayfaya, içerik sayfası ile iletişim kurmak gerektiğinde bu olayı Tetikle. Ana sayfaya, içerik sayfası kullanıcı fiyatlar katladı uyarı gerekiyorsa, Fiyatlar kadına hemen sonra Örneğin, kendi olay oluşturulması.
3. Bazı işlemler yapması için gereken bu içerik sayfalarında bir olay işleyicisi oluşturun.

Bu öğreticinin geri kalanında bu bölümde açıklanan örnek uygular; yani, Fiyatlar çift veritabanında olduğu ürünleri listeler bir içerik sayfası ve bir düğme içeren bir ana sayfa denetler.

## <a name="step-1-displaying-products-in-a-content-page"></a>1. adım: Bir içerik sayfasındaki ürünleri görüntüleme

Bizim ilk iş sırası, Northwind veritabanındaki olduğu ürünleri listeler bir içerik sayfasını oluşturmaktır. (Northwind veritabanına projeye önceki öğreticide eklediğimiz [ *içerik sayfasından ana sayfa ile etkileşim*](interacting-with-the-master-page-from-the-content-page-vb.md).) Yeni bir ASP.NET sayfasına ekleyerek başlangıç `~/Admin` adlı klasöre `Products.aspx`ettiğinizden emin olmak için bağlama `Site.master` ana sayfa. Şekil 1, bu sayfa Web sitesine eklendikten sonra Çözüm Gezgini gösterir.


[![Yönetici klasöre yeni bir ASP.NET sayfası ekleyin](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Şekil 01**: yeni bir ASP.NET sayfasına ekleme `Admin` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


Geri çağırma [ *ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) adlı bir özel taban sayfası sınıfı oluşturduk öğretici `BasePage` oluşturan başlığı değilse açıkça ayarlayın. Git `Products.aspx` sayfa arka plan kod sınıfı ve varsa, türetilen `BasePage` (yerine gelen `System.Web.UI.Page`).

Son olarak, güncelleştirme `Web.sitemap` bu ders için bir giriş eklemek için dosya. Altında aşağıdaki işaretlemeyi ekleyin `<siteMapNode>` ana sayfa etkileşim Ders içeriği için:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

Bu ek `<siteMapNode>` öğesi derslerde yansıtılır (bkz: Şekil 5) listesi.

Geri dönüp `Products.aspx`. İçerik denetimi için `MainContent`bir GridView denetimi ekleyin ve adlandırın `ProductsGrid`. Adlı yeni bir SqlDataSource denetimi GridView bağlamak `ProductsDataSource`.


[![GridView yeni SqlDataSource denetime bağlama](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Şekil 02**: yeni bir SqlDataSource denetimi GridView bağlamak ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Böylece Northwind veritabanı kullanan Sihirbazı'nı yapılandırın. Önceki öğreticide çalışılan sonra zaten adlı bir bağlantı dizesi olmalıdır `NorthwindConnectionString` içinde `Web.config`. Bu bağlantı dizesi, Şekil 3'te gösterildiği gibi aşağı açılan listeden seçin.


[![SqlDataSource Northwind veritabanını kullanacak şekilde yapılandırma](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Şekil 03**: Northwind veritabanına kullanılacak SqlDataSource yapılandırın ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


Ardından, veri kaynağı denetimin belirtin `SELECT` Ürünler tablosu aşağı açılan listeden seçerek ve döndüren deyimi `ProductName` ve `UnitPrice` sütunları (bkz. Şekil 4). İleri'ye tıklayın ve ardından veri kaynağı Yapılandırma Sihirbazı'nı tamamlamak için son.


[![ProductName ve UnitPrice alanları ürünleri bir tablo döndürür.](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Şekil 04**: iade `ProductName` ve `UnitPrice` alanlarını `Products` tablo ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


Tüm İşte bu kadar kolay! Sihirbazı tamamladıktan sonra Visual Studio iki BoundFields SqlDataSource denetimi tarafından döndürülen iki alan yansıtmak üzere GridView ekler. GridView ve SqlDataSource denetim biçimlendirme izler. Şekil 5 bir tarayıcıdan görüntülendiğinde sonuçları gösterilmektedir.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![Her ürün ve bunun ücreti GridView listelenir](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Şekil 05**: her ürün ve bunun ücreti GridView listelenir ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> GridView görünümünü oluşturan temiz çekinmeyin. Görüntülenen UnitPrice değeri bir para birimi olarak biçimlendirme ve kılavuz görünümü geliştirmek için arka plan renklerini ve yazı tiplerini kullanarak bazı öneriler içerir. Görüntüleme ve ASP.NET veri biçimlendirme hakkında daha fazla bilgi için benim [çalışma ile verileri öğretici serisinin](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>2. adım: ana sayfa için bir çift fiyatları düğme ekleme

Bizim sonraki görev eklemek için bir düğme Web Denetimi ana sayfasında, tıklandığında uygulamak çift veritabanındaki tüm ürün fiyatı olacaktır. Açık `Site.master` ana sayfa ve bir düğmeyi tasarımcıya altındaki yerleştirmek için araç kutusundan sürükleyin `RecentProductsDataSource` SqlDataSource denetimi önceki öğreticide ekledik. Düğmenin ayarlamak `ID` özelliğini `DoublePrice` ve kendi `Text` "Çift ürün fiyatlar" özelliği.

Ardından, SqlDataSource denetimi adlandırma ana sayfasına ekleme `DoublePricesDataSource`. Bu SqlDataSource yürütmek için kullanılan `UPDATE` tüm fiyatlar çift deyimi. Özellikle, ayarlanacak ihtiyacımız kendi `ConnectionString` ve `UpdateCommand` uygun bir bağlantı dizesi özellikleri ve `UPDATE` deyimi. Ardından bu SqlDataSource denetimin çağırmak ihtiyacımız `Update` yöntemi zaman `DoublePrice` düğmesine tıklandığında. Ayarlanacak `ConnectionString` ve `UpdateCommand` özellikleri SqlDataSource denetimi seçin ve ardından Özellikler penceresine gidin. `ConnectionString` Özelliği zaten depolanan Bu bağlantı dizelerini listeler `Web.config` ; bir aşağı açılan listeden seçin `NorthwindConnectionString` seçeneği Şekil 6'da gösterildiği gibi.


[![SqlDataSource NorthwindConnectionString kullanmak için yapılandırma](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Şekil 06**: SqlDataSource kullanılacak yapılandırma `NorthwindConnectionString` ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Ayarlanacak `UpdateCommand` özelliği, Özellikler penceresinde veUpdateQuery seçeneğini bulun. Bu onay kutusu seçildiğinde, bu özellik, bir düğme olarak üç görüntüler. Şekil 7'de gösterilen komut ve parametre Düzenleyicisi iletişim kutusunu görüntülemek için bu düğmeye tıklayın. Aşağıdaki komutu yazın `UPDATE` iletişim kutusunun metin INTO deyimi:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Bu deyimi yürütüldüğünde, çift `UnitPrice` her kayıt için değer `Products` tablo.


[![SqlDataSource'nın UpdateCommand özelliğini ayarlayın](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Şekil 07**: ayarlamak SqlDataSource'nın `UpdateCommand` özelliği ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Bu özellikleri ayarladıktan sonra bildirim temelli düğmesi ve SqlDataSource denetimlerinizi biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Kalan tek şey çağırmak için kendi `Update` yöntemi zaman `DoublePrice` düğmesine tıklandığında. Oluşturma bir `Click` için olay işleyicisi `DoublePrice` düğmesine ve ardından aşağıdaki kodu ekleyin:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Bu işlevi test etmek için ziyaret `~/Admin/Products.aspx` sayfasında 1. adımda oluşturduğunuz ve "Double ürün fiyatlar" düğmesine tıklayın. Düğmeye tıklandığında geri göndermeye neden olur ve yürüten `DoublePrice` düğmenin `Click` fiyatlar tüm ürünlerin Katlama, olay işleyicisi. Sayfayı daha sonra yeniden oluşturulur ve biçimlendirme döndürülen ve tarayıcıda yeniden görüntülenir. "Çift ürün fiyatlar önce" düğmesine tıklandığını GridView içerik sayfasında, ancak aynı fiyatlar listeler. Başlangıçta GridView içinde yüklenen veriler görünüm durumuna depolanmaz, yani bu üzerinde Geri göndermeler aksi belirtilmedikçe yüklenmez durumuna sahip olmasıdır. Farklı bir sayfasını ziyaret edin ve sonra geri dönüp `~/Admin/Products.aspx` sayfa güncelleştirilmiş fiyatları göreceksiniz.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>3. adım: bir olay olduğunda fiyatları yükseltme iki katına

Çünkü, GridView `~/Admin/Products.aspx` sayfa hemen yansıtmıyor Katlama fiyatı, kullanıcı bunlar "Çift ürün fiyatlar" düğmesine tıklayın değil, ya da işe yaramadı anlaşılır şekilde düşünebilirsiniz. Düğmeye tıklandığında daha fazla bilgi süreleri ve fiyatı tekrar tekrar Katlama birkaç çalıştıklarında. Bu kılavuzun içeriği sağlamak için ihtiyacımız sorunu gidermek için sayfayı görüntülemek yeni fiyatlar sonra hemen işledi.

Bu öğreticide daha önce açıklandığı gibi kullanıcı tıkladığında, ana sayfada bir olay oluşturabilmelidir ihtiyacımız `DoublePrice` düğmesi. Bir dizi ilgi çekici bir şey gerçekleşen diğer sınıfı (etkinlik abonelerinden) bilgilendirmek için bir yol için bir sınıf (bir olay yayımcısı) bir olaydır. Bu örnekte, olay yayımcısı ana sayfa.; Bu içerik hakkında ne zaman dikkatli olun sayfalarının `DoublePrice` düğmesine tıklandığında aboneler.

Bir sınıf oluşturarak, bir olaya abone olan bir *olay işleyicisi*, gerçekleştirilen olaya yanıt olarak yürütülen bir yöntemi. Yayımcı kendisinin başlatır tanımlayarak olayları tanımlayan bir *olay temsilcisini*. Olay temsilcisini olay işleyicisi kabul etmesi gereken giriş parametreleri belirtir. .NET Framework, olay temsilcilerini değil herhangi bir değer döndürür ve iki giriş parametreleri kabul eder:

- Bir `Object`, olay kaynağı tanımlar ve
- Türetilen bir sınıf `System.EventArgs`

Bir olay işleyicisine geçirilen ikinci parametresi, olay hakkında ek bilgiler içerebilir. While temel `EventArgs` sınıfı herhangi bir bilgi başarılı değil, .NET Framework genişleten sınıflar içerir `EventArgs` ve ek özellikleri kapsar. Örneğin, bir `CommandEventArgs` örnek yanıt olay işleyicileri geçirildiğinde `Command` olay ve iki bilgi özellikleri içerir: `CommandArgument` ve `CommandName`.

> [!NOTE]
> Oluşturma hakkında daha fazla bilgi için bkz: oluşturma ve olayları, işleme [olayları ve Temsilciler](https://msdn.microsoft.com/library/17sde2xt.aspx) ve [olay temsilcileri basit İngilizce](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Bir olayı tanımlamak için aşağıdaki sözdizimini kullanın:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Biz yalnızca kullanıcı tıkladığında içerik sayfası uyarı gerekir çünkü `DoublePrice` düğme ve diğer ek bilgileri geçmesi gerekmez, olay temsilcisini kullanabiliriz `EventHandler`, kendi saniye kabul eden bir olay işleyicisi tanımlar parametre türü bir nesne `System.EventArgs`. Ana sayfada bir olay oluşturmak için ana sayfa arka plan kod sınıfına aşağıdaki kod satırını ekleyin:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

Yukarıdaki kodu ortak olay adlı ana sayfasına ekler `PricesDoubled`. Şimdi iki katına fiyatlar sonra bu olayı Tetikle ihtiyacımız var. Bir olayı yükseltmek için aşağıdaki sözdizimini kullanın:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Burada *gönderen* ve *eventArgs* istediğiniz abonenin olay işleyicisine geçirilecek değerlerdir.

Güncelleştirme `DoublePrice` `Click` olay işleyicisi aşağıdaki kod ile:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Önceki örneklerde olduğu gibi `Click` olay işleyicisini başlatır çağırarak `DoublePricesDataSource` SqlDataSource denetimin `Update` fiyatlar tüm ürünlerin çift yöntemi. Aşağıdaki iki olay işleyicisi eklemeler vardır. İlk olarak, `RecentProducts` GridView'ın veri yenilenir. Bu GridView ana sayfaya önceki öğreticide eklendi ve en son eklediğiniz beş ürünleri görüntüler. Biz bu beş ürünler için yalnızca iki katına fiyatlar gösterir, böylece bu kılavuzu yenilemeniz gerekir. Aşağıdaki `PricesDoubled` olayı oluşturulur. Ana Sayfa başvurusu (`Me`) olay işleyicisi, olay kaynağı ve boş gönderilir `EventArgs` nesne olay bağımsız değişkenleri gönderilir.

## <a name="step-4-handling-the-event-in-the-content-page"></a>4. adım: içerik sayfası olayı işleme

Bu noktada ana sayfası oluşturur, `PricesDoubled` olay olduğunda `DoublePrice` düğme denetimi tıklandığında. Ancak bu yalnızca yarı Savaşı, - abone olayı işlemek biz yine de gerekir. Bu iki adımdan oluşur: olay işleyicisi oluşturmayı ve olay kablolama kod ekleyerek olay oluştuğunda olay işleyicisi yürütülür.

Adlı bir olay işleyicisi oluşturarak başlayın `Master_PricesDoubled`. Tanımladığımız nasıl nedeniyle `PricesDoubled` ana sayfasında olay olay işleyicinin iki giriş parametresi türü olmalıdır `Object` ve `EventArgs`sırasıyla. Olay işleyicisi çağrısında `ProductsGrid` GridView'ın `DataBind` kılavuza veriler yeniden bağlamak için yöntemi.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

Olay işleyicisi için kod tamamlandı ancak henüz için yaptığımız ana sayfanın wire `PricesDoubled` bu olay işleyicisi için olay. Bir olay işleyicisine aşağıdaki söz dizimini aracılığıyla olaya abone bağlayan:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Yayımcı* olay sunan nesneye bir başvurudur *eventName*, ve *methodName* abonenin tanımlanan olay işleyicisi adıdır.

Bu olay kablolama kod ilk sayfasını ziyaret edin ve sonraki geri göndermelere yürütülmelidir ve olayı yükleyen önündeki sayfa yaşam döngüsü içindeki bir noktada gerçekleşmelidir. Çok erken sayfa yaşam döngüsü içinde gerçekleşen PreInit aşamasında olay kablolama kodu eklemek için zamanı geldi.

Açık `~/Admin/Products.aspx` oluşturup bir `Page_PreInit` olay işleyicisi:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Bu teknik kod tamamlamak için içerik sayfasından ana sayfayı programlı başvuru ihtiyacımız var. Önceki öğreticide belirtildiği gibi bunu yapmanın iki yolu vardır:

- Geniş yazılmış atama tarafından `Page.Master` uygun ana sayfa türü özelliğine veya
- Ekleyerek bir `@MasterType` yönergesini `.aspx` sayfası ve kesin tür belirtilmiş'ı kullanarak `Master` özelliği.

İkinci yaklaşımda kullanalım. Aşağıdaki `@MasterType` bildirim temelli işaretleme sayfanın en üstüne yönergesi:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Ardından aşağıdaki olay kablolama kod ekleyin `Page_PreInit` olay işleyicisi:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Bu kod bir yerde içerik sayfasındaki GridView yenilenir her `DoublePrice` düğmesine tıklandığında.

Şekil 8 ve 9 bu davranış görülmektedir. Şekil 8 sayfa ilk ziyaret edildiğinde gösterir. Hem de değerleri fiyat Not `RecentProducts` GridView (sol sütunda ana sayfanın) ve `ProductsGrid` GridView (içerik sayfasındaki). Şekil 9 gösterir aynı ekran hemen sonra `DoublePrice` düğmeye tıkladı. Gördüğünüz gibi yeni fiyatlar hem GridViews anında yansıtılır.


[![Başlangıç fiyatı değerleri](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Şekil 08**: ilk fiyat değerleriyle ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Just-Doubled fiyatlar GridViews içinde görüntülenir.](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Şekil 09**: The Just-Doubled fiyatları içinde GridViews görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Özet

İdeal olarak, bir ana sayfa ve içerik sayfalarını birbirlerinden tamamen ayrı ve etkileşimi yok düzeyini gerektirir. Ana sayfa veya ana sayfa veya içerik sayfasındaki değiştirilebilir verileri görüntüleyen içerik sayfası varsa, ancak daha sonra içerik sayfası (veya tersi bir) uyarı ana sayfaya gerekebilir veri görüntüleme güncelleştirilebilmesi için değiştirildiğinde. Önceki öğreticide program aracılığıyla kendi ana sayfası ile etkileşim içerik bir sayfaya nasıl gördük; Bu öğreticide bir ana sayfa başlatma etkileşim sağlamak nasıl incelemiştik.

Program tabanlı etkileşimlerin arasında içerik ve ana sayfa içeriği ya ana sayfa kaynaklanan, ancak kullanılan etkileşim modeli kaynağına bağlıdır. İçerik sayfası tek bir ana sayfa var, ancak bir ana sayfa birçok farklı içerik sayfaları Bunun nedeni, fark vardır. Bir ana sayfa içerik sayfası ile doğrudan etkileşim sağlamak yerine, daha iyi bir yaklaşım bazı eylemleri yapıldığının göstermek için bir olay tetikleyebilir ana sayfaya sahip olmaktır. Bu içerik sayfalarını eylem hakkında dikkatli olay işleyicileri oluşturabilirsiniz.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Erişim ve ASP.NET'te verileri güncelleştirme](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Olaylar ve temsilciler](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [İçerik ve ana sayfalar arasında bilgi geçirme](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [ASP.NET Öğreticilerde verilerle çalışma](../../data-access/index.md)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Suchi Banerjee oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](interacting-with-the-master-page-from-the-content-page-vb.md)
> [İleri](master-pages-and-asp-net-ajax-vb.md)
