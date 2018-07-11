---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: (VB) ObjectDataSource ile verileri önbelleğe alma | Microsoft Docs
author: rick-anderson
description: Önbelleğe alma, yavaş ve hızlı bir Web uygulaması arasındaki fark anlamına gelebilir. Bu öğreticide, ASP.NET önbelleğe alma ayrıntılı göz atalım dört davranıştır...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: aa2af934a45ebd7e23d5d2ccf5a80f4949f1ec4f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817544"
---
<a name="caching-data-with-the-objectdatasource-vb"></a>(VB) ObjectDataSource ile verileri önbelleğe alma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe) veya [PDF olarak indirin](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> Önbelleğe alma, yavaş ve hızlı bir Web uygulaması arasındaki fark anlamına gelebilir. Bu öğretici, ASP.NET önbelleğe alma ayrıntılı göz atalım dört ilk bölümüdür. Önbelleğe alma temel kavramları ve uygulama için sunu katmanı aracılığıyla ObjectDataSource Denetimi önbelleğe alma öğrenin.


## <a name="introduction"></a>Giriş

Bilgisayar bilimleri *önbelleğe alma* , veri ya da elde etmek pahalı bilgisi alma ve erişimi daha hızlı bir şekilde bir konumda bir kopyasını depolayarak işlemidir. Veri odaklı uygulamalar için büyük ve karmaşık sorgular genellikle uygulama s yürütme süresi çoğunu kullanır. Bu tür bir uygulama s performansı, daha sonra genellikle uygulama s bellekte pahalı veritabanı sorguların sonuçlarını depolayarak geliştirilebilir.

ASP.NET 2.0 seçenekleriyle önbelleğe almayı çeşitli sunar. Bir web sayfasının tamamını veya kullanıcı denetimi işlenen s biçimlendirme aracılığıyla önbelleğe alınabilir *çıktı önbelleği*. ObjectDataSource ve SqlDataSource denetimleri denetimi düzeyinde önbelleğe alınması için önbelleğe alma özellikleri de, böylece veri izin vererek sağlar. Ve ASP.NET s *veri önbelleğini* sayfa geliştiricilerin önbellek nesnelere program aracılığıyla sağlayan zengin bir önbelleğe alma API sağlar. Bu öğretici ve ObjectDataSource s kullanarak inceleyeceğiz sonraki üç veri önbelleğini yanı sıra özellikleri önbelleğe alma. Biz de birçok farklı uygulama başlangıcında verileri önbelleğe alınacağını ve önbelleğe alınan verilerin SQL önbellek bağımlılıklarını genelindeki güncel kalması nasıl üzerinde durulacaktır. Çıktı önbelleği bu öğreticileri keşfedin değil. Çıkış önbelleğe alma, ayrıntılı bilgi için bkz. [çıktı önbelleği, ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Önbelleğe alma herhangi bir yerde mimarisi veri erişim katmanından yukarı sunu katmanı uygulanabilir. Bu öğreticide, uygulamayı sunu katmanı aracılığıyla ObjectDataSource denetimi için önbelleğe alma inceleyeceğiz. Sonraki öğreticide inceleyeceğiz iş mantığı katmanı verileri önbelleğe alma.

## <a name="key-caching-concepts"></a>Anahtar kavramlar önbelleğe alma

Önbelleğe alma uygulama s büyük ölçüde geliştirebilir genel performans ve ölçeklenebilirlik oluşturulması maliyetli olan verileri alma ve daha verimli bir şekilde erişilebilir bir konumda bir kopyasını depolayarak. Önbellek gerçek, temel alınan verilerin yalnızca bir kopyasını tuttuğu eski, olabilir veya *eski*, temel alınan verileri değişirse. Bu sorunla başa çıkmak için sayfasında geliştirici tarafından önbellek öğesinin olacak ölçüt belirtebilirsiniz *çıkarılacak* önbellekten kullanarak:

- **Zamana bağlı ölçütleri** bir öğe için mutlak ya da süresi kayan önbelleğe eklenebilir. Örneğin, bir sayfa geliştirici, örneğin 60 saniye süre gösterebilir. Mutlak bir süre ile 60 saniye önbelleğine ne sıklıkla erişilen bağımsız olarak eklendikten sonra önbelleğe alınan öğeyi çıkarılır. Bir kayan süresi ile 60 saniye sonra son erişim önbelleğe alınan öğeyi çıkarılır.
- **Bağımlılık alan ölçütleri** bağımlılık önbelleğe eklenen bir öğe ile ilişkili olabilir. S öğesi bağımlılık değiştiğinde, önbellekten çıkarılır. Bağımlılık, bir dosya, başka bir önbellek öğesi veya ikisinin bir birleşimi olabilir. ASP.NET 2.0 önbelleğe bir öğe ekleyin ve temel alınan veritabanı veriler değiştiğinde çıkarılmasını sağlamak geliştiricilerin SQL önbellek bağımlılıklarını de sağlar. SQL önbellek bağımlılıklarını yaklaşan içinde inceleyeceğiz [SQL önbellek bağımlılıklarını kullanma](using-sql-cache-dependencies-vb.md) öğretici.

Çıkarma ölçütlere bağımsız olarak, bir öğeyi önbellekte olabilir *işlemi* önce zaman veya bağımlılık tabanlı ölçütleri karşıladığında. Önbellek kapasitesi ulaştıysa, yeni bir tane eklemeden önce var olan öğeleri kaldırılması gerekir. Sonuç olarak, program aracılığıyla önbelleğe alınmış veri, s, her zaman, varsayıyorsunuz önemli çalışırken önbelleğe alınan verilerin mevcut olmayabilir. Verileri önbellekten program aracılığıyla sonraki müşterilerimize öğreticide erişirken kullanılacak düzeni inceleyeceğiz *mimaride verileri önbelleğe alma*.

Önbelleğe alma, bir uygulamadan daha fazla performans sıkıştırırsanız için ekonomik bir yol sağlar. Olarak [Steven Smith](http://aspadvice.com/blogs/ssmith/) makalede korumadaki [ASP.NET önbelleğe alma: teknikleri ve en iyi](https://msdn.microsoft.com/library/aa478965.aspx):

Önbelleğe alma, çok fazla zaman ve analiz gerek kalmadan yeterli performans iyi almak için en iyi yolu olabilir. Bellek ucuz, önbelleğe alma çözümü ihtiyaç gün veya haftada bir kod veya veritabanı iyileştirmeye çalışıyor yerine 30 saniye için çıktı önbelleği tarafından performansı elde edebilirsiniz, bu nedenle yapın (30 - eski ikinci veri olarak sayılıyor olabilir) ve geçin. Tabii ki uygulamalarınızı doğru tasarım denemelisiniz için sonuç olarak, kötü tasarım büyük olasılıkla sizin için yakalar. Ancak hemen bugün yeterli performans iyi almanız gerekirse, önbelleğe alma bir mükemmel [yaklaşım] olabilir zaman uygulamanız varsa bunu yapmak için zaman daha sonraki bir tarihte yeniden satın.

Önemli performans geliştirmeleri önbelleğe alma sağlarken, ancak gibi tüm durumlarda geçerli değildir uygulamalarla gerçek zamanlı, sık sık güncelleştirme veri kullanan ya da kısa süreli eski veri olduğu kabul edilemez. Ancak, önbelleğe alma uygulamalarının çoğu için kullanılmalıdır. ASP.NET 2.0 ile önbelleğe alma hakkında daha fazla arka plan için başvurmak [performans için önbelleğe alma](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) bölümünü [ASP.NET 2.0 hızlı başlangıç öğreticileri](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>1. adım: önbelleğe alma Web sayfaları oluşturma

Size sunduğumuz araştırma ObjectDataSource s önbelleğe alma özellikleri başlamadan önce ilk ASP.NET sayfaları ve bu öğreticinin sonraki üç yapmamız gereken bizim Web sitesi projesi oluşturmak için bir dakikanızı ayırarak s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `Caching`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![ASP.NET sayfaları için önbelleğe alma ile ilgili öğreticiler Ekle](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**Şekil 1**: ASP.NET sayfaları için önbelleğe alma ile ilgili öğreticiler Ekle


Diğer klasörler gibi `Default.aspx` içinde `Caching` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![Şekil 2: SectionLevelTutorialListing.ascx kullanıcı denetimi için Default.aspx ekleyin.](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**Şekil 2**: Şekil 2: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image4.png))


Son olarak, girişleri olarak bu sayfalar ekleme `Web.sitemap` dosya. Özellikle, ikili verilerle çalışma aşağıdaki işaretlemeyi ekleyin `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, önbelleğe alma öğreticileri için artık öğelerini içerir.


![Site Haritası girişleri için önbelleğe alma öğreticiler artık içerir.](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**Şekil 3**: Site Haritası girişleri için önbelleğe alma öğreticiler artık içerir.


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>2. adım: bir Web sayfasındaki ürünlerin listesini görüntüleme

Bu öğretici, ObjectDataSource Denetimi s yerleşik önbelleğe alma özelliklerinin nasıl kullanılacağını açıklar. Ancak, bu özellikleri bakacağız önce ilk çalışmak için bir sayfa gerekiyor. Let s bir ObjectDataSource tarafından alınan ürün bilgilerini listelemek GridView kullanan bir web sayfası oluşturma `ProductsBLL` sınıfı.

Başlangıç açarak `ObjectDataSource.aspx` sayfasını `Caching` klasör. GridView tasarımcı araç kutusundan sürükleyin, ayarla, `ID` özelliğini `Products`ve adlı yeni bir ObjectDataSource denetimine bağlamak, akıllı etiketten seçin `ProductsDataSource`. ObjectDataSource ile çalışmak için yapılandırma `ProductsBLL` sınıfı.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image8.png))


Bu sayfa için düzenlenebilir bir GridView oluşturun, böylece biz ObjectDataSource önbelleğe alınmış veri GridView s arabirimi aracılığıyla değiştirildiğinde ne inceleyebilirsiniz s olanak tanır. Aşağı açılan listesinde, varsayılan ayarına seçme sekmesinde bırakın `GetProducts()`, ancak seçili öğeyi güncelleştirme için sekmesinde değiştirmek `UpdateProduct` kabul eden aşırı yükleme `productName`, `unitPrice`, ve `productID` giriş parametre olarak.


[![Güncelleştirme sekmesini s açılır listede uygun UpdateProduct aşırı ayarlayın](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**Şekil 5**: UPDATE sekmesi s açılır listede uygun ayarlamak `UpdateProduct` aşırı yükleme ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image11.png))


Son olarak, açılan listeler INSERT ve DELETE sekmeler (yok) olarak ayarlayın ve Son'a tıklayın. Veri Kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra Visual Studio ObjectDataSource s ayarlar `OldValuesParameterFormatString` özelliğini `original_{0}`. Bölümünde açıklandığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) öğretici, bu özellik olması gerekiyor bildirim temelli söz dizimi kaldırıldı ya da geri varsayılan değerine ayarlanmış `{0}`, bizim güncelleştirme iş akışı için sırayla hatasız devam edin.

Ayrıca, sihirbaz tamamlandığında Visual Studio bir alan için GridView ürün veri alanların her biri için ekler. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, ve `UnitPrice` BoundFields. Ardından, güncelleştirme `HeaderText` özelliklerini bu BoundFields ürün, kategori ve fiyat, sırasıyla. Bu yana `ProductName` alan gereklidir, bir TemplateField BoundField dönüştürmek ve eklemek için bir RequiredFieldValidator `EditItemTemplate`. Benzer şekilde, dönüştürme `UnitPrice` BoundField bir TemplateField oturum ve kullanıcı tarafından girilen değer geçerli bir para birimi değeri, s değerinden büyük veya sıfıra eşit olduğundan emin olmak için bir CompareValidator ekleyin. Bu değişikliklerin yanı sıra sağa hizalama gibi estetik değişiklikleri gerçekleştirmek çekinmeyin `UnitPrice` değeri veya için biçimlendirme belirtme `UnitPrice` salt okunur ve düzenleme arabirimlerinden metin.

Akıllı etiket s GridView düzenlemeyi etkinleştir onay kutusunu işaretleyerek GridView düzenlenebilir hale getirin. Ayrıca etkinleştirme sayfalama ve sıralamayı etkinleştir onay kutularını kontrol edin.

> [!NOTE]
> GridView s düzenleme arabirimini özelleştirme incelenmesi gerekiyor? Bu durumda, kiracıurl [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) öğretici.


[![Düzenleme, sıralama ve disk belleği GridView desteğini etkinleştir](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**Şekil 6**: sıralama ve disk belleği düzenlemek için GridView desteğini etkinleştir ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image14.png))


Bu GridView değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

Şekil 7 gösterildiği gibi ad, kategori ve her veritabanında ürünlerin fiyat düzenlenebilir GridView listeler. Sayfası s işlevleri sıralama test sonuçları, bunları, sayfa için bir dakikanızı ayırın ve bir kayıt düzenleyin.


[![Her ürün s ad, kategori ve fiyat listelenen sıralanabilir, Pageable, düzenlenebilir GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**Şekil 7**: her ürün s ad, kategori ve fiyat sıralanabilir, Pageable, düzenlenebilir GridView listelenir ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>3. adım: İsteyen veri olduğu zaman ObjectDataSource İnceleme

`Products` GridView çağırarak görüntülenecek verisini alır `Select` yöntemi `ProductsDataSource` ObjectDataSource. Bu ObjectDataSource iş mantığı katmanı s örneği oluşturur `ProductsBLL` sınıfı ve çağrılarını kendi `GetProducts()` sırayla veri erişim katmanı s çağıran yöntem `ProductsTableAdapter` s `GetProducts()` yöntemi. DAL yöntemi Northwind veritabanına bağlanır ve sorunları yapılandırılmış `SELECT` sorgu. Bu veriler daha sonra içindeki paketleri yukarı DAL döndürülür bir `NorthwindDataTable`. GridView'a döndürür ObjectDataSource döndürür BLL DataTable nesne döndürülür. GridView ardından oluşturan bir `GridViewRow` her nesne `DataRow` DataTable ve her `GridViewRow` sonunda istemciye döndürülen ve ziyaretçi s tarayıcıda görüntülenen HTML olarak işlenir.

GridView, temel alınan verilere bağlamak için gereken her zaman bu olaylar dizisi gerçekleşir. Sayfanın ilk kez, bir veri sayfasından GridView sıralama veya düzenleme veya silme arabirimleri yerleşik GridView s verilerine değiştirirken diğerine taşırken ziyaret edildiğinde durum ortaya çıkar. GridView s görünüm durumu devre dışı bırakılırsa, GridView her geri göndermede de DataSet'e. GridView de açıkça verileriyle çağırarak DataSet'e kendi `DataBind()` yöntemi.

Veritabanından alınan veri ile sıklığı tam olarak anlamak için verileri yeniden alınan zamanı belirten bir ileti görüntüler s olanak tanır. Etiket Web denetim adlı GridView yukarıda ekleme `ODSEvents`. Temizle kendi `Text` özelliği ve kümesi kendi `EnableViewState` özelliğini `False`. Etiketi altında bir düğme Web denetimi ekleyip ayarlayın, `Text` özelliğini geri gönderme.


[![GridView yukarıda sayfasına bir etiket ve düğme ekleme](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**Şekil 8**: sayfa yukarıda GridView için bir etiket ve düğme ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image20.png))


Veri erişim iş akışı, ObjectDataSource s sırasında `Selecting` temel alınan nesne oluşturulmadan önce olay harekete geçirilir ve kendi yapılandırılmış yöntemi çağrılır. Bu olay için bir olay işleyicisi oluşturun ve aşağıdaki kodu ekleyin:


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

ObjectDataSource veri mimarisi için istekte bulunan her zaman harekete metin seçme olay etiketini görüntüler.

Bir tarayıcıda bu sayfasını ziyaret edin. Sayfa ilk ziyaret edildiğinde metin seçme olayı harekete gösterilir. Geri gönderme düğmesine tıklayın ve metin kaybolduğuna dikkat edin (varsayarak GridView s `EnableViewState` özelliği `True`, varsayılan). GridView Görünüm durumuna geri göndermede, yeniden düzenlenir ve bu nedenle eklenmemişse t açın verilerini ObjectDataSource için nedeni budur. Sıralama, sayfalama ve veri düzenleme ancak kendi veri kaynağı için yeniden bağlamaya GridView neden olur ve bu nedenle seçme olay metin yeniden harekete.


[![GridView, kendi veri kaynağına DataSet'e her seçme olay harekete görüntülenir](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**Şekil 9**: her GridView kendi veri kaynağına DataSet'e, seçme olay harekete görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![GridView Görünüm durumuna oluşturulmadan geri göndermenin neden düğmesine tıklayarak](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**Şekil 10**: geri gönderme düğmeye tıklandığında GridView Görünüm durumuna oluşturulmadan neden olur ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image26.png))


Veritabanı verileri her zaman veri aracılığıyla disk belleğine alınan veya sıralanmış almak üzere kısıp görünebilir. Sonuçta, biz bu yana varsayılan disk belleği'ni kullanarak yeniden ObjectDataSource tüm kayıtlar ilk sayfa görüntülenirken almıştır. GridView sıralama ve disk belleği desteği sağlamaz bile veri sayfası ilk herhangi bir kullanıcı tarafından (ve görünüm durumunu devre dışı bırakılırsa her geri gönderme,) ziyaret edilen her zaman veritabanından alınmalıdır. Ancak bu ek veritabanı istekleri GridView tüm kullanıcılar aynı verileri gösteriliyorsa, gereksiz olur. Neden döndürülen sonuçları önbelleğe `GetProducts()` yöntemi ve bağlama GridView olanlar için önbelleğe alınan sonuçları?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>4. adım: verileri ObjectDataSource kullanarak önbelleğe alma

Yalnızca birkaç özelliklerini ayarlayarak ObjectDataSource otomatik olarak ASP.NET verileri önbelleğe alınan verilerini önbelleğe almak için yapılandırılabilir. Aşağıdaki listede ObjectDataSource önbellekle ilgili özellikleri özetlenmektedir:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) ayarlanmalıdır `True` önbelleğe almayı etkinleştirmek için. Varsayılan, `False` değeridir.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) verileri önbelleğe saniye cinsinden süre. Varsayılan değer 0'dır. ObjectDataSource yalnızca veri varsa önbelleğe alacağı `EnableCaching` olduğu `True` ve `CacheDuration` sıfırdan büyük bir değere ayarlanmış.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) ayarlanabilir `Absolute` veya `Sliding`. Varsa `Absolute`, ObjectDataSource önbelleğe alınan verileri için `CacheDuration` ; saniye, `Sliding`, yalnızca bu için eriştiğini değil sonra verilerin süresini sona erdirir `CacheDuration` saniye. Varsayılan, `Absolute` değeridir.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) ObjectDataSource s önbellek girişlerinin mevcut bir önbellek bağımlılık ile ilişkilendirmek için bu özelliği kullanın. ObjectDataSource s veri girişleri erken önbellekten ilişkili süresinin dolmasını sağlayarak çıkartılabilir `CacheKeyDependency`. SQL önbellek bağımlılık ObjectDataSource s önbellek ile ilişkilendirmek için bu özelliğin en yaygın olarak kullanılır, bu konu şunları gelecekte keşfedeceğiz [SQL önbellek bağımlılıklarını kullanma](using-sql-cache-dependencies-vb.md) öğretici.

Yapılandırma s izin `ProductsDataSource` ObjectDataSource verilerini mutlak ölçek 30 saniye için önbelleğe alma. ObjectDataSource s ayarlamak `EnableCaching` özelliğini `True` ve kendi `CacheDuration` 30 özelliği. Bırakın `CacheExpirationPolicy` özelliği, varsayılan olarak ayarlanmış `Absolute`.


[![ObjectDataSource verilerini 30 saniye için önbelleğe al'ı yapılandırma](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**Şekil 11**: 30 saniye, verileri önbelleğe almak için ObjectDataSource yapılandırın ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-with-the-objectdatasource-vb/_static/image29.png))


Yaptığınız değişiklikleri kaydedin ve bir tarayıcıda bu sayfayı yeniden ziyaret. Sayfa ilk ziyaret edildiğinde metin seçme harekete geçirilen olay başlangıçta veriler önbellekte değil görünür. Ancak disk belleği veya düzenleme veya İptal düğmesini tıklatarak sıralama, geri gönderme düğmesine tıklayarak tetiklenen sonraki Geri göndermeler *değil* yeniden seçme olayı harekete metin. Bunun nedeni, `Selecting` olay yalnızca ObjectDataSource, temel alınan bir nesneden; verisini alır gerektiğinde ateşlenir `Selecting` olay değil yangın verileri veri önbelleğinden alındığından durumunda.

30 saniye sonra verilerin önbellekten çıkarılmasına. Veriler ayrıca önbellekten varsa çıkarılmasına ObjectDataSource s `Insert`, `Update`, veya `Delete` yöntemi çağrılır. Sonuç olarak, 30 saniye geçtiğinden veya güncelleştir düğmesine tıklatıldıktan, sıralama, sayfalama, veya düzenleme veya İptal düğmesini tıklatarak, temel alınan nesne verilerini almak ObjectDataSource neden sonra seçme olay görüntüleme metin tetiklenen zaman `Selecting` olay harekete geçirilir. Döndürülen sonuçlar, verileri önbelleğe geri yerleştirilir.

> [!NOTE]
> Metin seçme harekete geçirilen olay sık sık bakın, önbelleğe alınan verilerle çalışmak ObjectDataSource beklediğiniz bellek kısıtlamaları nedeniyle olabilir. Yeterli bellek yoksa, ObjectDataSource tarafından önbelleğe eklenen veriler attı. ObjectDataSource eklenmemişse t doğru verileri veya tek önbellekler tampon görünüyorsa verileri tutularak, belleği boşaltmak için bazı uygulamaları kapatın ve yeniden deneyin.


Şekil 12 iş akışı önbelleğe alma ObjectDataSource s gösterilmektedir. Seçme olayı tetiklendiğinde metin ekranda görünür, çünkü veriler önbellekte değil ve temel alınan nesneden alınması gerekiyordu. Bu metin eksik olduğunda ancak bunu s verileri önbellekten olmadığı için. Veri önbelleğinden döndürüldüğünde nesnesini çağrı yok ve bu nedenle, hiçbir veritabanı sorgusu orada s.


![ObjectDataSource depoları ve veri önbelleğinden verilerini alır](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**Şekil 12**: ObjectDataSource depolar ve verileri önbellekten verilerini alır


Her bir ASP.NET uygulama paylaşılan tüm sayfaları ve Ziyaretçiler, s örnek kendi veri önbelleği vardır. ObjectDataSource ile verileri önbelleğe depolanan veriler aynı şekilde sayfasını ziyaret edin tüm kullanıcılar arasında paylaşılan anlamına gelir. Bunu doğrulamak için açık `ObjectDataSource.aspx` sayfasını bir tarayıcıda. (Önceki testleri tarafından önbelleğe eklenen veriler artık çıkarıldı varsayarak) ilk sayfasını ziyaret ederek, metin seçme harekete geçirilen olay görüntülenir. İkinci bir tarayıcı örneğinde ve kopyalama açın ve ikinci ilk tarayıcı örnekten URL'yi yapıştırın. İkinci tarayıcı örneğinde olay harekete metin seçme çünkü gösterilmez, s aynı kullanarak önbelleğe alınmış verileri ilk olarak.

Önbelleğe alınan verileri eklerken, ObjectDataSource içeren bir önbellek anahtar değeri kullanır: `CacheDuration` ve `CacheExpirationPolicy` özellik değerlerini; belirtilen ObjectDataSource tarafından kullanılan temel alınan iş nesne türü aracılığıyla [ `TypeName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, bu örnekte); değerini `SelectMethod` özellik adı ve parametrelerin değerlerini `SelectParameters` toplama; ve kendi değerlerini`StartRowIndex`ve `MaximumRows` uygularken kullanılan özellikler [özel disk belleği](../paging-and-sorting/paging-and-sorting-report-data-vb.md).

Önbellek anahtar değeri bu özellikler bir birleşimi olarak hazırlayın, bu değerleri değiştikçe benzersiz önbellek girişi sağlar. Örneğin, önceki öğreticilerde ediyoruz ve baktığı kullanarak `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)`, belirtilen bir kategorideki tüm ürünleri döndürür. Bir kullanıcı, sahip olduğu sayfası ve görünüm İçecekler gelebilir bir `CategoryID` 1. ObjectDataSource bakmadan sonuçlarını önbelleğe varsa `SelectParameters` değerlerini, sayfaya başka bir kullanıcı gelen Çeşniler İçecekler ürünleri çalışırken görüntülemek için önbellekte olan, d Çeşniler yerine önbelleğe alınmış içecek ürünleri görürler. Bu özelliklere göre önbellek anahtarını değiştirerek içeren değerlerini `SelectParameters`, İçecekler ve Çeşniler için ayrı bir önbellek girdisi ObjectDataSource tutar.

## <a name="stale-data-concerns"></a>Eski veri konuları

ObjectDataSource öğelerinden birini, önbellekten otomatik olarak çıkarır, `Insert`, `Update`, veya `Delete` yöntemi çağrılır. Bu, veri sayfası aracılığıyla değiştirildiğinde önbellek girişleri temizleyerek eski veri karşı korunmasına yardımcı olur. Ancak, yine de eski verileri görüntülemek için önbelleğe alma'ı kullanarak bir ObjectDataSource için mümkündür. En basit durumda, doğrudan veritabanı içinde değiştirme veri nedeniyle olabilir. Belki de bir veritabanı yöneticisi, yalnızca bazı veritabanında kayıtlar değiştiren betik çalıştırdınız.

Bu senaryo ayrıca daha zarif bir şekilde unfold. Veri değişikliği yöntemlerinden biri çağrıldığında ObjectDataSource öğelerinden önbellekten çıkarır, ancak önbelleğe alınmış öğeleri kaldırıldı ObjectDataSource s belirli özellik değerleri için birleşimidir (`CacheDuration`, `TypeName`, `SelectMethod`, vb.). Farklı kullanan iki ObjectDataSources varsa `SelectMethods` veya `SelectParameters`, ancak yine de bir ObjectDataSource bir satırı güncelleştirmek ve kendi önbellek girişi, ancak karşılık gelen satır için ikinci ObjectDataSource geçersiz aynı verileri güncelleştirebilirsiniz yine de hizmet alabilecektir gelen önbelleğe alınmış. Bu işlev göstermesi için sayfa oluşturma geçmenizi öneriyoruz. Kullanan önbelleğe alma ve verileri almak üzere yapılandırılan bir ObjectDataSource gelen verileri çeker düzenlenebilir bir GridView görüntüleyen bir sayfa oluşturmak `ProductsBLL` s sınıfı `GetProducts()` yöntemi. Başka bir düzenlenebilir GridView ve bu sayfayı (veya başka bir tane), ancak bu ikinci ObjectDataSource ObjectDataSource sahip bunu kullanmak `GetProductsByCategoryID(categoryID)` yöntemi. İki ObjectDataSources beri `SelectMethod` özellikleri farklı, bunların tümünü her önbelleğe alınmış değerlerine sahip. Verileri geri diğer kılavuzuna (disk belleği, sıralama ve diğerleri tarafından) bağlama sonraki açışınızda bir kılavuz, bir ürün düzenlerseniz, hala eski, önbelleğe alınmış verileri sunmak ve diğer kılavuzdan yapılan değişiklik yansıtmıyor.

Kısacası, eski veri potansiyelini ve veri güncellik önemli olduğu senaryolar için daha kısa expiries kullanın istekliyse yalnızca zaman tabanlı expiries kullanın. Eski veri kabul edilebilir değilse, önbelleğe alma bırakmayı veya SQL önbellek bağımlılıklarını kullanma (veritabanı veri olduğunu varsayarak, önbelleğe alma re). Bir sonraki öğreticide şunları SQL önbellek bağımlılıklarını keşfedeceğiz.

## <a name="summary"></a>Özet

Bu öğreticide, biz ObjectDataSource s önbelleğe alma özellikleri yerleşik incelenir. Yalnızca birkaç özelliklerini ayarlayarak, biz belirtilen döndürülen sonuçları önbelleğe almak için ObjectDataSource bildirebilirsiniz `SelectMethod` ASP.NET veri önbelleğine. `CacheDuration` Ve `CacheExpirationPolicy` özellikleri, öğenin önbelleğe alınma süresi ve mutlak veya kayan zaman aşımı olup olmadığını gösterir. `CacheKeyDependency` Özelliği tüm ObjectDataSource s önbellek girişi var olan bir önbellek bağımlılık ile ilişkilendirir. Zamana bağlı süre sonu ulaşıldığında ve genelde SQL önbellek bağımlılıklarını ile kullanılan önce ObjectDataSource s girişlerin önbellekten çıkarmak için kullanılabilir.

ObjectDataSource yalnızca değerlerinin veri önbelleği için önbelleğe aldığından, biz ObjectDataSource s yerleşik işlevleri programlı olarak çoğaltabilirsiniz. Bunu eklenmemişse t mantıklı sunu katmanında ObjectDataSource bu işlevsellik sunar, ancak önbelleğe alma özellikleri mimarisinin ayrı bir katmana uygulayabilirsiniz olduğundan bunun için. Bunu yapmak için biz ObjectDataSource tarafından kullanılan aynı mantığı tekrarlamanız gerekir. Biz programlı olarak yapı içinde veri önbelleğinden sonraki müşterilerimize öğreticide çalışma hakkında bilgi edineceksiniz.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET önbelleğe alma: Teknikleri ve en iyi uygulamalar](https://msdn.microsoft.com/library/aa478965.aspx)
- [.NET Framework uygulamaları için önbellek Mimarisi Kılavuzu](https://msdn.microsoft.com/library/ee817645.aspx)
- [ASP.NET 2.0 çıkış önbelleğe alma](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Teresa Murphy oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](using-sql-cache-dependencies-cs.md)
> [İleri](caching-data-in-the-architecture-vb.md)
