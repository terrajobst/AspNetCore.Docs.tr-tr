---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: "SQL önbellek bağımlılıkları (VB) kullanarak | Microsoft Docs"
author: rick-anderson
description: "En basit önbelleğe alma stratejisi, belirtilen bir süre sonra süresi dolacak şekilde önbelleğe alınan veriler izin vermektir. Ancak bu basit bir yaklaşım, önbelleğe alınan veriler maintai anlamına gelir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 46521f48d31414ffff2707986d6f869ca2f9bc9a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-sql-cache-dependencies-vb"></a>SQL önbellek bağımlılıkları (VB) kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) veya [PDF indirin](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> En basit önbelleğe alma stratejisi, belirtilen bir süre sonra süresi dolacak şekilde önbelleğe alınan veriler izin vermektir. Ancak bu basit bir yaklaşım önbelleğe alınan veriler çok uzun süre eski veri ve çok yakında süresi geçerli verileri kendi temel alınan veri kaynağı ile ilişki tutar anlamına gelir. Temel alınan verileri SQL veritabanında değiştirildi kadar verileri önbelleğe alınmış kalmayacak şekilde SqlCacheDependency sınıfını kullanmak iyi bir yaklaşımdır. Bu öğretici şunların nasıl yapıldığını gösterir.


## <a name="introduction"></a>Giriş

İçinde önbelleğe alma teknikleri incelenmesi [ObjectDataSource ile veri önbelleğe alma](caching-data-with-the-objectdatasource-vb.md) ve [mimarisinde veri önbelleğe alma](caching-data-in-the-architecture-vb.md) öğreticileri zamana bağlı süre sonu verilerin önbellekten belirtilen sonra çıkarmak için kullanılan süre. Bu yaklaşım, veri eskime durumu karşı önbelleğe alma performans artışı dengelemek için basit bir yoldur. Bir süre sonu seçerek *x* , bir sayfa Geliştirici concedes için yalnızca önbelleğe alma performans avantajlarından faydalanmak için saniye *x* saniye, ancak kendi veri asla en daha eski olmasını kolay tuttuğunuzda *x* saniye. Elbette, statik verileri için *x* web uygulaması için kullanım ömrü içinde incelenmesi gibi genişletilmiş [uygulama başlangıcında veri önbelleğe alma](caching-data-at-application-startup-vb.md) Öğreticisi.

Veritabanı verileri önbelleğe alma, zamana bağlı süre sonu genellikle, kullanım kolaylığı için seçilir, ancak sık yetersiz bir çözümdür. İdeal olarak, temel alınan veri veritabanında değiştirildi kadar veritabanı verilerini önbelleğe alınmış olarak kalır; ancak bundan sonra önbelleği çıkarılacak. Bu yaklaşım, önbelleğe alma performans yararlarını en üst düzeye çıkarır ve eski veri süresini en aza indirir. Ancak, var. Bu avantajlarından faydalanmak için bazı sistem zaman temel alınan veritabanı veri değiştirilmiş ve karşılık gelen öğe önbellekten çıkarır bilir yerinde olmalıdır. ASP.NET 2.0 önce sayfa geliştiricileri bu sistem uygulamak için sorumlu.

ASP.NET 2.0 sağlayan bir [ `SqlCacheDependency` sınıfı](https://msdn.microsoft.com/en-us/library/system.web.caching.sqlcachedependency.aspx) ve böylece ilgili öğeleri önbelleğe alınmış bir değişiklik veritabanında ne zaman gerçekleştiğini belirlemek için gerekli altyapıyı çıkarılacak. Temel alınan veri ne zaman değiştiğini belirlemek için iki tekniği vardır: bildirim ve yoklama. Bildirim ve yoklama arasındaki farklar görüştükten sonra altyapı nasıl kullanıldığını keşfetmek ve yoklama desteklemek gerekli oluşturacağız `SqlCacheDependency` bildirim temelli sınıfında ve program aracılığıyla senaryoları.

## <a name="understanding-notification-and-polling"></a>Anlama bildirim ve yoklama

Verileri bir veritabanındaki bir zaman değişiklik belirlemek için kullanılan iki tekniği vardır: bildirim ve yoklama. Bildirim, sorgu son yürütüldüğü bu yana belirli bir sorgu sonuçlarını değiştirilmiş zaman veritabanı otomatik olarak ASP.NET çalışma zamanı uyarır, bu noktada sorguyla ilişkili önbelleğe alınmış öğeleri çıkarılacak. Yoklama ile veritabanı sunucusu belirli tabloları en son ne zaman güncelleştirildi hakkında bilgi tutar. ASP.NET çalışma zamanı ne tablolar değiştirilmiştir denetlemek için veritabanında düzenli aralıklarla yoklar önbelleğe girilen beri. Verileri değiştirildi bu tablolar çıkarılacak bunların ilişkili önbelleğe alınan öğe var.

Bildirim seçenek yoklama daha az Kurulum gerektirir ve sorgu düzeyinde yerine tablo düzeyi değişiklikleri izleyen daha ayrıntılı olduğu. Ne yazık ki, bildirimler yalnızca Microsoft SQL Server 2005 (yani, Express olmayan sürümleri) tam sürümlerinde kullanılabilir. Ancak, Microsoft SQL Server 7.0 2005 tüm sürümleri için yoklama seçeneği kullanılabilir. Bu öğreticiler SQL Server 2005 Express sürüm kullandığından, biz ayarlama ve yoklama seçeneği kullanılarak odaklanır. Daha fazla SQL Server 2005 s bildirim yetenekleri kaynakları için bu öğreticinin sonunda daha fazla okuma bölümüne başvurun.

Yoklama ile veritabanı adlı bir tablo içerecek şekilde yapılandırılması gerekir `AspNet_SqlCacheTablesForChangeNotification` üç sütun - olan `tableName`, `notificationCreated`, ve `changeId`. Bu tablo bir SQL önbellek bağımlılığı web uygulamasında kullanılan gerekebilir verileri içeren her bir tablo için bir satır içerir. `tableName` Sütun tablosu işlenirken adını belirtir `notificationCreated` satır tabloya eklenen saat ve tarihi belirtir. `changeId` Sütundur türü `int` ve başlangıç değeri 0 olarak sahiptir. Değeri, her değişiklik tablosu ile artırılır.

Ek olarak `AspNet_SqlCacheTablesForChangeNotification` tablosu, veritabanı ayrıca gerekir Tetikleyiciler görünebilir tabloların her biri, SQL önbellek bağımlılık olarak eklenecek. Bu tetikleyicileri her bir satır eklenir, güncellenen veya silinen yürütülen ve s tablosu Artır `changeId` değeri `AspNet_SqlCacheTablesForChangeNotification`.

Geçerli ASP.NET çalışma zamanı izler `changeId` verileri kullanarak önbelleğe alınırken bir tablo için bir `SqlCacheDependency` nesnesi. Veritabanını düzenli olarak denetlenir ve herhangi `SqlCacheDependency` özelliği nesneleri `changeId` değerini veritabanında farklıdır farklı itibaren çıkarılacak `changeId` değeri gösterir olmadığını bir değişiklik tablosu verileri önbelleğe beri.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>1. adım: Keşfetme`aspnet_regsql.exe`komut satırı programı

Yoklama yaklaşımda yukarıda açıklanan altyapı içerecek şekilde Kurulum veritabanı olmalıdır: önceden tanımlanmış bir tablo (`AspNet_SqlCacheTablesForChangeNotification`), saklı yordamları ve Tetikleyicileri her SQL önbellek bağımlılıkları Web kullanılabilir tabloların sayıda uygulama. Bu tablo, saklı yordamları ve Tetikleyicileri komut satırı programı aracılığıyla oluşturulabilir `aspnet_regsql.exe`, içinde bulunan `$WINDOWS$\Microsoft.NET\Framework\version` klasör. Oluşturmak için `AspNet_SqlCacheTablesForChangeNotification` tablo ve ilişkili saklı yordamlar, komut satırından aşağıdaki komutu çalıştırın:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Belirtilen veritabanı oturum açmayı olmalıdır bu komutları çalıştırmak için [ `db_securityadmin` ](https://msdn.microsoft.com/en-us/library/ms188685.aspx) ve [ `db_ddladmin` ](https://msdn.microsoft.com/en-us/library/ms190667.aspx) rolleri. T-SQL veritabanı tarafından gönderilen incelemek için `aspnet_regsql.exe` komut satırı programı, başvurmak [bu blog girdisi](http://scottonwriting.net/sowblog/posts/10709.aspx).


Örneğin, bir Microsoft SQL Server veritabanı altyapısı için yoklama eklemek için adlı `pubs` adlı bir veritabanı sunucusunda `ScottsServer` Windows kimlik doğrulaması kullanarak, uygun dizine gidin ve komut satırından girin:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Veritabanı düzeyi altyapı eklendikten sonra biz SQL önbellek bağımlılıkları kullanılacak olan tabloları Tetikleyiciler eklemeniz gerekir. Kullanmak `aspnet_regsql.exe` komut satırı yeniden programı, ancak tablo adını kullanarak belirtin `-t` geçin ve kullanmak yerine `-ed` anahtar kullanım `-et`, şu şekilde:


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Tetikleyiciler eklemek için `authors` ve `titles` üzerinde tabloları `pubs` üzerinde veritabanı `ScottsServer`, kullanın:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

Bu öğretici için Tetikleyiciler eklemek `Products`, `Categories`, ve `Suppliers` tabloları. Adım 3'te belirli komut satırı sözdizimi inceleyeceğiz.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>2. adım: bir Microsoft SQL Server 2005 Express sürüm veritabanında başvurma`App_Data`

`aspnet_regsql.exe` Komut satırı programı gerekli yoklama altyapı eklemek için veritabanı ve sunucu adı gerektirir. Ancak bulunan Microsoft SQL Server 2005 Express bir veritabanı için veritabanı ve sunucu adı nedir `App_Data` klasörü? Veritabanı ve sunucu adları nelerdir, bulmak zorunda yerine ı ullanıcı buldu en kolay yaklaşım veritabanına ekleme `localhost\SQLExpress` veritabanı örneği ve verileri kullanarak yeniden adlandırma [SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/ms174173.aspx). Makinenize yüklü SQL Server 2005'in tam sürümlerinden biri varsa, daha sonra büyük olasılıkla SQL Server Management Studio zaten. Yalnızca Express sürüm varsa, ücretsiz indirebilirsiniz [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Visual Studio kapatarak başlatın. Ardından, SQL Server Management Studio'yu açın ve bağlanmayı seçebileceğiniz `localhost\SQLExpress` Windows kimlik doğrulaması kullanarak sunucu.


![Localhost\SQLExpress sunucu ekleme](using-sql-cache-dependencies-vb/_static/image1.gif)

**Şekil 1**: ekleme `localhost\SQLExpress` sunucu


Sunucuya bağladıktan sonra Management Studio sunucu gösterebilir ve veritabanları, güvenlik ve benzeri için alt klasörler vardır. Veritabanları klasörünü sağ tıklatın ve Ekle seçeneğini belirleyin. Bu veritabanları Ekle iletişim kutusunu getirir (bkz: Şekil 2) kutusunda. Ekle düğmesini tıklatın ve seçin `NORTHWND.MDF` web uygulama s veritabanı klasöründe `App_Data` klasörü.


[![NORTHWND ekleyin. App_Data klasöründe MDF veritabanından](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Şekil 2**: Attach `NORTHWND.MDF` veritabanını `App_Data` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image2.png))


Bu veritabanı için veritabanları klasörünü ekler. Veritabanı adı veritabanı dosyasının tam yolu olabilir veya tam yol olan $a bir [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Bu uzun veritabanı adı aspnet kullanırken yazmak zorunda kalmamak için\_regsql.exe komut satırı aracı, sağ tıklanarak veritabanında yalnızca bir fazla insan kolay adı veritabanına bağlı yeniden adlandırın ve seçerek yeniden adlandırın. T ve DataTutorials için veritabanını yeniden adlandırıldı.


![Ekli veritabanı fazla insan kolay adla yeniden adlandırın](using-sql-cache-dependencies-vb/_static/image3.gif)

**Şekil 3**: ekli veritabanı fazla insan kolay adla yeniden adlandırın


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>3. adım: Northwind veritabanına yoklama altyapı ekleme

Bağlı olduğunuza `NORTHWND.MDF` veritabanını `App_Data` klasörü, biz re yoklama altyapı eklemek için hazır. Önceden DataTutorials için veritabanı yeniden adlandırılmış varsayılarak aşağıdaki dört komutları çalıştırın:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Dört bu komutları çalıştırdıktan sonra Management Studio'da veritabanı adına sağ tıklayın, görevleri alt menüsüne gidin ve ayırma seçin. Ardından Management Studio'yu kapatın ve Visual Studio'yu yeniden açın.

Visual Studio yeniden açılacak sonra veritabanını Sunucu Gezgini aracılığıyla ayrıntılarına. Yeni Tablo unutmayın (`AspNet_SqlCacheTablesForChangeNotification`), yeni depolanmış yordamları ve Tetikleyicileri `Products`, `Categories`, ve `Suppliers` tabloları.


![Veritabanı artık gerekli yoklama altyapı içerir](using-sql-cache-dependencies-vb/_static/image4.gif)

**Şekil 4**: veritabanı artık gerekli yoklama altyapı içerir


## <a name="step-4-configuring-the-polling-service"></a>4. adım: yoklama hizmetini yapılandırma

Gerekli tabloları, tetikleyiciler ve saklı yordamlar veritabanında oluşturduktan sonra son adım yoluyla yapılır yoklama hizmet yapılandırmaktır `Web.config` kullanın ve yoklama sıklığını veritabanlarına milisaniye cinsinden belirterek. Aşağıdaki biçimlendirme her saniye Northwind veritabanı yoklar.


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

`name` Değeri `<add>` öğesi (NorthwindDB) belirli bir veritabanıyla okunabilir adı ilişkilendirir. SQL önbellek bağımlılıkları ile çalışırken, biz önbelleğe alınmış verileri temel alan tablonun yanı sıra burada tanımlanan veritabanı adı başvurmak gerekir. Nasıl kullanılacağını göreceğiz `SqlCacheDependency` program aracılığıyla SQL önbellek bağımlılıkları ile ilişkilendirmek için sınıf önbelleğe adım 6'daki veri.

Bir SQL önbellek bağımlılığı kurulduktan sonra yoklama sistem tanımlı veritabanınız bağlanacağı `<databases>` öğeleri her `pollTime` milisaniye ve yürütme `AspNet_SqlCachePollingStoredProcedure` saklı yordamı. Eklenen bu saklı yordam - geri adım 3 kullanarak `aspnet_regsql.exe` komut satırı aracı - döndürür `tableName` ve `changeId` her kayıt için değerler `AspNet_SqlCacheTablesForChangeNotification`. Eski SQL önbellek bağımlılıkları önbellekten çıkarılmasına.

`pollTime` Ayarı kolaylığını performans ve veri eskime durumu arasında tanıtır. Küçük bir `pollTime` değeri veritabanına isteklerinin sayısını artırır, ancak daha hızlı bir şekilde çıkarır eski verileri veritabanından. Büyük bir `pollTime` değeri veritabanı isteklerinin sayısını azaltır, ancak arka uç veri değiştiğinde ve ne zaman ilgili önbellek öğeleri çıkarılacak arasındaki gecikme artar. Neyse ki, veritabanı isteği basit bir saklı yordam yalnızca birkaç satırları basit ve basit bir tablodan döndüren bu s yürütüyor. Ancak farklı denemeler `pollTime` arasında ideal bir denge bulmak için değerleri veritabanı erişimi ve veri eskime durumu, uygulamanız için. En küçük `pollTime` izin 500 değerdir.

> [!NOTE]
> Yukarıdaki örnekte, tek bir sunulmaktadır `pollTime` değeri `<sqlCacheDependency>` öğesi, ancak isteğe bağlı olarak belirtebilirsiniz `pollTime` değeri `<add>` öğesi. Belirtilen birden çok veritabanı varsa ve veritabanı başına yoklama sıklığı özelleştirmek istediğiniz kullanışlıdır.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>5. adım: Bildirimli olarak SQL önbellek bağımlılıkları ile çalışma

1-4 arası adımlar, biz gerekli veritabanı altyapısı Kurulum ve yoklama sistem yapılandırmak ne arama. Yerinde bu altyapısıyla biz şimdi öğeleri veri önbelleğine programlı ya da bildirim temelli teknikleri kullanarak bir ilişkili SQL önbellek bağımlılığı ile ekleyebilirsiniz. Bu adımda size bildirimli olarak SQL önbellek bağımlılıkları ile çalışma konusunda inceleyeceğiz. Adım 6'biz programlı yaklaşım göreceğiz.

[ObjectDataSource ile veri önbelleğe alma](caching-data-with-the-objectdatasource-vb.md) öğretici ObjectDataSource bildirim temelli önbelleğe alma özelliklerini incelediniz. Yalnızca ayarlayarak `EnableCaching` özelliğine `True` ve `CacheDuration` özelliği bazı zaman aralığı için ObjectDataSource otomatik olarak önbelleğe temel nesnesinden belirtilen aralık için döndürülen veriler. ObjectDataSource bir veya daha fazla SQL önbellek bağımlılıkları da kullanabilirsiniz.

SQL önbellek bağımlılıkları bildirimli olarak kullanarak göstermek için açık `SqlCacheDependencies.aspx` sayfasındaki `Caching` klasörü ve araç tasarımcıya GridView sürükleyin. GridView s ayarlamak `ID` için `ProductsDeclarative` ve adlı yeni bir ObjectDataSource bağlamak akıllı etiketten seçin `ProductsDataSourceDeclarative`.


[![ProductsDataSourceDeclarative adlı yeni bir ObjectDataSource oluşturma](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Şekil 5**: yeni ObjectDataSource adlandırılmış oluşturma `ProductsDataSourceDeclarative` ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image4.png))


ObjectDataSource kullanmak için yapılandırma `ProductsBLL` sınıfı ve SELECT sekmesi açılır listenin kümesindeki `GetProducts()`. Güncelleştirme sekmesinde seçin `UpdateProduct` aşırı üç giriş parametreleriyle - `productName`, `unitPrice`, ve `productID`. Aşağı açılır listeler (hiçbiri) INSERT ve DELETE sekmeleri ayarlayın.


[![UpdateProduct üç giriş parametreleriyle kullanın](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Şekil 6**: aşırı UpdateProduct üç giriş parametreleriyle kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image6.png))


[![(Hiçbiri) aşağı açılan listede INSERT ve DELETE sekmeler için ayarlayın](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Şekil 7**: ekleme ve silme sekmeler için aşağı açılan liste (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio BoundFields ve CheckBoxFields GridView veri alanların her biri için oluşturur. Tüm alanları Kaldır ancak `ProductName`, `CategoryName`, ve `UnitPrice`ve bu alanlar uygun gördüğünüz şekilde biçimlendirebilirsiniz. GridView s akıllı etiketten etkinleştirmek disk belleği, etkinleştirme sıralama ve düzenlemeyi etkinleştir onay kutularını kontrol edin. Visual Studio ObjectDataSource s ayarlayacak `OldValuesParameterFormatString` özelliğine `original_{0}`. GridView s Düzenle özelliğinin düzgün çalışması sırayla ya da bu özellik tamamen tanımlayıcı sözdizimi veya varsayılan değer olarak, geri kümesi kaldırmak `{0}`.

Son olarak, bir etiket Web denetimi kümesi ve GridView yukarıda ekleyin, `ID` özelliğine `ODSEvents` ve kendi `EnableViewState` özelliğine `False`. Bu değişiklikleri yaptıktan sonra sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir. Not girdiğim ullanıcı yapılan SQL önbellek bağımlılık işlevselliğini göstermek gerekli olmayan GridView alanlara estetik özelleştirmeleri sayısı.


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Ardından, ObjectDataSource s için bir olay işleyicisi oluşturun `Selecting` olay ve bunu, aşağıdaki kodu ekleyin:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Sözcüğünün ObjectDataSource s `Selecting` yalnızca, temel alınan nesnesinden veriyi geri alma olayı ateşlenir. ObjectDataSource kendi önbellekten veri erişirse, bu olay tetiklenir değil.

Şimdi, bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Şekil 8'de gösterildiği gibi Biz bu yana, sayfa, sıralama veya kılavuz sayfa düzenleme her zaman önbelleğe alma henüz uygulamak ve metin, seçme olay harekete, görüntülemelidir.


[![Her zaman GridView, düzenlenemez, disk belleği veya Sorted ObjectDataSource s seçme olay tetikler](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Şekil 8**: ObjectDataSource s `Selecting` olay ateşlenir her GridView disk belleğine alınan süresi, düzenlenen ya da Sorted ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image10.png))


İçinde gördüğümüz gibi [ObjectDataSource ile veri önbelleğe alma](caching-data-with-the-objectdatasource-vb.md) ayarı öğretici, `EnableCaching` özelliğine `True` tarafından belirtilen süre için verileri önbelleğe almak ObjectDataSource neden olan kendi `CacheDuration` özelliği. ObjectDataSource de sahip bir [ `SqlCacheDependency` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), düzeni kullanarak önbelleğe alınmış verileri bir veya daha fazla SQL önbellek bağımlılıkları ekler:


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Burada *databaseName* belirtildiği şekilde veritabanının adıdır `name` özniteliği `<add>` öğesinde `Web.config`, ve *tableName* veritabanı tablosunun adı. Örneğin, verileri süresiz olarak önbelleğe alan bir ObjectDataSource oluşturmak için temel bir SQL önbellek bağımlılığı Northwind s karşı `Products` tablo, ObjectDataSource s ayarlamak `EnableCaching` özelliğine `True` ve kendi `SqlCacheDependency` özelliği NorthwindDB:Products.

> [!NOTE]
> Bir SQL önbellek bağımlılığı kullanabilirsiniz *ve* ayarlayarak bir zamana bağlı süre sonu `EnableCaching` için `True`, `CacheDuration` zaman aralığına ve `SqlCacheDependency` veritabanı ve tablo adlarına gönderilen. ObjectDataSource verilerini zamana bağlı süre sonu erişildiğinde veya temel alınan veritabanı veri değişti olduğunu, hangisi daha önce gerçekleşir yoklama sistem Not çıkarın.


GridView `SqlCacheDependencies.aspx` - iki tablodaki verileri görüntüler `Products` ve `Categories` (s ürün `CategoryName` alanı aracılığıyla alınır bir `JOIN` üzerinde `Categories`). Bu nedenle, iki SQL önbellek bağımlılıkları belirlemenizi istiyoruz: NorthwindDB:Products; NorthwindDB:Categories.


[![ObjectDataSource SQL önbellek bağımlılıkları ürünler ve kategoriler kullanılarak önbelleğe alma destekleyecek şekilde yapılandırma](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Şekil 9**: Destek önbelleğe alma kullanarak SQL önbellek bağımlılıkları için ObjectDataSource yapılandırmasına `Products` ve `Categories` ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image12.png))


Önbelleğe alma işlemini desteklemek için ObjectDataSource yapılandırdıktan sonra bir tarayıcı aracılığıyla sayfa yeniden ziyaret. Yeniden harekete metin seçme olayı ilk sayfasını ziyaret edin görünür, ancak disk belleği, sıralama veya Edit ya da İptal düğmeleri hemen tamamlamalıdır. ObjectDataSource s önbelleğine veriler yüklendikten sonra orada kadar kalır olmasıdır `Products` veya `Categories` tabloları değiştirildiğinde veya veriler GridView güncelleştirilir.

Kılavuz disk belleği ve seçme olay eksikliği belirtmeye sonra metin, yeni bir tarayıcı penceresi açın ve düzenleme, ekleme ve bölüm silme temelleri öğreticide gidin (`~/EditInsertDelete/Basics.aspx`). Adı veya bir ürünün fiyat güncelleştirin. Ardından, gelen verilerin başka bir sayfaya ilk tarayıcı penceresini görüntülemek, ızgarayı sıralamak veya bir satır s Düzenle düğmesini tıklatın. Bu süre, harekete seçme olay veriler temel alınan veritabanı (bkz. Şekil 10) değiştirilmiş olarak görünecektir. Metin görünmüyorsa, birkaç dakika bekleyin ve yeniden deneyin. Değişiklikleri yoklama hizmeti denetleme unutmayın `Products` tablo her `pollTime` milisaniye arasında bir gecikme temel alınan veri güncelleştirildiğinde önbelleğe alınan veriler ne zaman çıkarılacak gelir.


[![Ürünler tablosuna değiştirerek önbelleğe alınmış ürün verilerini çıkarır](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Şekil 10**: önbelleğe alınmış ürün verilerini çıkarır Ürünler tablosuna değiştirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>6. adım: Program aracılığıyla çalışma`SqlCacheDependency`sınıfı

[Mimarisinde veri önbelleğe alma](caching-data-in-the-architecture-vb.md) öğretici Aranan sıkı bir şekilde önbelleğe alma ile ObjectDataSource Kuplaj aksine mimarisinde ayrı bir önbelleğe alma katmana kullanmanın avantajları adresindeki. Bu öğreticide oluşturduğumuz bir `ProductsCL` program aracılığıyla veri önbelleği ile çalışma göstermek için sınıf. SQL önbellek bağımlılıkları önbelleğe alma katmanında kullanmaya kullanmak `SqlCacheDependency` sınıfı.

Yoklama sistemi, bir `SqlCacheDependency` nesne belirli bir veritabanı ve tablo çifti ile ilişkili olmalıdır. Örneğin, aşağıdaki kodu oluşturur bir `SqlCacheDependency` nesne tabanlı Northwind veritabanı s `Products` tablosu:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

İki giriş parametreleri için `SqlCacheDependency` s Oluşturucusu olan veritabanı ve tablo adlarının sırasıyla. ObjectDataSource s gibi `SqlCacheDependency` özelliği, kullanılan veritabanı adı belirtilen değerle aynı olduğundan `name` özniteliği `<add>` öğesinde `Web.config`. Tablo adı veritabanı tablosunun asıl adıdır.

İlişkilendirilecek bir `SqlCacheDependency` veri önbelleğine eklenmiş bir öğesiyle birini kullanın `Insert` bir bağımlılık kabul yöntemi aşırı. Aşağıdaki kod ekler *değeri* belirsiz bir süre için veri önbelleğine, ancak ilişkilendirir ile bir `SqlCacheDependency` üzerinde `Products` tablo. Kısacası, *değeri* bellek kısıtlamaları nedeniyle çıkarılacak kadar ya da yoklama sistem algıladığından önbellekte kalır `Products` tablo önbelleğe ilişkili bu yana değişti.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

Önbelleğe alma katman s `ProductsCL` sınıfı, şu anda verileri önbelleğe `Products` zamana bağlı süre sonu 60 saniye kullanarak tablo. Bunun yerine SQL önbellek bağımlılıkları kullanır, böylece bu sınıf güncelleştirme s olanak tanır. `ProductsCL` s sınıfı `AddCacheItem` verileri önbelleğe eklenmesinden sorumludur, yöntemi şu anda aşağıdaki kod içerir:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Update kullanmak için bu kodu bir `SqlCacheDependency` yerine nesne `MasterCacheKeyArray` önbelleğe bağımlılık:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Bu işlevselliğini sınamak için bir GridView varolan altındaki sayfaya ekleme `ProductsDeclarative` GridView. Bu yeni GridView s ayarlamak `ID` için `ProductsProgrammatic` ve isteğe bağlı olarak, akıllı etiket adlı yeni bir ObjectDataSource bağlama `ProductsDataSourceProgrammatic`. ObjectDataSource kullanmak için yapılandırma `ProductsCL` Seç açılan listeleri ve güncelleştirme sekmelere ayarlama sınıfı `GetProducts` ve `UpdateProduct`sırasıyla.


[![ObjectDataSource ProductsCL sınıfını kullanmak için yapılandırma](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `ProductsCL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image16.png))


[![GetProducts yöntemi seçme sekmesinde s aşağı açılan listeden seçin](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Şekil 12**: seçin `GetProducts` sekmesinde s aşağı açılan liste yönteminden ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image18.png))


[![UpdateProduct yöntemini güncelleştirme sekmesini s aşağı açılan listeden seçin.](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Şekil 13**: UPDATE sekmesi s aşağı açılan liste UpdateProduct yöntemini seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-sql-cache-dependencies-vb/_static/image20.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio BoundFields ve CheckBoxFields GridView veri alanların her biri için oluşturur. Bu sayfaya eklenen ilk GridView ile tüm alanları Kaldır gibi ancak `ProductName`, `CategoryName`, ve `UnitPrice`ve bu alanlar uygun gördüğünüz şekilde biçimlendirebilirsiniz. GridView s akıllı etiketten etkinleştirmek disk belleği, etkinleştirme sıralama ve düzenlemeyi etkinleştir onay kutularını kontrol edin. İle `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio ayarlayacak `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` özelliğine `original_{0}`. GridView s düzenleme özelliği düzgün çalışması için bu özelliği ayarlamak için sırayla geri `{0}` (veya özellik ataması bildirim temelli sözdiziminde tamamen kaldırın).

Bu görevleri tamamladıktan sonra sonuçta elde edilen GridView ve ObjectDataSource bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

SQL test etmek için önbellek bağımlılığı önbelleğe alma katmanında bir kesme noktası kümesinde `ProductCL` s sınıfı `AddCacheItem` yöntemi ve ardından hata ayıklamayı Başlat. İlk kez ziyaret ettiğinizde `SqlCacheDependencies.aspx`, veriler ilk olarak istenen ve önbelleğe yerleştirilmiş olarak kesme noktası isabet. Ardından, GridView başka bir sayfaya taşımak veya sütunlardan biri sıralayabilirsiniz. Bu verileri sorgulayacak GridView neden olur, ancak veri önbelleğindeki itibaren bulunamadı `Products` veritabanı tablosu değişiklik yapılmadı. Veri önbellekte art arda bulunmazsa, yeterli bellek kullanılabilir bilgisayarınızda yüklü olduğundan emin olun ve yeniden deneyin.

GridView, birkaç sayfaları aracılığıyla disk belleği sonra ikinci bir tarayıcı penceresi açın ve düzenleme, ekleme ve bölüm silme temelleri öğreticide gidin (`~/EditInsertDelete/Basics.aspx`). Ürünler tablosundaki bir kaydı güncelleştirmeye ve daha sonra yeni bir sayfa ilk tarayıcı penceresinde görüntülemek veya sıralama üst bilgilerinden biri tıklatın.

Bu senaryoda ikisinden birini görürsünüz: her iki kesme, önbelleğe alınan veriler veritabanında; değişikliği nedeniyle çıkarıldı belirten karşılaşır veya, kesme, yani karşılaşır değil `SqlCacheDependencies.aspx` artık eski veri gösterme. Kesme noktası isabet değil, verileri değiştirilmesinden bu yana yoklama hizmet henüz değil çünkü olasıdır. Değişiklikleri yoklama hizmeti denetleme unutmayın `Products` tablo her `pollTime` milisaniye arasında bir gecikme temel alınan veri güncelleştirildiğinde önbelleğe alınan veriler ne zaman çıkarılacak gelir.

> [!NOTE]
> Bu gecikme bir ürün GridView aracılığıyla düzenlerken görünür olasılığı daha yüksektir `SqlCacheDependencies.aspx`. İçinde [mimarisinde veri önbelleğe alma](caching-data-in-the-architecture-vb.md) öğreticide eklediğimiz `MasterCacheKeyArray` önbelleğe aracılığıyla düzenlenmekte olan veri emin olmak için bağımlılık `ProductsCL` s sınıfı `UpdateProduct` yöntemi önbellekten çıkarılmasına. Ancak, biz Bu önbellek bağımlılığı değiştirirken yerini `AddCacheItem` Bu adımda yöntemi ve bu nedenle `ProductsCL` sınıfı yoklama sistem değişikliği Notlar kadar önbelleğe alınmış verileri gösterecek şekilde devam edecek `Products` tablo. Yeniden etkilenmesine neden nasıl göreceğiz `MasterCacheKeyArray` önbelleğe bağımlılık adım 7'de.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>7. adım: Birden çok bağımlılıkları önbelleğe alınmış bir öğesiyle ilişkilendirme

Sözcüğünün `MasterCacheKeyArray` emin olmak için kullanılan önbellek bağımlılığı *tüm* ürünle ilgili veri içinde ilişkili herhangi bir tek öğesini güncelleştirildiğinde önbellekten çıkarılmasına. Örneğin, `GetProductsByCategoryID(categoryID)` yöntemi önbellekleri `ProductsDataTables` örnekleri her benzersiz *adlı kullanıcı, Categoryıd'si* değeri. Bu nesnelerden birini kaldırıldıysa `MasterCacheKeyArray` önbellek bağımlılığı sağlar diğerleri de kaldırılır. Önbelleğe alınan veriler değiştirildiğinde bu önbellek bağımlılığı diğer önbelleğe alınmış ürün verilerin güncel olması olasılığı bulunmaktadır. Sonuç olarak, bu s biz korumak önemli `MasterCacheKeyArray` SQL önbellek bağımlılıkları kullanırken bağımlılık önbelleğe. Ancak, veri s önbelleğe `Insert` yöntemi yalnızca bir tek bağımlılık nesnesi sağlar.

Ayrıca, SQL önbellek bağımlılıkları ile çalışırken, birden çok veritabanı tabloları bağımlılıklar olarak ilişkilendirmek ihtiyacımız. Örneğin, `ProductsDataTable` önbelleğinde `ProductsCL` sınıfı, her ürün için kategori ve sağlayıcı adları içerir ancak `AddCacheItem` yöntemi yalnızca kullanan bir bağımlılık üzerinde `Products`. Bu durumda, kullanıcı adı bir kategori veya tedarikçi, güncelleştirmeleri olursa önbelleğe alınmış ürün veriler önbellekte kalmasını ve güncel değil. Bu nedenle, önbelleğe alınan ürün veri bağımlı olmak istiyoruz yalnızca `Products` , ancak tablosundaki `Categories` ve `Suppliers` tablolar da.

[ `AggregateCacheDependency` Sınıfı](https://msdn.microsoft.com/en-us/library/system.web.caching.aggregatecachedependency.aspx) birden çok bağımlılıkları önbellek öğesi ile ilişkilendirmek için bir yol sağlar. Başlangıç oluşturarak bir `AggregateCacheDependency` örneği. Ardından, kullanarak bağımlılıkları kümesi eklemek `AggregateCacheDependency` s `Add` yöntemi. Öğenin veri önbelleğine bundan sonra eklerken, geçirin `AggregateCacheDependency` örneği. Zaman *herhangi* , `AggregateCacheDependency` örneği s bağımlılıkları değiştirmek için önbelleğe alınan öğe çıkarılacak.

Güncelleştirilmiş kodu aşağıdaki gösterilir `ProductsCL` s sınıfı `AddCacheItem` yöntemi. Yöntem oluşturur `MasterCacheKeyArray` önbelleğe bağımlılık ile birlikte `SqlCacheDependency` için nesneleri `Products`, `Categories`, ve `Suppliers` tabloları. Bunlar tüm birine birleştirilir `AggregateCacheDependency` adlı nesne `aggregateDependencies`, hangi sonra geçirilir içine `Insert` yöntemi.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Bu yeni kodu sınayın. Şimdi değişikliklerini `Products`, `Categories`, veya `Suppliers` tabloları neden çıkarılacak önbelleğe alınan veriler. Ayrıca, `ProductsCL` s sınıfı `UpdateProduct` GridView aracılığıyla bir ürün düzenlerken olarak adlandırılır, yöntemi çıkarır `MasterCacheKeyArray` önbelleğe önbelleğe alınan neden olan bağımlılık `ProductsDataTable` çıkarılacak ve sonraki yeniden alınması için verileri İstek.

> [!NOTE]
> SQL önbellek bağımlılıkları da kullanılabilir olan [çıktı önbelleği](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Bu işlev tanıtımı için bkz: [kullanarak ASP.NET çıktı önbelleği SQL Server ile](https://msdn.microsoft.com/en-us/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Özet

Veritabanında değiştirilinceye kadar veritabanı verileri önbelleğe alma, veri ideal olarak önbellekte kalır. ASP.NET 2.0 ile SQL önbellek bağımlılıkları oluşturulur ve bildirim temelli ve programlama senaryolarında kullanılır. Bu yaklaşım zorluklar veri değiştirildiğinde keşfetme içinde biridir. Microsoft SQL Server 2005'in tam sürümü sorgu sonucu değişti yükleyen bir uygulama uyarabilir bildirim yetenekleri sağlar. Yoklama sistem, bunun yerine Express sürümü, SQL Server 2005 ve SQL Server'ın daha eski sürümleri için kullanılmalıdır. Neyse ki, gerekli yoklama altyapı ayarlamak oldukça basittir.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Microsoft SQL Server 2005'te sorgu bildirimleri kullanma](https://msdn.microsoft.com/en-us/library/ms175110.aspx)
- [Sorgu bildirimi oluşturma](https://msdn.microsoft.com/en-us/library/ms188669.aspx)
- [ASP.NET ile önbelleğe almayı `SqlCacheDependency` sınıfı](https://msdn.microsoft.com/en-us/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server Kayıt Aracı (`aspnet_regsql.exe`)](https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx)
- [Genel bakış`SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Marko Rangel, Teresa Murphy ve Hilton Giesenow yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](caching-data-at-application-startup-vb.md)
