---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Ana sayfa (C#) içerik sayfasından etkileşimde | Microsoft Docs
author: rick-anderson
description: Ana sayfa kodunda özellikleri içerik sayfasının vb. kümeden nasıl yöntemlerini çağıran inceler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c845c7b0077e6d3fb5ce770029b4f9f48609b17
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>Ana sayfa (C#) içerik sayfasından ile etkileşim kurma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Ana sayfa kodunda özellikleri içerik sayfasının vb. kümeden nasıl yöntemlerini çağıran inceler.


## <a name="introduction"></a>Giriş

Önceki öğretici program aracılığıyla kendi ana sayfa ile etkileşim içerik sayfasını nasıl incelendi. Biz en son beş listelenen GridView denetimini eklemek için ana sayfa güncelleştirilmeden geri çağırma ürünleri eklendi. Ardından kullanıcının yeni bir ürün ekleyebilirsiniz bir içerik sayfasını oluşturduk. Yeni bir ürün ekledikten sonra içerik sayfasını yeni eklenen ürün içerir böylece, GridView yenilemek için ana sayfa istemek üzere gerekli. Bu işlevsellik, yenilenen verileri GridView bağlı olduğunu ana sayfaya genel yöntem ekleyerek veya içerik sayfasından bu yöntemi çağırmadan gerçekleştirilmiştir.

En yaygın formun içeriği ve ana sayfa etkileşim içerik sayfasından kaynaklanır. Ancak, geçerli içerik sayfasını eyleme rouse ana sayfa mümkündür ve ana sayfa ayrıca içerik sayfasında görüntülenen verileri değiştirmek kullanıcıların kullanıcı arabirimi öğeleri içeriyorsa, bu tür işlevselliği gerekli olabilir. GridView içinde ürün bilgilerine denetimi görüntüler ve bir düğmeyi içeren bir ana denetim sayfası, tıklatıldığında bir içerik sayfasını düşünün, tüm ürünleri fiyatlar iki katına çıkar. Örnek önceki öğreticide benzediğini GridView, böylece yeni fiyatlarını görüntüler düğmesine tıklandığında çift fiyat sonra yenilenmesi gerekiyor, ancak isteğe bağlı olarak bu senaryoda, içerik sayfasını eyleme rouse gereken ana sayfa.

Bu öğretici içerik sayfada tanımlı işlevleri çağırma ana sayfa nasıl araştırır.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Olay ve olay işleyicileri aracılığıyla programlı etkileşim instigating

Bir ana sayfa İçerik sayfasını işlevleri yürütmesini daha şekilde daha zordur. Bir içerik sayfasını içerik sayfasından programlı etkileşim instigating olduğunda tek bir ana sayfa olduğundan bizim elden genel yöntemler ve özellikler nelerdir biliyoruz. Bir ana sayfa ancak birçok farklı içerik sayfaları, her biri kendi özellikleri ve yöntemleri kümesini sahip olabilir. Nasıl daha sonra size kod size hangi içerik sayfasını çalışma zamanına kadar çağrılacak bilmiyorsanız, içerik sayfasındaki bazı eylemleri gerçekleştirmek için ana sayfa yazabilirsiniz?

Düğme denetimi gibi bir ASP.NET Web denetimi göz önünde bulundurun. Düğme denetimi ASP.NET sayfaları sayıda görüntülenebilir ve bir mekanizma olarak bu sayfayı tıklattınız uyarabilir gerekiyor. Bu kullanılarak gerçekleştirilir *olayları*. Özellikle, düğme denetimi başlatır, `Click` olay tıklatıldığında; bu bildirim düğmeyi içeren ASP.NET sayfası isteğe bağlı olarak yanıt verebilir bir *olay işleyicisi*.

Bu aynı düzeni içerik sayfalarında bir ana sayfa tetikleyici işlevsellik sağlamak için kullanılabilir:

1. Bir olay ana sayfasına ekleyin.
2. Ana sayfa kendi içerik sayfası ile iletişim kurması için gereken her olayı oluşturun. Ana sayfa kendi içerik sayfası kullanıcı fiyatlar iki katına uyarı gerekiyorsa, Fiyatlar hemen iki katına sonra Örneğin, kendi olayı.
3. Olay işleyici bazı işlem yapmanıza gerek bu içerik sayfalarında oluşturun.

Bu öğreticinin bu kalan girişte ana hatlarıyla verilen örnek uygular; Ayrıca, Fiyatlar çift veritabanındaki ürünleri listeler bir içerik sayfasını ve bir düğmeyi içeren bir ana sayfa denetler.

## <a name="step-1-displaying-products-in-a-content-page"></a>1. adım: Bir içerik sayfasını ürünleri görüntüleme

Bizim ilk iş sırası Northwind veritabanı ürünleri listeler bir içerik sayfasını oluşturmaktır. (Northwind veritabanı projeye önceki öğreticide eklediğimiz [ *içerik sayfasından ana sayfa ile etkileşim*](interacting-with-the-master-page-from-the-content-page-cs.md).) Yeni bir ASP.NET sayfası eklemeye başlayın `~/Admin` adlı klasörü `Products.aspx`, emin bağlamak için yapmayı `Site.master` ana sayfa. Şekil 1, bu sayfa Web sitesine eklendikten sonra Çözüm Gezgini gösterir.


[![Yönetici klasörüne yeni bir ASP.NET sayfa ekleyin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Şekil 01**: yeni bir ASP.NET sayfası Ekle `Admin` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


Uygulamasında geri çağırma [ *başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfasında belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) adlı bir özel ana sayfa sınıf oluşturduğumuz öğretici `BasePage` oluşturan sayfanın başlığı değilse açıkça ayarlayın. Git `Products.aspx` sayfanın arka plandaki kod sınıfı ve sahip öğesinden türetilen `BasePage` (yerine gelen `System.Web.UI.Page`).

Son olarak, güncelleştirme `Web.sitemap` dosya bu ders için bir giriş içerir. Altında aşağıdaki biçimlendirmeleri eklemek `<siteMapNode>` içerik ana sayfa etkileşim ders için:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

Bu ek `<siteMapNode>` öğesi dersleri yansıtılır (bkz. Şekil 5) listesi.

Geri dönüp `Products.aspx`. İçerik denetimi için `MainContent`, GridView denetimini ekleyin ve adını `ProductsGrid`. GridView adlı yeni bir SqlDataSource denetimi bağlamak `ProductsDataSource`.


[![GridView yeni bir SqlDataSource denetimine bağlama](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Şekil 02**: yeni bir SqlDataSource denetimi GridView bağlamak ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


Northwind veritabanı kullanan Sihirbazı'nı yapılandırın. Önceki öğreticide çalışılan sonra adlı bir bağlantı dizesi zaten yüklü olmalıdır `NorthwindConnectionString` içinde `Web.config`. Şekil 3'te gösterildiği gibi bu bağlantı dizesi aşağı açılan listeden seçin.


[![Northwind veritabanı kullanmak için SqlDataSource yapılandırın](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Şekil 03**: Northwind veritabanı kullanmak için SqlDataSource yapılandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


Ardından, veri kaynağı denetiminin belirtin `SELECT` Ürünler tablosuna aşağı açılan listeden seçerek ve döndürme by deyimi `ProductName` ve `UnitPrice` sütunlar (bkz. Şekil 4). İleri'yi tıklatın ve ardından veri kaynağı Yapılandırma Sihirbazı'nı tamamlamak için Son'u tıklatın.


[![ProductName ve UnitPrice alanları ürünleri bir tablo döndürür](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Şekil 04**: dönüş `ProductName` ve `UnitPrice` alanlarını `Products` tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


Tüm olan İşte bu kadar! Sihirbazı tamamladıktan sonra Visual Studio iki BoundFields SqlDataSource denetim tarafından döndürülen iki alan yansıtmak üzere GridView ekler. GridView ve SqlDataSource denetimleri biçimlendirme izler. Şekil 5 bir tarayıcıdan görüntülendiğinde sonuçları gösterir.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![Her ürün ve onun fiyat GridView listelenir](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Şekil 05**: her ürün ve onun fiyat GridView listelenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> GridView görünümünü oluşturan temizlemek çekinmeyin. Bazı öneriler, bir para birimi olarak görüntülenen UnitPrice değeri biçimlendirme ve kılavuz görünümlerini geliştirmek için arka plan renk ve yazı tipleri kullanmayı kapsar. Görüntüleme ve ASP.NET verileri biçimlendirme daha fazla bilgi için başvurmak my [veri öğretici serisi çalışma](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>2. adım: ana sayfa çift fiyatlar düğme ekleme

Bizim sonraki görev eklemek için bir düğme Web Denetimi ana sayfa, tıklatıldığında uygulamak çift veritabanındaki tüm ürünleri fiyat olacaktır. Açık `Site.master` ana sayfa ve bir düğme altında yerleştirme tasarımcıya araç sürükleyin `RecentProductsDataSource` önceki öğreticide eklediğimiz SqlDataSource denetimi. Düğmenin ayarlamak `ID` özelliğine `DoublePrice` ve kendi `Text` "Çift ürün fiyatları" özelliğine.

Ardından, adlandırma ana sayfaya SqlDataSource denetim ekleme `DoublePricesDataSource`. Bu SqlDataSource yürütmek için kullanılan `UPDATE` fiyatların tümü çift deyimi. Özellikle, ayarlamak ihtiyacımız kendi `ConnectionString` ve `UpdateCommand` uygun bir bağlantı dizesi özellikleri ve `UPDATE` deyimi. Bu SqlDataSource denetimin çağırmak ihtiyacımız sonra `Update` yöntemi zaman `DoublePrice` düğmesine tıklandığında. Ayarlamak için `ConnectionString` ve `UpdateCommand` özelliklerini SqlDataSource denetimi seçin ve ardından Properties penceresine gidin. `ConnectionString` Özellik listelerini zaten depolanmış Bu bağlantı dizeleri `Web.config` aşağı açılan listede; seçin `NorthwindConnectionString` seçeneği Şekil 6'da gösterildiği gibi.


[![SqlDataSource NorthwindConnectionString kullanacak şekilde yapılandırma](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Şekil 06**: SqlDataSource kullanılacak yapılandırma `NorthwindConnectionString` ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


Ayarlamak için `UpdateCommand` özelliği, Özellikler penceresinde veUpdateQuery seçeneğini bulun. Seçili olduğunda, bu özellik, bir üç nokta düğmesini görüntüler; Şekil 7'de gösterilen komut ve parametre Düzenleyicisi iletişim kutusunu görüntülemek için bu düğmeye tıklayın. Aşağıdaki komutu yazın `UPDATE` iletişim kutusunun textbox INTO deyimi:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Çalıştırıldığında, bu bildirimi çift `UnitPrice` her kayıt için değer `Products` tablo.


[![SqlDataSource'nın UpdateCommand özelliğini ayarlayın](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Şekil 07**: ayarlamak SqlDataSource'nın `UpdateCommand` özelliği ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


Bu özellikleri ayarladıktan sonra düğme ve SqlDataSource denetimlerini tanımlayıcı biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Kalan tek şey çağırmak için kendi `Update` yöntemi zaman `DoublePrice` düğmesine tıklandığında. Oluşturma bir `Click` için olay işleyicisini `DoublePrice` düğmesi ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Bu işlevselliğini sınamak için ziyaret `~/Admin/Products.aspx` sayfasında 1. adımda oluşturduğunuz ve "Çift ürün fiyatları" düğmesini tıklatın. Düğmesini tıklatarak geri gönderimin neden olur ve yürütür `DoublePrice` düğmenin `Click` tüm ürünlerin fiyatları Katlama olay işleyicisi. Sayfa daha sonra yeniden oluşturulur ve biçimlendirme döndürülen ve yeniden tarayıcıda görüntülenen. "Çift ürün fiyatları önce" düğmesine tıklanana içerik sayfasındaki GridView ancak, aynı fiyatlar listeler. Başlangıçta GridView yüklenen veriler görünüm durumuna depolanmaz, yani bu üzerinde Geri göndermeler aksi belirtilmedikçe yüklenmez durumuna sahip olmasıdır. Farklı bir sayfasını ziyaret edin ve sonra geri dönüp `~/Admin/Products.aspx` sayfa güncelleştirilmiş fiyatlar göreceksiniz.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>3. adım: bir olay olduğunda fiyatlar oluşturma iki katına

Çünkü GridView `~/Admin/Products.aspx` sayfa değil hemen yansıtacak fiyat Katlama, bir kullanıcı understandably kullanıcılar "Çift ürün fiyatları" düğmesini tıklatarak değil, ya da işe yaramadı düşünebilirsiniz. Bunlar düğmesi süreleri ve fiyatlarını yeniden katlama birkaç daha deneyebilirsiniz. Bu kılavuzun içeriği sağlamak için ihtiyacımız sorunu gidermek için sayfayı görüntüleme yeni fiyatlar hemen çift sonra.

Bu öğreticide daha önce bahsedildiği gibi kullanıcı tıkladığında ana sayfa olayda yükseltmek ihtiyacımız `DoublePrice` düğmesi. Bir olay başka bir ilgi çekici bir şey oluştu diğer sınıflar (olay aboneleri için) kümesini bildirmek için bir tane sınıfı (olay yayımcısı) için yoludur. Bu örnekte, ana sayfa olay yayımcısı.; Bu içerik hakkında ne zaman verdiğiniz sayfalarının `DoublePrice` düğmesine tıklandığında aboneleri şunlardır.

Bir sınıf oluşturarak, bir olaya abone olan bir *olay işleyicisi*, gerçekleştirilen olaya yanıt olarak yürütülen bir yöntemi. Yayımcı kendisinin başlatır tanımlayarak olayları tanımlayan bir *olay temsilci*. Olay temsilci olay işleyicisi kabul etmelisiniz hangi giriş parametreleri belirtir. .NET Framework olay temsilcileri olmayan herhangi bir değer döndürür ve iki giriş parametreleri kabul eder:

- Bir `Object`, olay kaynağı tanımlar ve
- Türetilen bir sınıfı `System.EventArgs`

Bir olay işleyicisi geçen ikinci parametre olayla ilgili ek bilgiler içerebilir. While temel `EventArgs` sınıfı herhangi bir bilgi geçişi değil, .NET Framework genişleten sınıflar içerir `EventArgs` ve ek özellikleri kapsar. Örneğin, bir `CommandEventArgs` örnek yanıt olay işleyicileri iletilir `Command` olayı ve iki bilgi özellikleri içerir: `CommandArgument` ve `CommandName`.

> [!NOTE]
> Oluşturma konusunda daha fazla bilgi için bkz: oluşturma ve olayları, işleme [olaylar ve Temsilciler](https://msdn.microsoft.com/library/17sde2xt.aspx) ve [olay temsilcileri basit İngilizce](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Bir olay tanımlamak için aşağıdaki sözdizimini kullanın:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Biz yalnızca kullanıcı tıklatıldığında içerik sayfasını uyarı gerektiğinden `DoublePrice` düğmesini tıklatın ve diğer ek bilgileri geçmesi gerek yoktur, olay temsilci kullanırız `EventHandler`, kendi saniye kabul eden olay işleyicisi tanımlar parametre türünde bir nesne `System.EventArgs`. Ana sayfada olay oluşturmak için ana sayfa arka plan kodu sınıfına aşağıdaki kod satırını ekleyin:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Yukarıdaki kod ortak olay adlı ana sayfasına ekler `PricesDoubled`. Şimdi iki katına fiyatlar sonra bu olay yükseltmek ihtiyacımız. Bir olay oluşturmak için aşağıdaki sözdizimini kullanın:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Burada *gönderen* ve *eventArgs* abonenin olay işleyicisine geçirmek istediğiniz değerlerdir.

Güncelleştirme `DoublePrice` `Click` aşağıdaki kod ile olay işleyicisi:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Önceki gibi `Click` olay işleyicisini başlatır çağırarak `DoublePricesDataSource` SqlDataSource denetimin `Update` tüm ürünlerin fiyatları çift yöntemi. Aşağıdaki iki olay işleyicisi eklemeler vardır. İlk olarak, `RecentProducts` GridView'ın veri yenilenir. Bu GridView ana sayfaya önceki öğreticide eklendi ve beş en son eklenen ürünleri görüntüler. Böylece bu beş ürünler için yalnızca iki katına fiyatlar gösterir, bu kılavuz yenilemek gerekir. Aşağıdaki `PricesDoubled` olayı oluşturulur. Ana sayfa başvuru (`this`) olay işleyicisi için olay kaynağı ve boş gönderilir `EventArgs` nesne, olay bağımsız değişken olarak gönderilir.

## <a name="step-4-handling-the-event-in-the-content-page"></a>4. adım: içerik sayfasındaki olay işleme

Bu noktada ana sayfayı başlatır, `PricesDoubled` olay her `DoublePrice` düğme denetimi tıklandığında. Ancak, yalnızca yarı var olma Savaşının budur - biz yine de abone olayı işleme gerekir. Bu iki adımdan oluşur: olay işleyicisi oluşturma ve olay kablolama kodu ekleyerek olay işleyicisi olay oluşturulduğunda yürütülür.

Başlangıç adlı bir olay işleyicisi oluşturarak `Master_PricesDoubled`. Nasıl tanımladığımız nedeniyle `PricesDoubled` ana sayfa olayında olay işleyicinin iki giriş parametreleri türlerini olmalıdır `Object` ve `EventArgs`sırasıyla. Olay işleyici çağrısında `ProductsGrid` GridView'ın `DataBind` kılavuza veri rebind yöntemi.


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Olay işleyicisi için kod tamamlanır ancak biz henüz olduğunuz için ana sayfa wire `PricesDoubled` bu olay işleyicisi olaya. Abone, aşağıdaki söz dizimini aracılığıyla olay işleyicisi için bir olay bağlayan:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*Yayımcı* olay sunar nesneye bir başvurusu olan *eventName*, ve *methodName* karşılık gelen bir imzaya sahip abone tanımlanan olay işleyicisi adı *eventDelegate*. Diğer bir deyişle, olay temsilci ise `EventHandler`, ardından *methodName* bir yöntemi, bir değer döndürmüyor ve iki giriş türlerinin parametreleri kabul eden abone adı olmalıdır `Object` ve `EventArgs`, sırasıyla.

Bu olay kablolama kodu ilk sayfasını ziyaret edin ve sonraki geri göndermelere yürütülmelidir ve olayı yükleyen önündeki sayfa ömrü içindeki bir noktada olmamalıdır. Olay kablolama kodu eklemek için uygun bir çok erken sayfa çevriminin oluşur PreInit aşamasında zamandır.

Açık `~/Admin/Products.aspx` ve oluşturma bir `Page_PreInit` olay işleyicisi:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Bu kablolama kod tamamlamak için ana sayfa programlı başvuru içerik sayfasından ihtiyacımız var. Önceki öğreticide belirtildiği gibi bunu yapmanın iki yolu vardır:

- Geniş yazılmış atama tarafından `Page.Master` uygun ana sayfa türü özelliğine veya
- Ekleyerek bir `@MasterType` yönergesini `.aspx` sayfası ve kesin tür belirtilmiş kullanarak `Master` özelliği.

İkinci yaklaşımı kullanalım. Aşağıdakileri ekleyin `@MasterType` sayfanın bildirim temelli biçimlendirme üstüne yönerge:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Ardından aşağıdaki olay kablolama kodda ekleyin `Page_PreInit` olay işleyicisi:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Yerinde şu kodla içerik sayfasındaki GridView yenilenir her `DoublePrice` düğmesine tıklandığında.

Şekil 8 ve 9 bu davranışı gösterir. Şekil 8 ilk sitesini ziyaret ettiğinizde sayfası gösterilir. Değerleri, ikisi de fiyat Not `RecentProducts` GridView (sol sütunda ana sayfanın) ve `ProductsGrid` GridView (içerik sayfasındaki). Şekil 9 gösterir aynı ekran hemen sonra `DoublePrice` düğmesine tıklanana. Gördüğünüz gibi yeni fiyatlar eşzamanlı olarak iki GridViews yansıtılır.


[![Başlangıç fiyatı değerleri](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Şekil 08**: ilk fiyat değerlerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![Just-Doubled fiyatlar GridViews görüntülenir](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Şekil 09**: The Just-Doubled fiyatlar GridViews görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>Özet

İdeal olarak, bir ana sayfa ve onun içerik sayfalarını birbirinden tamamen ayrı ve etkileşim hiçbir düzeyi gerektiriyor. Bir ana sayfa veya ana sayfa veya içerik sayfasını değiştirilebilir verileri görüntüleyen içerik sayfası varsa, ancak daha sonra içerik sayfası (veya tersi bir) uyarı ana sayfa bulunması gerekebilir veri değiştirildiği böylece görüntü güncelleştirilebilir. Önceki öğreticide program aracılığıyla kendi ana sayfa ile etkileşim bir içerik sayfasını nasıl gördüğümüz; Bu öğreticide bir ana sayfa başlatma etkileşim nasıl adresindeki arama.

İçerik ve ana sayfa arasındaki programlı etkileşim içerik ya ana sayfa kaynaklanan, ancak kullanılan etkileşim düzeni üzerinde oluşturulma bağlıdır. Farkları bir içerik sayfasını tek bir ana sayfa olsa da, bir ana sayfa birçok farklı içerik sayfaları olabilir due için nedeni demektir. Bir ana sayfa bir içerik sayfasını ile doğrudan etkileşim sağlamak yerine, daha iyi bir yaklaşım bazı eylemleri gerçekleştikten göstermek için bir olayı ana sayfa sağlamaktır. Bu içerik sayfaları eylem hakkında dikkatli olay işleyicileri oluşturabilirsiniz.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Verilere erişme ve verileri ASP.NET güncelleştirme](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Olaylar ve temsilciler](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [İçerik ve ana sayfalar arasında bilgi geçirme](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [ASP.NET eğitimlerine verileri ile çalışma](../../data-access/index.md)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Suchi Banerjee oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](interacting-with-the-master-page-from-the-content-page-cs.md)
> [sonraki](master-pages-and-asp-net-ajax-cs.md)
