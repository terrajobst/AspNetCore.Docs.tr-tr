---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: (C#) uygulama başlangıcında verileri önbelleğe alma | Microsoft Docs
author: rick-anderson
description: Herhangi bir Web uygulamasına bazı verileri sık kullanılır ve bazı verileri seyrek kullanılır. Biz bizim ASP.NET uygulama b performansı...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: 01f80353e3f871606d022b01c98b0d36d15d2379
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370484"
---
<a name="caching-data-at-application-startup-c"></a>(C#) uygulama başlangıcında verileri önbelleğe alma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF'yi indirin](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> Herhangi bir Web uygulamasına bazı verileri sık kullanılır ve bazı verileri seyrek kullanılır. Önceden bir teknik olarak bilinen sık kullanılan verileri yükleyerek biz bizim ASP.NET uygulama performansını artırabilir. Bu öğreticide, uygulama başlangıcında önbelleğe verileri yüklemek için proaktif yükleme için bir yaklaşım gösterilmektedir.


## <a name="introduction"></a>Giriş

Sunu ve önbellek katmanları verileri önbelleğe alma iki önceki öğreticilerdeki arıyordu. İçinde [ObjectDataSource ile verileri önbelleğe alma](caching-data-with-the-objectdatasource-cs.md), sunu katmanı, verileri önbelleğe almak için özellikleri önbelleğe alma ObjectDataSource s kullanarak incelemiştik. [Mimaride verileri önbelleğe alma](caching-data-in-the-architecture-cs.md) yeni, ayrı önbelleğe alma katmanda önbelleğe alma incelenir. Bu öğreticileri kullanılan her ikisi de *reaktif yükleme* veri önbelleği ile çalışma. Reaktif, her zaman veri istenildiğinde, yüklenmekte olan sistem ilk olmadığını denetler, önbellekte s. Değilse, veritabanı, kaynak kaynak verileri alan ve ardından önbellekte depolar. Reaktif yükleme başlıca avantajı, uygulama Kolaylığı ' dir. Kendi dezavantajları düzensiz performansını istekler genelinde biridir. Ürün bilgilerini görüntülemek için önceki öğreticide önbelleğe alma katmandan kullanan bir sayfa düşünün. Bu sayfa ilk kez ziyaret veya bellek kısıtlamaları veya üst sınırına belirtilen süre sonu nedeniyle önbelleğe alınan verilerin çıkarıldı sonra ilk kez ziyaret, verileri veritabanından alınmalıdır. Bu nedenle, bu kullanıcıların istekleri önbelleği tarafından sunulabilen kullanıcıların istekleri daha uzun sürer.

*Proaktif yükleme* bir alternatif önbellek yönetim stratejisini gereken, önce önbelleğe alınmış verileri yükleyerek bu performans düzeltir istekler genelinde sağlar. Genellikle, proaktif yükleme düzenli aralıklarla denetleyen veya temel alınan bir güncelleştirme olduğunda bildirim bazı işlemi kullanır. Bu işlem, sonra güncel kalması için önbelleği güncelleştirir. Proaktif yükleniyor, temel alınan verileri yavaş veritabanı bağlantısı, bir Web hizmeti ya da başka bir özellikle Ağır veri kaynağından geliyorsa, özellikle yararlıdır. Ancak, oluşturma, yönetme ve dağıtma değişiklikleri denetlemek ve önbelleği güncelleştirmek için bir işlem gerektirdiği proaktif yüklenmesi için bu yaklaşımı uygulamak daha zordur.

Proaktif yükleniyor ve biz Bu öğreticide, araştırma türü başka bir özellik olarak uygulama başlangıcında önbelleğe veri yükleniyor. Bu yaklaşım, veritabanı arama tablolarındaki kayıtlara gibi statik verileri önbelleğe almak için özellikle yararlıdır.

> [!NOTE]
> Başvurmak için daha ayrıntılı bir görünüm olumlu ve olumsuz uygulama önerileri listesi yanı sıra, proaktif ve reaktif yükleme arasındaki farklar, [bir önbelleğinin içeriğini yönetme](https://msdn.microsoft.com/library/ms978503.aspx) bölümünü [ .NET Framework uygulamalarına yönelik Mimari Kılavuzu önbelleğe alma](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>1. adım: Uygulama başlangıcında hangi verilerin belirlenmesi

Reaktif yükleniyor kullanarak önbelleğe alma örnekler biz önceki iki öğreticiler verilerle çalışma oluşturmak için düzenli aralıklarla değişebilir ve exorbitantly uzun sürmez iyi incelenir. Ancak hiçbir zaman önbelleğe alınmış verileri değişirse, reaktif yükleme tarafından kullanılan süre sonu gereksiz. Benzer şekilde, önbelleğe alınmasını veri oluşturmak için bir ilgil zorlukları uzun sürerse, daha sonra önbellek boş uzun bekleme sırasında temel alınan verileri alabilecek gerekecek olan isteklerini bulmak kullanıcılarla alınır. Statik veri ve uygulama başlangıcında oluşturmak için bir çıkmaz verileri önbelleğe almayı düşünün.

Veritabanları çok sayıda dinamik olsa da, en sık değişen değerler de ciddi miktarda bir statik veri var. Örneğin, veri modellerini neredeyse tüm seçenekleri kümesinden belirli bir değeri içeren bir veya daha fazla sütun var. A `Patients` veritabanından olabilir bir `PrimaryLanguage` sütun, İngilizce, İspanyolca, Fransızca, Rusça, Japonca ve benzeri olan değerleri kümesi olabilir. Bu sütun türlerinden kullanma görmemeleri, uygulanır *arama tabloları*. İngilizce ve Fransızca dize depolamak yerine `Patients` tablo, ikinci bir tablo oluşturulur, yaygın olarak, bir kaydı olası her değerin iki - benzersiz bir tanımlayıcı ve bir dize açıklamasını - sütuna sahip. `PrimaryLanguage` Sütununda `Patients` tablo arama tablosunda karşılık gelen benzersiz tanımlayıcısı depolar. Ed Johnson s Rusça ederken Şekil 1'de, hasta John Doe s birincil dili, İngilizce olarak belirlenmiştir.


![Bir arama tablosu tarafından kullanılan Hastalara tablo dilleri tablodur](caching-data-at-application-startup-cs/_static/image1.png)

**Şekil 1**: `Languages` tablodur bir arama tablosu kullanılan `Patients` tablo


Düzenleme veya yeni bir Hasta oluşturmak için kullanılan kullanıcı arabirimi verilen dillerin kayıtları tarafından doldurulan aşağı açılan listesi verilebilir `Languages` tablo. Önbelleğe alma olmadan her zaman bu arabirimin sistem ziyaret faydalanacaksa `Languages` tablo. Arama tablosu değerlerinin çok sık değiştiği bu hiç olmadığı kadar kısıp ve gereksiz olur.

Önbelleğe alınamadı `Languages` önceki öğreticilerdeki incelenirken aynı reaktif yükleme teknikleri kullanarak verileri. Reaktif yükleniyor, ancak statik arama için tablo verileri gerek olmayan bir zamana bağlı süre sonu, kullanır. Reaktif yükleme kullanarak önbelleğe alma hiç önbelleksizlik daha iyi olacaktır ancak proaktif bir şekilde uygulama başlangıcında önbelleğe arama tablo verileri yüklemek için en iyi yaklaşım olacaktır.

Bu öğreticide arama tablo verileri önbelleğe almak ve diğer statik bilgi atacağız.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>2. adım: Verileri önbelleğe almak için farklı şekilde İnceleme

Bilgi yönelik çeşitli yaklaşımları kullanarak bir ASP.NET uygulamasında programlı bir şekilde önbelleğe alınabilir. Biz ve önceki öğreticilerdeki veri önbelleğini kullanma zaten görüldü. Alternatif olarak, nesneleri program aracılığıyla kullanarak önbelleğe alınabilir *statik üyeleri* veya *uygulama durumu*.

Üyeleri erişilmeden önce bir sınıf ile çalışırken, genellikle sınıfı ilk örneği gerekir. Örneğin, bizim iş mantığı katmanı sınıflarda birinden bir yöntem çağırmak için size ilk sınıfının bir örneğini oluşturmanız gerekir:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Biz de çağırabilirsiniz önce *SomeMethod* veya çalışmak *SomeProperty*, biz öncelikle sınıfını kullanarak bir örneğini oluşturmanız gerekir `new` anahtar sözcüğü. *SomeMethod* ve *SomeProperty* belirli bir örneği ile ilişkili. Bu üyeleri ömrünü kendi ilişkili nesne ömrünü bağlıdır. *Statik üyeleri*, diğer taraftan, değişkenleri, özellikleri ve yöntemleri arasında paylaşılan olan *tüm* sınıfının örneklerini ve sonuç olarak, bir sınıf olarak uzun ömürlü. Statik üyeleri anahtar sözcüğü tarafından gösterilen `static`.

Statik üyeleri ek olarak veri kullanan uygulama durumu önbelleğe alınabilir. Her bir ASP.NET uygulaması tüm kullanıcılar ve uygulamanın sayfalar arasında paylaşılan, s ad/değer koleksiyonunu tutar. Bu koleksiyonu kullanılarak erişilebilir [ `HttpContext` sınıfı](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` özelliği](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx), ASP.NET sayfalarının arka plan kod sınıftan kullanılan ve şu şekilde:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

Veri önbelleğini süresi ve bağımlılık tabanlı expiries, önbellek öğesi öncelikleri ve diğerleri için mekanizmalar sağlayan verileri önbelleğe almak için bir çok daha zengin bir API sağlar. Statik üyeleri ve uygulama durumu ile gibi özellikler sayfasında geliştirici tarafından el ile eklenmesi gerekir. Ancak, uygulama ömrü boyunca uygulama başlangıcında verileri önbelleğe alma, veri önbelleği s avantajları moot uygulanır. Bu öğreticide, statik verileri önbelleğe almak için tüm üç teknikler kullanan kodu inceleyeceğiz.

## <a name="step-3-caching-thesupplierstable-data"></a>3. adım: Önbelleğe alma`Suppliers`tablo verileri

Northwind veritabanı tabloları şu tarihe uygulanan ve tüm geleneksel arama tabloları içermez. Dört DataTable statik olmayan değerleri olan tüm modeli tabloları bizim DAL uygulanır. Bu öğreticide yalnızca s izin vermek için yeni bir veri Tablolsu DAL ve ardından yeni bir sınıf ve BLL yöntemleri eklemek için zaman harcadığı yerine, görünen `Suppliers` statik tabloyu s. Bu nedenle, size bu uygulama başlangıcında verileri önbelleğe alınamadı.

Başlamak için adlı yeni bir sınıf oluşturma `StaticCache.cs` içinde `CL` klasör.


![CL klasörde StaticCache.cs sınıfı oluşturun](caching-data-at-application-startup-cs/_static/image2.png)

**Şekil 2**: oluşturma `StaticCache.cs` sınıfını `CL` klasörü


Size uygun önbellek depoya başlangıcında verileri yükler bir yöntem yanı sıra bu önbellekten veri döndüren yöntemler eklemeniz gerekir.


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

Yukarıdaki kod, bir statik üye değişkeninin kullanır `suppliers`, sonuçlardan tutacak `SuppliersBLL` s sınıfı `GetSuppliers()` çağrılır yöntemi `LoadStaticCache()` yöntemi. `LoadStaticCache()` Yöntemi uygulama s başlatma sırasında çağrılacak yöneliktir. Bu veriler uygulama başlangıcında veriler yüklendikten sonra tedarikçi verilerle çalışmak için gereken herhangi bir sayfa çağırabilirsiniz `StaticCache` s sınıfı `GetSuppliers()` yöntemi. Bu nedenle, tedarikçileri alma çağrısı veritabanına yalnızca bir kez, uygulama başlangıcında gerçekleşir.

Statik üye değişkeni önbellek deposu olarak kullanmak yerine alternatif olarak uygulama durumu veya veri önbelleğini kullandık. Aşağıdaki kod, uygulama durumunu kullanmak için retooled sınıfı göstermektedir:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

İçinde `LoadStaticCache()`, sağlayıcı bilgileri uygulama değişkenine depolanan *anahtar*. Bunu s uygun türü döndürdü (`Northwind.SuppliersDataTable`) öğesinden `GetSuppliers()`. Uygulama durumu ASP.NET sayfaları kullanarak arka plan kod sınıflarda erişilebilir durumdayken `Application["key"]`, kullanmamız gerekir mimarisinde `HttpContext.Current.Application["key"]` geçerli ürününü `HttpContext`.

Benzer şekilde, veri önbelleği olarak aşağıdaki kodun gösterdiği bir önbellek deposu olarak kullanılabilir:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Zamana bağlı bitiş tarihi ile verileri önbelleğe bir öğe eklemek için `System.Web.Caching.Cache.NoAbsoluteExpiration` ve `System.Web.Caching.Cache.NoSlidingExpiration` giriş parametre değerleri. Bu belirli veri önbelleğini s yüklemesini `Insert` yöntemi, böylece belirtmemiz seçilmiştir *öncelik* önbellek öğesinin. Öncelik kullanılabilir bellek düşük çalıştığında önbellekten atmak öğeleri belirlemek için kullanılır. Öncelik burada kullandığımız `NotRemovable`, işlemi bu önbellek öğesinin t kazanılan da sağlar.

> [!NOTE]
> Bu öğretici s indirme uygulayan `StaticCache` statik üye değişkeni yaklaşımı kullanarak sınıfı. Uygulama durumunu ve verileri önbellek teknikleri için kod açıklamaları sınıf dosyası kullanıma sunulmuştur.


## <a name="step-4-executing-code-at-application-startup"></a>4. adım: Uygulama başlangıcında kod yürütülüyor

Bir web uygulaması ilk kez başlatıldığında, kod yürütmek için adlı özel bir dosya oluşturmak ihtiyacımız `Global.asax`. Bu dosya için uygulama-oturum-olay işleyicileri içerebilir ve istek düzeyi olayları ve bu, burada uygulama başladığında, yürütülecek kod burada ekleyebiliriz.

Ekle `Global.asax` dosyasını Visual Studio s Çözüm Gezgini içinde Web sitesi proje adının üzerine sağ tıklayın ve Yeni Öğe Ekle seçerek web uygulaması s kök dizini. Yeni Öğe Ekle iletişim kutusu, genel uygulama sınıfı öğesi türünü seçin ve ardından Ekle düğmesine tıklayın.

> [!NOTE]
> Zaten bir `Global.asax` dosya projenizde, genel uygulama sınıfı öğesi türü değil yeni öğe Ekle iletişim kutusunda listelenir.


[![Web uygulaması s kök dizinine Global.asax dosyası Ekle](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Şekil 3**: ekleme `Global.asax` Web uygulamanız s kök dizininde bir dosyaya ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-at-application-startup-cs/_static/image5.png))


Varsayılan `Global.asax` dosya şablonu içinde sunucu tarafı beş yöntemler içerir `<script>` etiketi:

- **`Application_Start`** web uygulamasını ilk kez başlatıldığında yürütür
- **`Application_End`** Uygulama kapatılıyor çalışır
- **`Application_Error`** İşlenmeyen bir özel durum uygulama eriştiğinde yürütür
- **`Session_Start`** Yeni bir oturum oluşturulduğunda yürütür
- **`Session_End`** oturum süresi doldu veya durdurulmuş çalıştırmalar

`Application_Start` Olay işleyicisi bir s uygulaması yaşam döngüsü boyunca yalnızca bir kez çağrılır. Uygulama içeriğini değiştirerek gerçekleşebilir ASP.NET kaynak uygulamadan istenen ve uygulamayı yeniden başlatılana kadar çalışmaya devam eder ilk kez başlatılmadan `/Bin` klasöründe değiştirme `Global.asax`, değiştirme içindeki içeriği `App_Code` klasör veya değiştirme `Web.config` yol açabilecek diğer nedenler arasında bir dosya. Başvurmak [ASP.NET uygulama yaşam döngüsü'ne genel bakış](https://msdn.microsoft.com/library/ms178473.aspx) uygulama yaşam döngüsü üzerinde daha ayrıntılı bir açıklaması için.

Bu öğreticiler için yalnızca kod eklemek ihtiyacımız `Application_Start` yöntemi, bunu kullanımında diğerleri kaldırmak boş. İçinde `Application_Start`, yalnızca çağrı `StaticCache` s sınıfı `LoadStaticCache()` yükleyecek ve Tedarikçi bilgilerini önbelleğe yöntemi:


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

Tüm var. Bu s için İşte bu kadar! Uygulama başlangıcında `LoadStaticCache()` yöntemi BLL sağlayıcı bilgileri alın ve bir statik üye değişkeni depolar (veya hangi önbelleği depolamak sona erdi, kullanılarak `StaticCache` sınıfı). Bu davranış doğrulamak için bir kesme noktası ayarlayın `Application_Start` yöntemi ve uygulamanızı çalıştırın. Uygulama başlatma sırasında kesme noktasına erişildiğinde unutmayın. Sonraki istekler, ancak neden olmaz `Application_Start` yürütmek için yöntemi.


[![Bir kesme noktasına uygulama_başlatma olay işleyicisi olan yürütülen olduğundan emin olun kullanın](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Şekil 4**: bir kesme noktası doğrulama için kullanmak, `Application_Start` olay işleyicisidir olan yürütülen ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-at-application-startup-cs/_static/image8.png))


> [!NOTE]
> Değil isabet durumunda `Application_Start` kesme noktası, hata ayıklama ilk kez başlattığınızda olduğu uygulamanız zaten başlamış olduğundan. Değiştirerek yeniden uygulamaya zorlamak, `Global.asax` veya `Web.config` dosyalarını ve yeniden deneyin. Yalnızca ekleyebileceğiniz (uygulamayı hızla yeniden başlatmak bu dosyalardan biri, sonunda boş bir satır veya kaldırabileceğiniz).


## <a name="step-5-displaying-the-cached-data"></a>5. adım: önbelleğe alınan verileri görüntüleme

Bu noktada `StaticCache` sınıfı aracılığıyla erişilen uygulama başlangıcında önbelleğe tedarikçi veri sürümü var. onun `GetSuppliers()` yöntemi. Sunu katmanı bu verilerle çalışmak için size bir ObjectDataSource kullanma veya program aracılığıyla çağırma `StaticCache` s sınıfı `GetSuppliers()` bir ASP.NET sayfasında s arka plan kod sınıfı yöntemi. ObjectDataSource ile GridView denetimleri kullanarak önbelleğe alınmış üretici bilgilerini görüntülemek için konum s olanak tanır.

Başlangıç açarak `AtApplicationStartup.aspx` sayfasını `Caching` klasör. GridView tasarımcıya ayarlanması için araç kutusundan sürükleyin, `ID` özelliğini `Suppliers`. GridView ' s akıllı etiket seçin sonra adlı yeni bir ObjectDataSource oluşturmak `SuppliersCachedDataSource`. ObjectDataSource kullanmak için yapılandırma `StaticCache` s sınıfı `GetSuppliers()` yöntemi.


[![ObjectDataSource StaticCache sınıfını kullanmak için yapılandırma](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Şekil 5**: kullanılacak ObjectDataSource yapılandırma `StaticCache` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-at-application-startup-cs/_static/image11.png))


[![Önbelleğe alınan üretici veri almak için GetSuppliers() yöntemi kullanın](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Şekil 6**: kullanım `GetSuppliers()` yönteminin önbellekte tutulan sağlayıcı veri almak için ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-at-application-startup-cs/_static/image14.png))


Sihirbazı tamamladıktan sonra Visual Studio otomatik olarak BoundFields her veri alanı için ekler `SuppliersDataTable`. GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

Şekil 7, sayfada bir tarayıcıdan görüntülendiğinde gösterilir. Çıkış ki çekilen BLL s verilerden aynıdır `SuppliersBLL` sınıf ancak kullanarak `StaticCache` sınıfı Tedarikçi verileri uygulama başlangıcında önbelleğe alınmış olarak döndürür. Kesme noktalarını ayarlayabilir `StaticCache` s sınıfı `GetSuppliers()` bu davranışı doğrulamak için yöntem.


[![Önbelleğe alınmış tedarikçi veriler içinde GridView görüntülenir](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Şekil 7**: Tedarikçi verileri önbelleğe içinde GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-at-application-startup-cs/_static/image17.png))


## <a name="summary"></a>Özet

Çoğu her veri modeli ciddi miktarda bir genellikle arama tabloları biçiminde uygulanan statik veriler içerir. Bu bilgiler statik olduğundan burada s Bu bilgiler gerekli görüntülenecek sürekli olarak her zaman veritabanına erişmek için neden. Verileri önbelleğe alırken Ayrıca, statik doğası, nedeniyle burada s bir sona erme gerek yoktur. Bu öğreticide bu tür veri alıp veri önbelleğindeki uygulama durumu ve bir statik üye değişkeni aracılığıyla önbelleğe nasıl gördük. Bu bilgiler, uygulama başlangıcında önbelleğe alınır ve uygulama s kullanım ömrü boyunca önbellekte kalır.

Bu öğreticide ve son iki ediyoruz ve uygulama s yaşam süresi boyunca verileri önbelleğe alma yanı sıra, zamana bağlı expiries kullanarak görünüyordu. Ancak, veritabanı verileri önbelleğe alma, zamana bağlı süre sonu küçüktür ideal olabilir. Önbellek Temizleme yerine düzenli aralıklarla, temel alınan veritabanı veri değiştirildiğinde önbelleğe alınan öğeyi yalnızca çıkarmak için en uygun olacaktır. Bu bizim sonraki öğreticide inceleyeceğiz SQL önbellek bağımlılıklarını sağlanamaz idealdir.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Teresa Murphy ve Zack Jones yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](caching-data-in-the-architecture-cs.md)
> [İleri](using-sql-cache-dependencies-cs.md)
