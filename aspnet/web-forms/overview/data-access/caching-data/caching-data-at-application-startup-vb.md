---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: "Uygulama başlangıcında (VB) verileri önbelleğe alma | Microsoft Docs"
author: rick-anderson
description: "Herhangi bir Web uygulamasında bazı verileri sık kullanılır ve bazı verileri sık kullanılır. Biz bizim ASP.NET uygulama b performansını artırabilir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b84b797bf0c9670ac65a5384b6d95d5df3827eb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-at-application-startup-vb"></a>Uygulama başlangıcında (VB) verileri önbelleğe alma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF indirin](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> Herhangi bir Web uygulamasında bazı verileri sık kullanılır ve bazı verileri sık kullanılır. Sık kullanılan veri olarak bilinen bir tekniği önceden yükleyerek biz ASP.NET uygulamamız performansını artırabilir. Bu öğretici uygulama başlangıcında önbelleğine verileri yüklemek için öngörülü yükleme için bir yaklaşım gösterir.


## <a name="introduction"></a>Giriş

Sunu ve önbelleğe alma katmanları verileri önbelleğe alma sırasında iki önceki öğreticileri arama. İçinde [ObjectDataSource ile veri önbelleğe alma](caching-data-with-the-objectdatasource-vb.md), sunu katmanı veriyi önbelleğe alma özellikleri önbelleğe alma ObjectDataSource s kullanarak Aranan. [Mimarisinde verileri önbelleğe alma](caching-data-in-the-architecture-vb.md) yeni, ayrı önbelleğe alma katmanda önbelleğe alma inceledi. Kullanılan bu öğreticileri her ikisi de *reaktif yükleme* veri önbelleği ile çalışma. Geriye dönük, verileri istenildiğinde, her zaman yüklenmesi ile sistem ilk değilse denetler, önbelleğinde s. Aksi halde, kaynak kaynak veritabanı gibi verileri alan ve önbellekte depolar. Geriye dönük yükleme için ana avantajı, uygulama kolaylığı olmasıdır. Kendi dezavantajları düzensiz performansını istekler genelinde biridir. Ürün bilgilerini görüntülemek için yukarıdaki öğretici önbelleğe alma katmandan kullanan bir sayfa düşünün. Bu sayfa ilk kez ziyaret veya bellek kısıtlamaları veya üst sınırına belirtilen süre sonu nedeniyle önbelleğe alınan veriler çıkarıldı sonra ilk kez ziyaret, verileri veritabanından gerekir. Bu nedenle, bu kullanıcıların istekleri önbelleği tarafından sunulabilen kullanıcıların istekleri uzun sürer.

*Öngörülü yükleme* bir alternatif önbellek yönetimi stratejisi, gerekli %s, önce önbelleğe alınmış verileri yükleyerek istekler genelinde bu performans düzeltir sağlar. Genellikle, öngörülü yükleme ya da düzenli olarak denetler veya temel alınan veri için bir güncelleştirme olduğunda bildirim bazı işlemi kullanır. Bu işlem, daha sonra yeni kalmasını sağlamak için önbellek güncelleştirir. Öngörülü yükleme yavaş veritabanı bağlantısı, bir Web hizmeti ya da başka bir özellikle Ağır veri kaynağını temel alınan veri geliyorsa, özellikle yararlı olur. Ancak oluşturma, yönetme ve değişiklikleri denetlemek ve önbellek güncelleştirmek için bir işlem dağıtma gerektirdiğinden bu yaklaşım öngörülü yükleme uygulamak, daha zordur.

Başka bir özellik öngörülü yükleme ve biz Bu öğreticide, keşfetme tür uygulama başlangıcında önbelleğine verileri yüklüyor. Bu yaklaşım veritabanı arama tabloları kayıtları gibi statik verileri önbelleğe alma için özellikle yararlıdır.

> [!NOTE]
> İçin daha kapsamlı bir görünüm ileriye ve geriye dönük yükleme gibi uzmanları, simgeler ve uygulama önerileri listeler arasındaki farklar adresindeki başvurmak [bir önbelleğinin içeriğini yönetme](https://msdn.microsoft.com/library/ms978503.aspx) bölümünü [ .NET Framework uygulamaları için Mimari Kılavuzu önbelleğe alma](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>1. adım: Uygulama başlangıcında önbelleğine hangi verilerin belirleme

Geriye dönük yüklenirken kullanarak önbelleğe alma örnekler biz önceki iki öğreticileri verilerle çalışma oluşturmak için düzenli aralıklarla değişebilir ve exorbitantly uzun sürmez iyi incelendi. Ancak önbelleğe alınan veriler hiçbir zaman değişirse reaktif yükleme tarafından kullanılan süre sonu gereksiz. Benzer şekilde, önbelleğe alınmasını verileri oluşturmak için bir verebilmesine uzun sürüyorsa, daha sonra önbellek boş temel alınan veriler uzun bekleme dayanabileceği gerekecek olan istekleri Bul kullanıcılarla alınır. Statik verileri ve uygulama başlangıcında oluşturmak için bir çıkmaz verileri önbelleğe alma göz önünde bulundurun.

Veritabanları birçok dinamik olmakla birlikte, en sık değişen değerler yeterince miktarda statik veri de. Örneğin, neredeyse tüm veri modelleri seçimler sabit kümesinden belirli bir değeri içeren bir veya daha fazla sütuna sahip. A `Patients` veritabanı tablosu olabilir bir `PrimaryLanguage` sütun, İngilizce, İspanyolca, Fransızca, Rusça, Japonca ve benzeri olan değerleri kümesi olabilir. Kullanarak bu tür sütunlar görmemeleri, uygulanan *arama tabloları*. İngilizce ve Fransızca dize depolamak yerine `Patients` tablo, ikinci bir tablo oluşturuldu. yaygın olarak, her bir olası değer için bir kayıtla iki sütun - benzersiz bir tanımlayıcı ve dize açıklamasını - sahip. `PrimaryLanguage` Sütununda `Patients` tablo arama tablosunda karşılık gelen benzersiz tanımlayıcı depolar. Ed Johnson s Rusça olsa Şekil 1'de hasta John Doe s birincil dili İngilizce ' dir.


![Diller tablosundaki bir arama tablosu hastalar tablosu tarafından kullanılır](caching-data-at-application-startup-vb/_static/image1.png)

**Şekil 1**: `Languages` tablo olan bir arama tablosu tarafından kullanılan `Patients` tablo


Düzenleme veya yeni bir Hasta oluşturmak için kullanılan kullanıcı arabirimi kayıtları tarafından doldurulmuş izin verilen dilleri açılan listesi içerir `Languages` tablo. Önbelleğe alma olmadan, bu arabirimin her zaman sistem ziyaret sorgulaması gerekir `Languages` tablo. Arama tablosu değerlerinin çok sık değiştiği bu hiç kayıp ve gereksiz olur.

Önbellek `Languages` önceki eğitimlerine incelenmesi aynı reaktif yükleme teknikleri kullanarak verileri. Geriye dönük yükleniyor, ancak statik arama tablosu verileri için gerek olmayan bir zamana bağlı süre sonu, kullanır. Geriye dönük yükleme kullanarak önbelleğe alma hiç önbelleğe alma daha iyi olacaktır olsa da, en iyi yaklaşımı proaktif olarak uygulama başlangıcında önbelleğine arama tablosu verileri yüklemek için olacaktır.

Bu öğreticide önbellek arama tablosu verileri ve diğer statik bilgileri nasıl ele alacağız.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>2. adım: Verileri önbelleğe almak için farklı yollar inceleniyor

Bilgi program aracılığıyla yaklaşımlar çeşitli kullanarak bir ASP.NET uygulamasındaki önbelleğe alınabilir. Biz ullanıcı zaten önceki eğitimlerine veri önbelleği kullanma görülür. Alternatif olarak, nesneleri program aracılığıyla kullanarak önbelleğe alınabilir *statik üyeler* veya *uygulama durumu*.

Üyeleri erişilmeden önce bir sınıf ile çalışırken, genellikle sınıfı ilk örneğinin oluşturulması gerekir. Örneğin, bizim iş mantığı katmanı sınıflarda birinden bir yöntemi çağırmak için biz önce sınıfın örneğini oluşturmanız gerekir:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Biz çağırabileceği önce *SomeMethod* veya birlikte çalıştığınız *SomeProperty*, biz öncelikle sınıfını kullanarak bir örneğini oluşturmanız gerekir `New` anahtar sözcüğü. *SomeMethod* ve *SomeProperty* belirli bir örneği ile ilişkilendirilmiş. Bu üyeler ömrü bunların ilişkili nesne ömrü bağlıdır. *Statik üyeler*, diğer yandan, değişkenleri, özellikleri ve arasında paylaşılan yöntemleri olan *tüm* sınıfının örnekleri ve sonuç olarak, bir sınıf olarak uzun ömürlü. Statik üyeler anahtar sözcüğüyle gösterilen `Shared`.

Statik üyeler yanı sıra veri uygulama durumu kullanarak önbelleğe alınabilir. Her ASP.NET uygulaması bir ad/değer koleksiyonu tüm kullanıcılar ve uygulamanın sayfalar arasında paylaşılan bu s tutar. Bu koleksiyon kullanılarak erişilebilir [ `HttpContext` sınıfı](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` özelliği](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)ve bir ASP.NET sayfası s arka plan kodu sınıfın kullanılan sözlüğüdür:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

Veri önbelleği mekanizmaları zaman ve bağımlılık tabanlı expiries, önbellek öğesi öncelikleri ve benzeri için sağlama verileri önbelleğe alma işlemi için bir çok daha zengin bir API sağlar. Statik üyeler ve uygulama durumu ile gibi özellikler sayfasında geliştirici tarafından el ile eklenmesi gerekir. Ancak, uygulama yaşam süresi için uygulama başlangıcında verileri önbelleğe alma, veri önbelleği s avantajları moot. Bu öğreticide statik verileri önbelleğe alma için tüm üç teknikleri kullanan kodu inceleyeceğiz.

## <a name="step-3-caching-thesupplierstable-data"></a>3. adım: Önbelleğe alma`Suppliers`tablo verileri

Northwind veritabanı tabloları biz tarih olarak uygulanır ve tüm geleneksel arama tabloları içermez. Dört DataTables Bizim DAL statik olmayan değerleri olan tüm modeli tabloları uygulanmadı. Bu öğretici s yalnızca denetlemesine izin vermek için DAL ve ardından yeni bir sınıf ve BLL yöntemleri için yeni bir DataTable eklemek için zaman harcama yerine, görünen `Suppliers` tablo s verileri statik. Bu nedenle, size bu verileri uygulama başlangıcında önbellekte.

Başlatmak için adlı yeni bir sınıf oluşturun `StaticCache.cs` içinde `CL` klasör.


![CL klasöründe StaticCache.vb sınıfı oluşturun](caching-data-at-application-startup-vb/_static/image2.png)

**Şekil 2**: oluşturmak `StaticCache.vb` sınıfını `CL` klasörü


Biz bu önbellekten veri döndüren yöntemler yanı sıra, başlangıçta verileri uygun önbellek deposuna yükler yöntemi eklemeniz gerekir.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Yukarıdaki kod, bir statik üye değişkeni kullanır `suppliers`, gelen sonuçları tutmak için `SuppliersBLL` s sınıfı `GetSuppliers()` çağrılır yöntemi `LoadStaticCache()` yöntemi. `LoadStaticCache()` Yöntemi uygulama s başlatma sırasında çağrılacak yöneliktir. Bu veriler uygulama başlangıcında yüklenen sonra tedarikçi verilerle çalışmak için gereken herhangi bir sayfayı çağırabilirsiniz `StaticCache` s sınıfı `GetSuppliers()` yöntemi. Bu nedenle, tedarikçileri alma çağrısı veritabanına yalnızca bir kez uygulama başlangıcında gerçekleştirilir.

Statik üye değişkeni önbellek deposu olarak kullanmak yerine, biz alternatif olarak uygulama durumu veya veri önbelleği kullanmış. Aşağıdaki kod, uygulama durumunu kullanmak için retooled sınıfı gösterir:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

İçinde `LoadStaticCache()`, üretici bilgilerini uygulama değişkenine depolanan *anahtar*. Bu s uygun bir tür döndürdü (`Northwind.SuppliersDataTable`) gelen `GetSuppliers()`. Uygulama durumu kullanarak ASP.NET sayfaları arka plan kodu sınıflarda erişilebilir sırada `Application("key")`, biz kullanmalıdır mimarisinde `HttpContext.Current.Application("key")` geçerli alabilmek için `HttpContext`.

Benzer şekilde, veri önbelleği aşağıdaki kodu gösterildiği gibi bir önbellek deposu olarak kullanılabilir:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Veri önbelleği hiçbir zaman tabanlı süre sonu ile bir öğe eklemek için kullanın `System.Web.Caching.Cache.NoAbsoluteExpiration` ve `System.Web.Caching.Cache.NoSlidingExpiration` giriş parametresi olarak değerleri. Bu belirli veri önbelleği s yüklemesini `Insert` yöntemi, böylece biz belirtebilirsiniz seçilmedi *öncelik* önbellek öğesinin. Öncelik, kullanılabilir bellek azaldığında önbellekten atmak öğeleri belirlemek için kullanılır. Öncelik burada kullandığımız `NotRemovable`, işlemi bu önbellek öğesi t won sağlar.

> [!NOTE]
> Bu öğretici s indirme uygulayan `StaticCache` statik üye değişkeni yaklaşımı kullanarak sınıfı. Uygulama durumu ve veri önbelleği teknikleri kodunu sınıfı dosyasındaki açıklamalarda kullanılabilir.


## <a name="step-4-executing-code-at-application-startup"></a>4. adım: Uygulama başlangıcında kodu çalıştırma

Bir web uygulaması ilk başladığında kod yürütmek için adlı özel bir dosya oluşturmak ihtiyacımız `Global.asax`. Bu dosya için uygulama-oturum-olay işleyicileri içerebilir ve istek düzeyi olayları ve hazır uygulama başladığında, yürütülecek kod burada ekleyebiliriz.

Ekle `Global.asax` dosyasını Visual Studio s Solution Explorer Web sitesi proje adına sağ tıklayarak ve Yeni Öğe Ekle seçerek web uygulaması s kök dizine. Yeni Öğe Ekle iletişim kutusu, genel uygulama sınıfı öğesi türünü seçin ve ardından Ekle düğmesini tıklatın.

> [!NOTE]
> Zaten varsa bir `Global.asax` öğesine yeni öğe Ekle iletişim kutusunda listelenmez genel uygulama sınıfı projenizi dosyasında.


[![Web uygulaması s kök dizinine Global.asax dosyası ekleme](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Şekil 3**: eklemek `Global.asax` Web uygulamanızı s kök dizini dosyasına ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-at-application-startup-vb/_static/image5.png))


Varsayılan `Global.asax` dosya şablonu içeren sunucu tarafı içinde beş yöntemlerini `<script>` etiketi:

- **`Application_Start`**web uygulaması ilk başladığında yürütür
- **`Application_End`**uygulamanın kapanacağını çalışır
- **`Application_Error`**İşlenmeyen bir özel durum uygulama eriştiğinde yürütür
- **`Session_Start`**Yeni bir oturum oluşturulduğunda yürütür
- **`Session_End`**bir oturum süresi doldu veya terk çalışır

`Application_Start` Olay işleyicisi s uygulaması yaşam döngüsü boyunca yalnızca bir kez çağrılır. Uygulama içeriğini değiştirerek gerçekleşebilir bir ASP.NET kaynağı uygulamadan istenir ve uygulamayı yeniden başlatılana kadar çalışmaya devam eder ilk kez başlar `/Bin` klasörü değiştirme `Global.asax`, değiştirme içinde içeriği `App_Code` klasör veya değiştirme `Web.config` diğer nedenleri arasında dosya. Başvurmak [ASP.NET uygulama yaşam döngüsü genel bakış](https://msdn.microsoft.com/library/ms178473.aspx) uygulama yaşam döngüsü hakkında ayrıntılı bilgiler için.

Bu öğreticileri için yalnızca kodu eklemek ihtiyacımız `Application_Start` yöntemi, bunu kullanımında diğer kaldırmak boş. İçinde `Application_Start`, yalnızca çağrısı `StaticCache` s sınıfı `LoadStaticCache()` yük ve üretici bilgilerini önbelleğe yöntemi:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Bu s tüm var olan üzere! Uygulama başlangıcında `LoadStaticCache()` yöntemi BLL tedarikçi bilgileri alın ve bir statik üye değişkeni depolamak (veya ne olursa olsun önbellek depolamak sona erdi kullanarak `StaticCache` sınıfı). Bu davranış doğrulamak için bir kesme noktası belirleyerek `Application_Start` yöntemi ve uygulamanızı çalıştırın. Uygulama başlatma sırasında kesme noktası isabet unutmayın. İstekler, ancak neden olmaz `Application_Start` yürütülecek yöntem.


[![Bir kesme noktası Application_Start olay işleyici bırakılıyor yürütülen olduğunu doğrulayın kullanın](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Şekil 4**: bir kesme noktası doğrulayın kullanın, `Application_Start` olay işleyicisi olduğu anda yürütülen ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Değil isabet durumunda `Application_Start` hata ayıklama ilk kez başlattığınızda kesme noktası, onu olduğundan, uygulamanızın zaten başlatıldı. Değiştirerek yeniden uygulamaya zorlamak, `Global.asax` veya `Web.config` dosyalarını ve yeniden deneyin. Yalnızca ekleyebileceğiniz (kaldırmak hızlı bir şekilde uygulamayı yeniden başlatmak için bu dosyalardan biri sonunda boş bir satır veya).


## <a name="step-5-displaying-the-cached-data"></a>5. adım: önbelleğe alınmış verileri görüntüleme

Bu noktada `StaticCache` sınıfı üzerinden erişilen uygulama başlangıcında önbelleğe tedarikçi veri sürümünün yüklü kendi `GetSuppliers()` yöntemi. Sunu katmanı bu verilerle çalışmak için size bir ObjectDataSource kullanın veya program aracılığıyla çağırma `StaticCache` s sınıfı `GetSuppliers()` bir ASP.NET sayfası s arka plandaki kod sınıfı yönteminden. Önbelleğe alınan üretici bilgilerini görüntülemek için ObjectDataSource ve GridView denetimlerini kullanarak ara s olanak tanır.

Başlangıç açarak `AtApplicationStartup.aspx` sayfasındaki `Caching` klasör. Araç kutusu ayarı tasarımcıya GridView sürükleyin kendi `ID` özelliğine `Suppliers`. Ardından, s akıllı etiket GridView seçin adlı yeni bir ObjectDataSource oluşturmak `SuppliersCachedDataSource`. ObjectDataSource kullanmak için yapılandırma `StaticCache` s sınıfı `GetSuppliers()` yöntemi.


[![ObjectDataSource StaticCache sınıfını kullanmak için yapılandırma](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Şekil 5**: ObjectDataSource kullanmak için yapılandırma `StaticCache` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-at-application-startup-vb/_static/image11.png))


[![Önbelleğe alınan tedarikçi veri almak için GetSuppliers() yöntemini kullanın](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Şekil 6**: kullanım `GetSuppliers()` önbelleğe Tedarikçi verileri almak üzere yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-at-application-startup-vb/_static/image14.png))


Sihirbazı tamamladıktan sonra Visual Studio otomatik olarak BoundFields her veri alanlarını ekleyecektir `SuppliersDataTable`. GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Şekil 7 bir tarayıcıdan görüntülendiğinde sayfa gösterir. Çıktı biz çekilen BLL s verilerden aynıdır `SuppliersBLL` sınıfı, ancak kullanarak `StaticCache` sınıfı Tedarikçi verileri uygulama başlangıcında önbelleğe alınmış olarak döndürür. Kesme noktaları ayarlayabilirsiniz `StaticCache` s sınıfı `GetSuppliers()` Bu davranış doğrulamak için yöntem.


[![Önbelleğe alınmış tedarikçi veriler bir GridView görüntülenir](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Şekil 7**: Tedarikçi verileri önbelleğe alınmış bir GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Özet

Çoğu her veri modeli eşit miktarda genellikle arama tabloları formunda uygulanan statik veri içeriyor. Bu bilgiler statik olduğundan orada s Bu bilgiye gerek duyuyor görüntülenecek sürekli olarak her zaman veritabanına erişmek için bir sebep. Verileri önbelleğe alma, ayrıca, statik doğası, nedeniyle orada s geçerlilik süresi gerek yoktur. Bu öğreticide bu tür veriler alıp veri önbelleğindeki uygulama durumu ve statik üye değişkeni aracılığıyla önbelleğe nasıl gördük. Bu bilgiler, uygulama başlangıcında önbelleğe alınır ve uygulama s kullanım ömrü boyunca önbellekte kalır.

Bu öğretici ve son iki biz ve arama uygulama s ömrü boyunca verileri önbelleğe alma yanı sıra, zamana bağlı expiries kullanarak. Ancak, veritabanı verileri önbelleğe alma, zamana bağlı süre sonu değerinden ideal olabilir. Düzenli aralıklarla önbelleği temizleme yerine, temel alınan veritabanı veri değiştirildiğinde önbelleğe alınan öğe yalnızca çıkarmak için en iyi olacaktır. Bu ideal biz bizim sonraki öğreticide inceleyeceğiz SQL önbellek bağımlılıkları kullanımı ile mümkündür.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Teresa Murphy ve Zack CAN yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](caching-data-in-the-architecture-vb.md)
[sonraki](using-sql-cache-dependencies-vb.md)
