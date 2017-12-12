---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: "ObjectDataSource (C#) ile verileri önbelleğe alma | Microsoft Docs"
author: rick-anderson
description: "Önbelleğe alma yavaş ve hızlı bir Web uygulaması arasındaki farkı anlamına gelebilir. Bu öğretici ilk ASP.NET önbelleğe alma ayrıntılı bir göz atalım dört ediyor..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ce0bd1d3302ee68c9c65584686172a07143e4a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-with-the-objectdatasource-c"></a>ObjectDataSource (C#) ile verileri önbelleğe alma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) veya [PDF indirin](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Önbelleğe alma yavaş ve hızlı bir Web uygulaması arasındaki farkı anlamına gelebilir. ASP.NET önbelleğe alma ayrıntılı bir göz atalım dört ilk öğreticidir. Önbelleğe alma temel kavramları ve sunu katmanı ObjectDataSource Denetimi aracılığıyla önbelleğe alma uygulamak nasıl öğrenin.


## <a name="introduction"></a>Giriş

De bilgisayar bilimi *önbelleğe alma* alma veri ya da edinilir pahalıdır bilgisi ve erişim daha hızlı bir şekilde bir konumda bir kopyasını saklamak işlemidir. Veri tabanlı uygulamalar için büyük ve karmaşık sorgular s uygulaması yürütme süresi çoğunluğu yaygın olarak kullanabilir. Bu tür bir uygulama s performansı, daha sonra genellikle uygulama s bellekte pahalı veritabanı sorguların sonuçlarını depolayarak geliştirilebilir.

ASP.NET 2.0 çeşitli seçenekler önbelleğe alma sunar. Bir web sayfası ya da kullanıcı denetimi çizilir s biçimlendirme aracılığıyla önbelleğe alınabilir *çıktı önbelleği*. ObjectDataSource ve SqlDataSource denetimleri denetim düzeyinde önbelleğe alınacak önbelleğe alma özelliklerini de, böylelikle veri olanak sağlar. Ve ASP.NET s *veri önbelleği* program aracılığıyla nesneleri önbelleğe almak için sayfa geliştiricileri sağlayan zengin bir önbelleğe alma API sağlar. Bu öğretici ve biz ObjectDataSource s kullanarak inceleyeceğiz sonraki üç veri önbelleği yanı sıra özellikleri önbelleğe alma. Biz de başlangıçta uygulama genelinde verileri önbelleğe almak nasıl ve önbelleğe alınan veriler SQL önbellek bağımlılıkları kullanımı ile yeni kalmasını sağlamak nasıl ele alacağız. Çıktı önbelleği bu öğreticileri keşfedin değil. Çıktı önbelleği bir ayrıntılı bakış için bkz: [çıktı önbelleği, ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Önbelleğe alma mimarisi herhangi bir yerde veri erişim katmanı yukarı sunu katmanı uygulanabilir. Bu öğreticide ObjectDataSource Denetimi aracılığıyla sunu katmanı için önbelleğe almayı uygulamayı göreceğiz. Biz inceleyeceğiz sonraki öğreticide iş mantığı katmanı verileri önbelleğe alma.

## <a name="key-caching-concepts"></a>Anahtar kavramlar önbelleğe alma

Önbelleğe alma uygulama s önemli ölçüde artırabilir genel performans ve ölçeklenebilirlik oluşturmak pahalıdır veri alma ve daha verimli bir şekilde erişilebilir bir konumda bir kopyasını depolayarak. Önbellek gerçek, temel alınan verilerin yeni bir kopyasını tuttuğundan, eski, olabilir veya *eski*, temel alınan veriler değişirse. Bu mücadele etmek için bir sayfasında geliştirici tarafından önbellek öğesi olacak ölçütleri belirtebilirsiniz *çıkarılacak* önbellekten kullanarak:

- **Zamana bağlı ölçütleri** öğeyi mutlak veya süresi kayan önbelleğine eklenebilir. Örneğin, bir sayfa geliştirici, söyleyin, 60 saniye süre gösteriyor olabilir. Mutlak bir süre ile 60 saniye ne sıklıkta erişildiği bağımsız olarak önbelleğine eklenmiş olan sonra önbelleğe alınan öğe çıkarılacak. Bir kayan süresiyle önbelleğe alınan öğe 60 saniye son erişimden sonra çıkarılacak.
- **Bağımlılık tabanlı ölçütü** bir bağımlılık önbelleğe eklendiğinde, bir öğesiyle ilişkili olabilir. S öğesi bağımlılık değiştiğinde önbellekten çıkarılmasına. Bağımlılık bir dosya, başka bir önbellek öğesi veya ikisinin birleşimi olabilir. ASP.NET 2.0 önbelleğe bir öğe ekleyin ve temel veritabanı veriler değiştiğinde çıkarılacak sağlamak, geliştiricilerin SQL önbellek bağımlılıkları da sağlar. SQL önbellek bağımlılıkları yaklaşan inceleyeceğiz [kullanarak SQL önbellek bağımlılıkları](using-sql-cache-dependencies-cs.md) Öğreticisi.

Çıkarma ölçütlere bağımsız olarak, öğenin önbellekte olabilir *işlemi* zaman veya bağımlılık tabanlı ölçütleri karşılanıyorsa önce. Önbellek kapasitesi ulaştıysa yenilerini eklenebilmesi için önce varolan öğeleri kaldırılmalıdır. Sonuç olarak, programlı olarak önbelleğe alınan verilerle, s, her zaman, varsayılmaktadır önemli çalışırken, önbelleğe alınan veriler mevcut olmayabilir. Veri önbellekten programlı olarak sonraki öğreticimizi erişirken kullanılacak deseni inceleyeceğiz *mimarisinde veri önbelleğe alma*.

Önbelleğe alma, bir uygulamadan daha fazla performans sıkıştırırsanız için ekonomik bir yol sağlar. Olarak [Steven Smith](http://aspadvice.com/blogs/ssmith/) makalede articulates [ASP.NET önbelleğe alma: teknikleri ve en iyi yöntemler](https://msdn.microsoft.com/en-us/library/aa478965.aspx):

Önbelleğe alma, çok fazla zaman ve analiz gerek kalmadan yeterli performans iyi almak için en iyi yolu olabilir. Bellek ucuz, gereken bir gün veya kod veya veritabanı en iyi duruma getirme çalışılırken bir hafta harcama yerine 30 saniye için çıktı önbelleği tarafından performans elde edebilirsiniz, bu nedenle önbelleğe alma çözümünü yapın (30 - ikinci eski verileri varsayılarak Tamam) ve geçin. Elbette, uygulamalarınızın doğru tasarım denemelisiniz şekilde sonuç olarak, zayıf tasarım büyük olasılıkla size yakalar. Ancak hemen bugün yeterli performans iyi almanız gerekirse, önbelleğe alma bir mükemmel [yaklaşımı] zaman uygulamanız varsa bunu yapmak için gereken süre daha sonraki bir tarihte yeniden düzenlemeniz için satın alma.

Önbelleğe alma fark edilir performans iyileştirmeleri sağlayabilir, ancak bu gibi tüm durumlarda uygulanabilir değil gerçek zamanlı, sık sık güncelleştirme veri kullanın ya da eski verileri daha kısa bir süre içinde yaşamakta olduğu kabul edilemez uygulamalarla. Ancak önbelleğe alma uygulamaları çoğunluğu için kullanılmalıdır. ASP.NET 2.0 ile önbelleğe alma hakkında daha fazla arka plan için başvurmak [performans için önbelleğe alma](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) bölümünü [ASP.NET 2.0 hızlı başlangıç öğreticileri](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>1. adım: önbelleğe alma Web sayfaları oluşturma

Biz ObjectDataSource s önbelleğe alma özelliklerini bizim incelenmesi başlamadan önce öncelikle Bu öğretici ve sonraki üç yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `Caching`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![ASP.NET sayfaları için önbelleğe alma ile ilgili öğreticiler ekleme](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Şekil 1**: ASP.NET sayfaları için önbelleğe alma ile ilgili öğreticiler ekleme


Diğer klasörler gibi `Default.aspx` içinde `Caching` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![Şekil 2: için Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Şekil 2**: Şekil 2: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Son olarak, bu sayfaları girişlere eklemek `Web.sitemap` dosya. Özellikle, ikili veri çalıştıktan sonra aşağıdaki biçimlendirme eklemeniz `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Sol menü öğelerini önbelleğe alma öğreticileri için şimdi içerir.


![Site Haritasını girişleri önbelleğe alma öğreticileri için şimdi içerir.](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Şekil 3**: Site Haritası girişleri önbelleğe alma öğreticileri için şimdi içerir.


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>2. adım: bir Web sayfasındaki ürünlerin listesini görüntüleme

Bu öğretici ObjectDataSource Denetimi s yerleşik önbelleğe alma özelliklerinin nasıl kullanılacağını açıklar. Ancak, biz bu özellikleri bakabilirsiniz önce ilk çalışabilmesi için bir sayfa gerekiyor. Let s bir ObjectDataSource tarafından alınan ürün bilgilerini listelemek için GridView kullanan bir web sayfası oluşturmak `ProductsBLL` sınıfı.

Başlangıç açarak `ObjectDataSource.aspx` sayfasındaki `Caching` klasör. GridView tasarımcıya araç çubuğuna sürükleyin, Ayarla kendi `ID` özelliğine `Products`ve akıllı etiketten adlı yeni bir ObjectDataSource denetimi bağlamak isterseniz `ProductsDataSource`. ObjectDataSource çalışmak için yapılandırma `ProductsBLL` sınıfı.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Bu sayfa için düzenlenebilir GridView oluşturun, böylece biz ObjectDataSource önbelleğe alınmış verileri GridView s arabirimi aracılığıyla değiştirildiğinde olanlar inceleyebilirsiniz s olanak tanır. Aşağı açılan listesinde, varsayılan, SELECT sekmesinde bırakın `GetProducts()`, ancak UPDATE sekmesi seçili öğe değiştirmek `UpdateProduct` kabul aşırı `productName`, `unitPrice`, ve `productID` kendi giriş parametresi olarak.


[![Güncelleştirme sekmesi s açılır listede uygun UpdateProduct aşırı yüklemesine ayarlayın](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Şekil 5**: UPDATE sekmesi s aşağı açılan liste için uygun ayarlamak `UpdateProduct` aşırı yükleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Son olarak, açılan listeleri INSERT ve DELETE sekmeler (hiçbiri) olarak ayarlayın ve Son'u tıklatın. Veri Kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra Visual Studio ObjectDataSource s ayarlar `OldValuesParameterFormatString` özelliğine `original_{0}`. ' Da anlatıldığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğretici, bu özellik gereken bildirim temelli söz dizimi kaldırıldı veya varsayılan değer olarak, döndürülmek `{0}`, bizim güncelleştirme iş akışı için sırayla hatasız devam edin.

Ayrıca, sihirbaz tamamlandığında Visual Studio bir alan için GridView ürün veri alanların her biri için ekler. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, ve `UnitPrice` BoundFields. Ardından, güncelleştirme `HeaderText` her ürün, kategori ve fiyat, bu BoundFields özelliklerine sırasıyla. Bu yana `ProductName` alan gereklidir, TemplateField'a BoundField dönüştürmek ve bir RequiredFieldValidator eklemek `EditItemTemplate`. Benzer şekilde, dönüştürme `UnitPrice` bir TemplateField içine BoundField ve kullanıcı tarafından girilen değer geçerli bir para birimi değeri bu s değerinden büyük veya sıfıra eşit olduğundan emin olmak için bir CompareValidator ekleyin. Bu değişiklikler yanı sıra sağa hizalama gibi estetik değişiklikleri gerçekleştirmek çekinmeyin `UnitPrice` değeri veya için biçimlendirme belirterek `UnitPrice` salt okunur ve düzenleme arabirimlerinden metin.

GridView düzenlenebilir GridView s akıllı etiket düzenlemeyi etkinleştir onay kutusunu işaretleyerek olun. Ayrıca etkinleştirmek disk belleği ve etkinleştirme sıralama onay kutularını denetleyin.

> [!NOTE]
> GridView s düzenleme arabirimini özelleştirmek nasıl incelenmesi gerekiyor? Bunu geri oluştuysa, [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Öğreticisi.


[![Düzenleme, sıralama ve disk belleği GridView desteğini etkinleştir](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Şekil 6**: sıralama ve disk belleği düzenlemek için GridView desteğini etkinleştir ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Bu GridView değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Şekil 7'de görüldüğü gibi ad, kategori ve her bir veritabanı ürünlerinde fiyat düzenlenebilir GridView listeler. Sayfa s işlevselliği sıralama test sonuçları, bunları aracılığıyla sayfa için bir dakikanızı ayırın ve bir kayıt düzenleyin.


[![Her ürün s ad, kategori ve fiyat listelenen sıralanabilir, Pageable, düzenlenebilir GridView](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Şekil 7**: her ürün s ad, kategori ve fiyat sıralanabilir, Pageable, düzenlenebilir GridView listelenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>3. adım: Zaman ObjectDataSource inceleniyor isteyen veridir

`Products` GridView çağırarak görüntülemek için kendi verilerini alır `Select` yöntemi `ProductsDataSource` ObjectDataSource. Bu ObjectDataSource iş mantığı katmanı s örneği oluşturur `ProductsBLL` sınıfı ve çağrıları kendi `GetProducts()` sırayla veri erişim katmanı s çağırır yöntemi `ProductsTableAdapter` s `GetProducts()` yöntemi. DAL yöntemi Northwind veritabanına bağlanır ve yapılandırılmış sorunları `SELECT` sorgu. Bu veriler sonra içinde paketler yukarı DAL döndürülen bir `NorthwindDataTable`. DataTable nesnesini GridView döndürür ObjectDataSource döndürür BLL döndürülür. GridView ardından oluşturan bir `GridViewRow` her nesne `DataRow` DataTable ve her `GridViewRow` sonunda, istemciye döndürülen ve ziyaretçi s tarayıcıda görüntülenen HTML halinde işlenir.

GridView, temel alınan veri bağlamak için gereken her zaman bu olaylar dizisi gerçekleşir. Sayfa ilk, GridView sıralarken ya da düzenleme veya silme arabirimleri yerleşik GridView s verilerine değiştirirken veri bir sayfadan diğerine taşırken, ziyaret edilen, olur. GridView s görünüm durumu devre dışıysa, GridView her geri göndermede de DataSet'e. GridView de açıkça verileriyle çağırarak DataSet'e kendi `DataBind()` yöntemi.

Veritabanından alınan veri ile sıklığı tam olarak anlamak için verileri yeniden alınan yüklenirken belirten bir ileti görüntüler s olanak tanır. Bir etiket Web denetimi adlı GridView yukarıda eklemek `ODSEvents`. Temizle kendi `Text` özelliği ve kümesi kendi `EnableViewState` özelliğine `false`. Etiketi altında bir düğme Web denetimi ekleyin ve ayarlayın, `Text` geri gönderme özelliği.


[![GridView yukarıda sayfaya bir etiket ve düğme ekleme](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Şekil 8**: sayfa yukarıda GridView için bir etiket ve düğmesi ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Veri erişim iş akışı, ObjectDataSource s sırasında `Selecting` nesnesini oluşturulmadan önce olay etkinleşir ve kendi yapılandırılmış yöntemi çağrılır. Bu olay için bir olay işleyicisi oluşturun ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

ObjectDataSource mimarisinde, verileri için istekte her zaman harekete metin seçme olayı etiketini görüntüler.

Bir tarayıcıda bu sayfasını ziyaret edin. Sayfa ilk sitesini ziyaret ettiğinizde harekete metin seçme olayı gösterilir. Geri gönderme düğmesini tıklatın ve metin kaybolması unutmayın (varsayılarak GridView s `EnableViewState` özelliği ayarlanmış `true`, varsayılan). Geri göndermede, GridView Görünüm durumunu yeniden düzenlenir ve bu nedenle içermiyor t açın verilerini ObjectDataSource için nedeni budur. Sıralama, disk belleği veya verileri düzenleme ancak, kendi veri kaynağına rebind GridView neden olur ve bu nedenle metin görüntülenir seçme olay tetiklenir.


[![GridView kendi veri kaynağına DataSet'e her seçme olay harekete görüntülenir](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Şekil 9**: her GridView kendi veri kaynağına DataSet'e, seçme olay harekete görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Görünüm durumuna oluşturulmadan GridView tıklatarak geri gönderme düğmesini neden olur](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Şekil 10**: neden olan kendi görünüm durumundan oluşturulmadan GridView geri gönderme düğmesini tıklatarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Veri aracılığıyla disk belleğine alınan ya da sıralanmış her zaman veritabanı verileri almak üzere kayıp görünebilir. Tüm, biz bu yana varsayılan disk belleği kullanarak yeniden ObjectDataSource tüm kayıtlar ilk sayfa görüntülenirken alınan. GridView, sıralama ve Destek disk belleği sağlamaz olsa bile, veri sayfasını önce herhangi bir kullanıcı tarafından (ve görünüm durumu devre dışı bırakılırsa her geri gönderme) ziyaret her zaman veritabanından alınmalıdır. Ancak GridView, tüm kullanıcılara aynı veri gösteriliyorsa, bu ek veritabanı istekleri gereksiz. Neden döndürülen sonuçları önbelleğe `GetProducts()` yöntemi ve bağ GridView olanlar için önbelleğe alınan sonuçları?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>4. adım: ObjectDataSource kullanarak verileri önbelleğe alma

Yalnızca birkaç özelliklerini ayarlayarak ObjectDataSource otomatik olarak ASP.NET veri önbelleğindeki alınan verileri önbelleğe almak için yapılandırılabilir. Aşağıdaki listede ObjectDataSource önbellek ilgili özellikleri özetlenmektedir:

- [EnableCaching](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) ayarlanmalıdır `true` önbelleğe almayı etkinleştirmek için. Varsayılan, `false` değeridir.
- [CacheDuration](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) verileri önbelleğe saniye cinsinden süre miktarı. Varsayılan değer 0'dır. ObjectDataSource yalnızca veri varsa önbellekte `EnableCaching` olan `true` ve `CacheDuration` sıfırdan büyük bir değere ayarlanır.
- [CacheExpirationPolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) ayarlanabilir `Absolute` veya `Sliding`. Varsa `Absolute`, ObjectDataSource önbelleğe alınan verileri için `CacheDuration` ; varsa saniye `Sliding`, yalnızca bu için erişildikten değil sonra verilerin süresi sona `CacheDuration` saniye. Varsayılan, `Absolute` değeridir.
- [CacheKeyDependency](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) ObjectDataSource s önbellek girişlerinin varolan bir önbellek bağımlılığı ile ilişkilendirmek için bu özelliği kullanın. ObjectDataSource s veri girişi erken önbellekten, ilişkili süresinin dolmasını sağlayarak çıkarılmasına `CacheKeyDependency`. Bu özellik SQL önbellek bağımlılığı ObjectDataSource s önbellek ile ilişkilendirmek için en yaygın olarak kullanılır, bir konu biz gelecekte ele alacağız [kullanarak SQL önbellek bağımlılıkları](using-sql-cache-dependencies-cs.md) Öğreticisi.

Yapılandırma s izin `ProductsDataSource` mutlak ölçekte 30 saniye boyunca verileri önbelleğe almak için ObjectDataSource. ObjectDataSource s ayarlamak `EnableCaching` özelliğine `true` ve kendi `CacheDuration` 30 özelliği. Bırakın `CacheExpirationPolicy` özelliği, varsayılan olarak ayarlanmış `Absolute`.


[![ObjectDataSource 30 saniye boyunca verileri önbelleğe almak için yapılandırma](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Şekil 11**: 30 saniye boyunca verileri önbelleğe almak için ObjectDataSource yapılandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Yaptığınız değişiklikleri kaydedin ve bu sayfasını bir tarayıcıda yeniden ziyaret. İlk sayfasını ziyaret ettiğinizde metin seçme olay harekete başlangıçta veriler önbellekte değil olarak görünür. Ancak disk belleği veya Edit ya da İptal düğmeleri tıklatarak sıralama, geri gönderme düğmesini tıklatarak tetiklenen sonraki Geri göndermeler *değil* yeniden seçme olay harekete metin. Bunun nedeni, `Selecting` olay yalnızca ateşlenir ObjectDataSource, temel alınan nesnesinden; verisini alır `Selecting` olayını değil ateşle verileri veri önbelleğinden çekilir durumunda.

30 saniye sonra verilerin önbellekten çıkarılmasına. Verileri de önbellekten varsa çıkarılmasına ObjectDataSource s `Insert`, `Update`, veya `Delete` yöntemleri çağrılır. Sonuç olarak, 30 saniye geçtikten veya güncelleştirme düğmesi tıklatıldıktan, sıralama, disk belleği'ni veya Edit ya da İptal düğmeleri temel nesnesinden verileri almak ObjectDataSource neden olacak sonra seçme olay görüntüleme metni harekete olduğunda `Selecting` olay etkinleşir. Döndürülen sonuçlar veri önbelleğine yerleştirilir.

> [!NOTE]
> Metin seçme harekete olay sık sık görürseniz, önbelleğe alınan verilerle çalışmak için ObjectDataSource beklediğiniz bellek kısıtlamaları nedeniyle olabilir. Yeterli bellek değilse ObjectDataSource tarafından önbelleğe eklenen veriler attı. Veri ya da yalnızca önbellekleri önbelleğe doğru için ObjectDataSource içermiyor t görünüyorsa, verilerin düzensiz aralıklarla hataya, belleği boşaltmak için bazı uygulamaları kapatın ve yeniden deneyin.


Şekil 12 iş akışı önbelleğe alma ObjectDataSource s gösterilmektedir. Seçme olay şu metin ekranda görüntülenen, çünkü veri önbellekte olmadığı ve temel nesneden alınması gerekiyordu. Bu metin eksik olduğunda ancak onu s önbellekten veri kullanılabilir olduğu için. Veri önbelleğinden döndürüldüğünde nesnesini için çağrı yok ve bu nedenle, hiçbir veritabanı sorgusu yürütülen orada s.


![ObjectDataSource depoları ve veri önbellek verilerini alır](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Şekil 12**: ObjectDataSource depolar ve veri önbelleğinden verisini alır.


Her ASP.NET uygulamasının tüm sayfaları ve ziyaretçileri paylaşılan bu s örnek kendi veri önbelleği vardır. ObjectDataSource tarafından veri önbellekte depolanan veriler benzer şekilde sayfasını ziyaret edin tüm kullanıcılar arasında paylaşılan anlamına gelir. Bunu doğrulamak için açık `ObjectDataSource.aspx` sayfasını bir tarayıcıda. (Önceki testleri tarafından önbelleğe eklenen veriler artık çıkarıldı varsayılarak) ilk sitesini ziyaret ettiğinde metin seçme olay harekete görünür. İkinci tarayıcı örneği ve kopyalama açın ve ikinci ilk tarayıcı örneğinden URL'sini yapıştırın. İkinci tarayıcı örneğinde metin seçme olay harekete çünkü gösterilmez, aynı kullanarak s önbelleğe alınmış verileri ilk olarak.

Önbelleğe alınan veriler ekleme yaparken ObjectDataSource içeren bir önbellek anahtar değerini kullanır: `CacheDuration` ve `CacheExpirationPolicy` özellik değerlerini; belirtilen ObjectDataSource tarafından kullanılmakta olan iş nesnesini türü aracılığıyla [ `TypeName` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, bu örnekte); değerini `SelectMethod` özellik adını ve parametre değerlerini `SelectParameters` koleksiyonu; ve kendi değerlerini`StartRowIndex`ve `MaximumRows` uygularken kullanılan özellikler [özel sayfalama.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Önbellek anahtar değeri bu özelliklerin bir birleşimi hazırlayın, bu değerlerden değiştirdikçe benzersiz önbellek girişi sağlar. Örneğin, son öğreticileri, biz ve arama kullanarak `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)`, belirtilen bir kategorideki tüm ürünleri döndürür. Bir kullanıcı gelen sayfası ve görünüm beverages olarak sahip olduğu bir `CategoryID` 1. ObjectDataSource sonuçlarını bakmadan önbelleğe varsa `SelectParameters` değerlerini, başka bir kullanıcı için sayfanın geldiği zaman condiments içecek ürünleri çalışırken görüntülemek için önbellekte, d condiments yerine önbelleğe alınmış içecek ürünleri görürler. Bu özellik tarafından önbellek anahtarını değiştirerek, aşağıdakileri içeren değerlerini `SelectParameters`, ayrı önbellek girişi Meşrubat ve condiments ObjectDataSource korur.

## <a name="stale-data-concerns"></a>Eski veri sorunları

ObjectDataSource öğelerinden herhangi biri, önbellek otomatik olarak çıkarır, `Insert`, `Update`, veya `Delete` yöntemleri çağrılır. Bu sayfadan veri değiştirildiğinde önbellek girişlerinin temizleyerek eski veri karşı korunmasına yardımcı olur. Ancak, hala eski verileri görüntülemek için önbelleğe alma kullanan bir ObjectDataSource için mümkündür. En basit durumda, doğrudan veritabanı içinden değiştirme veriler nedeniyle olabilir. Belki de bir veritabanı yöneticisi yalnızca veritabanındaki kayıtları bazıları değiştiren bir komut dosyası verdi.

Bu senaryo ayrıca daha zarif bir şekilde unfold. Veri değişikliği yöntemlerinden birini çağrıldığında ObjectDataSource öğelerinden önbellekten çıkarır olsa da, kaldırılan önbelleğe alınmış öğeleri ObjectDataSource s belirli özellik değerleri için birleşimidir (`CacheDuration`, `TypeName`, `SelectMethod`, vb.). Farklı kullanan iki ObjectDataSources varsa `SelectMethods` veya `SelectParameters`, ancak bir ObjectDataSource bir satırı güncelleştirme ve kendi önbellek girişlerinin, ancak karşılık gelen bir satır için ikinci ObjectDataSource geçersiz hala aynı verileri güncelleştirebilirsiniz hala sunulacak gelen önbelleğe alınmış. Bu işlev sergilemesine sayfalarına oluşturmanızı öneririz. Verileri önbelleğe alma kullanan ve verileri almak üzere yapılandırılan bir ObjectDataSource gelen çeker düzenlenebilir bir GridView görüntüleyen bir sayfa oluşturma `ProductsBLL` s sınıfı `GetProducts()` yöntemi. Başka bir tane eklemek düzenlenebilir GridView ve ObjectDataSource bu ikinci ObjectDataSource yanı sıra, bu sayfayı (veya başka bir tane) sahip bunu kullanmak `GetProductsByCategoryID(categoryID)` yöntemi. İki ObjectDataSources itibaren `SelectMethod` özellikleri farklılık, bunlar üm her önbelleğe alınmış değerlerine sahip. Verileri geri diğer kılavuza (disk belleği, sıralama ve benzeri tarafından) Bağla başlatıldığında bir kılavuz, bir üründe düzenlerseniz, hala eski, önbelleğe alınan veriler hizmet ve diğer kılavuzdan yapılan değişiklik yansıtacak değil.

Kısacası, eski veri potansiyeline sahiptir ve veri yenilik önemli olduğu senaryolar için daha kısa expiries kullanmak istiyorum varsa yalnızca zaman tabanlı expiries kullanın. Eski verileri kabul edilebilir değilse, önbelleğe alma atlayabilirsiniz veya SQL önbellek bağımlılıkları kullanın (veritabanı veri olduğunu varsayarak, önbelleğe alma re). Biz, gelecekteki bir öğreticide SQL önbellek bağımlılıkları ele alacağız.

## <a name="summary"></a>Özet

Bu öğreticide ObjectDataSource s yerleşik önbelleğe alma özellikleri incelendi. Yalnızca birkaç özellikleri ayarlayarak, biz belirtilen döndürülen sonuçlar önbelleğe almak için ObjectDataSource söyleyebilirsiniz `SelectMethod` ASP.NET veri önbelleğine. `CacheDuration` Ve `CacheExpirationPolicy` özellikleri, öğenin önbelleğe alınma süresi ve mutlak veya kayan bitiş tarihinin olup olmadığını gösterir. `CacheKeyDependency` Özelliği tüm ObjectDataSource s önbellek girişlerinin varolan bir önbellek bağımlılığı ile ilişkilendirir. Zamana bağlı süre sonu ulaşıldığında ve genellikle SQL önbellek bağımlılıkları ile kullanılan önce ObjectDataSource s girişlerin önbellekten çıkarmak için kullanılabilir.

ObjectDataSource yalnızca veri önbelleğine değerlerini önbelleğe aldığından, biz ObjectDataSource s yerleşik işlevsellik program aracılığıyla çoğaltabilirsiniz. Bunu mevcut değil t algılama sunuyu katmanında ObjectDataSource kutunun dışında bu işlevselliği sunar, ancak önbelleğe alma özelliklerini mimarisinin ayrı bir katmana uygulayabilirsiniz olduğundan bunu yapın. Bunu yapmak için biz ObjectDataSource tarafından kullanılan aynı mantığı yinelemeniz gerekir. Biz program aracılığıyla mimarisi içinde veri önbelleğinden bizim sonraki öğreticide çalışmak nasıl ele alacağız.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET önbelleğe alma: Teknikleri ve en iyi uygulamalar](https://msdn.microsoft.com/en-us/library/aa478965.aspx)
- [.NET Framework uygulamaları için önbelleğe alma Mimarisi Kılavuzu](https://msdn.microsoft.com/en-us/library/ee817645.aspx)
- [ASP.NET 2.0 çıkış önbelleğe alma](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Teresa Murphy oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Sonraki](caching-data-in-the-architecture-cs.md)
