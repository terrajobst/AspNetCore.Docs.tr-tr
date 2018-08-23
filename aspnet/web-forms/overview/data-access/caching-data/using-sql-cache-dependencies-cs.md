---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: SQL önbellek bağımlılıklarını (C#) kullanma | Microsoft Docs
author: rick-anderson
description: En basit önbelleğe alma stratejisi, belirtilen bir süre sonra süresi dolacak şekilde önbelleğe alınmış verileri izin vermektir. Ancak bu basit yaklaşım önbelleğe alınmış verileri maintai anlamı...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: ddd0ce9e8e0f69da6f9c0f65165e4842d460f0c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752179"
---
<a name="using-sql-cache-dependencies-c"></a>SQL önbellek bağımlılıklarını (C#) kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) veya [PDF olarak indirin](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> En basit önbelleğe alma stratejisi, belirtilen bir süre sonra süresi dolacak şekilde önbelleğe alınmış verileri izin vermektir. Ancak bu basit yaklaşım önbelleğe alınan verilerin çok uzun süre eski veri veya süresi çok yakında geçerli verileri bunun sonucunda, temel alınan veri kaynağı ile hiçbir ilişkisi tutmasını anlamına gelir. Daha iyi bir yaklaşım, temel alınan verileri SQL veritabanı'nda değiştirilmiş kadar verileri önbelleğe alınmış olarak kalır, böylece SqlCacheDependency sınıfı kullanmaktır. Bu öğretici, nasıl gösterir.


## <a name="introduction"></a>Giriş

Önbelleğe alma tekniklerini de incelenirken [ObjectDataSource ile verileri önbelleğe alma](caching-data-with-the-objectdatasource-cs.md) ve [mimaride verileri önbelleğe alma](caching-data-in-the-architecture-cs.md) öğreticiler zamana bağlı süre sonu sonra belirtilen bir önbellekten veri çıkarmak için kullanılan Dönem. Bu yaklaşım, veri eskime karşı önbelleğe alma, gelen performans artışı dengelemek için en basit yoludur. Süre sonu seçerek *x* , sayfa Geliştirici concedes için yalnızca önbelleğe almanın performans avantajlarından faydalanmak için saniye *x* saniye, ancak kendi veri asla en uzun eski olacağını rahat edebilirsiniz ' ın *x* saniye. Elbette, statik veriler için *x* için web uygulamasının kullanım ömrü içinde incelenirken gibi genişletilmiş [uygulama başlangıcında verileri önbelleğe alma](caching-data-at-application-startup-cs.md) öğretici.

Veritabanı verileri önbelleğe alırken zamana bağlı süre sonu, kullanım kolaylığı için genellikle seçilir, ancak sık yetersiz bir çözümdür. İdeal olarak, temel alınan verileri veritabanında değiştirildi kadar veritabanı verilerini önbelleğe alınmış olarak kalır; ancak bundan sonra önbellek çıkarılacak. Bu yaklaşım önbelleğe alma performans yararlarını en üst düzeye çıkarır ve eski veri süresini en aza indirir. Ancak, var. bu avantajlardan faydalanmak için temel alınan veritabanı verileri değiştirilmiş ve karşılık gelen öğe önbellekten çıkarır bilir yerinde bazı sistem olmalıdır. ASP.NET 2.0 önce sayfa geliştiricileri bu sistemi uygulamak için sorumlu.

ASP.NET 2.0 sağlar bir [ `SqlCacheDependency` sınıfı](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) ve böylece karşılık gelen öğeleri önbelleğe alınmış bir değişiklik veritabanında ne zaman gerçekleştiğini belirlemek için gerekli altyapıyı çıkarılacak. Temel alınan verilerin ne zaman değiştirildiğini belirlemek için iki teknik vardır: bildirim ve yoklama. Bildirim yoklama arasındaki farkları görüştükten sonra altyapı ardından nasıl kullanılacağını keşfedin ve yoklama desteklemek için gereken oluşturacağız `SqlCacheDependency` sınıfında bildirim temelli ve programlı olarak senaryoları.

## <a name="understanding-notification-and-polling"></a>Bildirimi anlama ve yoklama

Bir veritabanındaki verilerle bir zaman değişiklik belirlemek için kullanılan iki teknik vardır: bildirim ve yoklama. Bildirimi, belirli bir sorgunun sonuçlarını, en son sorgu çalıştırılmış olsaydı bu yana değiştirilmiş olduğunda otomatik olarak uyarıları ASP.NET çalışma zamanı veritabanı, bu noktada sorgusu ile ilişkili önbelleğe alınmış öğeleri çıkarılan. Yoklama ile veritabanı sunucusunu zaman tabloları en son güncelleştirilen hakkında bilgi tutar. ASP.NET çalışma zamanı veritabanı tabloları nelerin değiştiğini kontrol etmek için düzenli aralıklarla yoklar önbelleğine girildikleri olduğundan. Bu tablolardaki verileri değiştiren çıkarılacak kendi ilişkili önbellek öğeleri var.

Bildirimi seçeneği, yoklama'dan az Kurulum gerektirir ve değişiklikleri tablo düzeyi yerine sorgu düzeyinde izler olduğundan daha ayrıntılı. Ne yazık ki, bildirimler yalnızca Microsoft SQL Server 2005 (yani, Express dışı sürümler) tam sürümlerinde kullanılabilir. Ancak, tüm Microsoft SQL Server 7.0 2005 sürümleri için yoklama seçeneği kullanılabilir. Bu öğretici, SQL Server 2005 Express edition kullandığından, biz ayarlama ve yoklama seçeneğini kullanarak odaklanır. SQL Server 2005 s bildirim özellikleriyle ilgili ek kaynaklar için bu öğreticinin sonunda daha fazla okuma bölümüne bakın.

Adlı bir tablo eklemek için veritabanı yoklama ile yapılandırılmalıdır `AspNet_SqlCacheTablesForChangeNotification` üç sütun - olan `tableName`, `notificationCreated`, ve `changeId`. Bu tablo için bir SQL önbellek bağımlılık web uygulamasında kullanılmak üzere gerekebilecek veri içeren her tabloda bir satır içerir. `tableName` Sütunu sırasında tablonun adını belirtir `notificationCreated` satır tabloya eklenen saat ve tarihi belirtir. `changeId` Sütun türüdür `int` ve bir başlangıç 0 değerine sahiptir. Değeri, her değişikliği tablosuna sahip artırılır.

Ek olarak `AspNet_SqlCacheTablesForChangeNotification` tablo, veritabanı gerekir SQL önbellek bağımlılık olarak görünebilir tabloların her biri Tetikleyicileri içerir. Bu Tetikleyiciler her bir satır eklenen, güncelleştirilen veya silinen yürütülür ve tabloyu s Artır `changeId` değerini `AspNet_SqlCacheTablesForChangeNotification`.

Geçerli ASP.NET çalışma zamanı izleyen `changeId` kullanarak verileri önbelleğe alırken bir tablo için bir `SqlCacheDependency` nesne. Veritabanını düzenli olarak denetlenir ve herhangi `SqlCacheDependency` ayarlanmış nesneleri `changeId` veritabanında değerden farklı farklı beri çıkarılan `changeId` değer olmadığını tabloda değişiklik verilerini önbelleğe ilişkili olduğundan gösterir.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>1. adım: Araştırma`aspnet_regsql.exe`komut satırı programı

Yoklama yaklaşımıyla veritabanı Kurulum yukarıda açıklanan altyapı içerecek biçimde olmalıdır: önceden tanımlanmış bir tablo (`AspNet_SqlCacheTablesForChangeNotification`), saklı yordamları ve Tetikleyicileri SQL önbellek bağımlılıklarını Web kullanılabilir tabloların her biri üzerinde bir dizi uygulama. Bu tablo, saklı yordamlar ve tetikleyicilerle komut satırı programı aracılığıyla oluşturulan `aspnet_regsql.exe`, içinde bulunan `$WINDOWS$\Microsoft.NET\Framework\version` klasör. Oluşturulacak `AspNet_SqlCacheTablesForChangeNotification` tablo ve ilişkili saklı yordamlar, komut satırından aşağıdaki komutu çalıştırın:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Belirtilen veritabanı oturum açma olmalıdır bu komutları yürütmek için [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) ve [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) rolleri. T-SQL veritabanı tarafından gönderilen incelemek için `aspnet_regsql.exe` komut satırı programı, başvurmak [bu blog girişine](http://scottonwriting.net/sowblog/posts/10709.aspx).


Yoklama için altyapıyı bir Microsoft SQL Server veritabanına eklemek için örneğin, adlı `pubs` adlı bir veritabanı sunucusunda `ScottsServer` Windows kimlik doğrulamasını kullanarak uygun dizine gidin ve komut satırından girin:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Veritabanı düzeyinde altyapı eklendikten sonra biz SQL önbellek bağımlılıklarını içinde kullanılan tabloların Tetikleyiciler eklemeniz gerekir. Kullanma `aspnet_regsql.exe` komut satırı yeniden programı, ancak tablo adı kullanarak belirtin `-t` geçiş ve kullanmak yerine `-ed` geçiş kullanım `-et`, şu şekilde:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Eklemek için Tetikleyicileri için `authors` ve `titles` üzerinde tabloları `pubs` veritabanına `ScottsServer`, kullanın:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

Bu öğretici için eklemek için Tetikleyicileri `Products`, `Categories`, ve `Suppliers` tablolar. Adım 3'te belirli komut satırı sözdizimi inceleyeceğiz.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>2. adım: bir Microsoft SQL Server 2005 Express Edition veritabanında başvurma`App_Data`

`aspnet_regsql.exe` Komut satırı programı gerekli yoklama altyapı eklemek için veritabanı ve sunucu adı gerekiyor. Ancak bulunan Microsoft SQL Server 2005 Express veritabanı için veritabanı ve sunucu adı nedir `App_Data` klasör? Veritabanı ve sunucu adları nelerdir, bulmak zorunda yerine miyim ve bulunan en kolay yaklaşım veritabanına iliştirme olduğunu `localhost\SQLExpress` veritabanı örneğine ve verileri kullanarak yeniden adlandırma [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Makinenizde SQL Server 2005'in tam sürümü birine sahipseniz, sonra büyük olasılıkla SQL Server Management Studio zaten. Express edition'ı yalnızca varsa, ücretsiz indirebileceğiniz [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Visual Studio kapatarak başlatın. Ardından, SQL Server Management Studio'yu açın ve bağlanmayı seçebileceğiniz `localhost\SQLExpress` Windows kimlik doğrulaması kullanan bir sunucu.


![Localhost\SQLExpress sunucusu ekleme](using-sql-cache-dependencies-cs/_static/image1.gif)

**Şekil 1**: ekleme `localhost\SQLExpress` sunucusu


Sunucuya bağlandıktan sonra Management Studio sunucunun Göster ve veritabanları, güvenlik ve diğerleri için alt klasörler sahip. Veritabanları klasörünü üzerinde sağ tıklayın ve Ekle seçeneğini belirleyin. Bu veritabanları ekleme iletişim kutusu getirir (bkz: Şekil 2) kutusunda. Ekle düğmesine tıklayıp `NORTHWND.MDF` veritabanı klasöründe, web uygulaması s `App_Data` klasör.


[![NORTHWND ekleyin. App_Data klasöründen MDF veritabanı](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Şekil 2**: ekleme `NORTHWND.MDF` veritabanını `App_Data` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image2.png))


Bu veritabanı için veritabanları klasörünü ekler. Veritabanı adı, veritabanı dosyasının tam yolu olabilir veya tam yol başına bir [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). ASP.NET kullanırken bu uzun bir veritabanı adı yazın zorunda kalmamak için\_regsql.exe komut satırı aracı, veritabanında yalnızca sağ tıklayarak bir insan kullanımı daha kolay ad veritabanına ekli yeniden adlandırma ve seçerek yeniden adlandırın. Ben ve Veritabanım için DataTutorials yeniden adlandırıldı.


![Veritabanına ekli bir insan kullanımı daha kolay adla yeniden adlandırın](using-sql-cache-dependencies-cs/_static/image3.gif)

**Şekil 3**: veritabanına ekli bir insan kullanımı daha kolay adla yeniden adlandırın


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>3. adım: yoklama altyapı Northwind veritabanına ekleme

Ekli olduğunuza göre `NORTHWND.MDF` veritabanını `App_Data` klasörü, biz re yoklama altyapı eklemek için hazır. Önceden DataTutorials için veritabanı yeniden adlandırılmış varsayılarak aşağıdaki dört komutları çalıştırın:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Dört bu komutları çalıştırdıktan sonra Management Studio'da veritabanı adına sağ tıklayın, görevleri menüye gidin ve ayırma seçin. Ardından Management Studio'yu kapatın ve Visual Studio'yu yeniden açın.

Visual Studio kapatılıp sonra Sunucu Gezgini aracılığıyla veritabanına detayına gidin. Yeni Tablo unutmayın (`AspNet_SqlCacheTablesForChangeNotification`), yeni saklı yordamlar ve tetikleyiciler üzerinde `Products`, `Categories`, ve `Suppliers` tablolar.


![Veritabanı artık gerekli yoklama altyapı içerir](using-sql-cache-dependencies-cs/_static/image4.gif)

**Şekil 4**: veritabanı artık gerekli yoklama altyapı içerir


## <a name="step-4-configuring-the-polling-service"></a>4. adım: yoklama hizmetini yapılandırma

Gerekli tabloları, tetikleyiciler ve saklı yordamları veritabanında oluşturduktan sonra son adım aracılığıyla gerçekleştirilir yoklama hizmet yapılandırmaktır `Web.config` milisaniye cinsinden yoklama sıklığı ve veritabanlarını belirterek. Aşağıdaki biçimlendirmede Northwind veritabanına her saniyede yoklar.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

`name` Değerini `<add>` öğesi (NorthwindDB), belirli bir veritabanıyla okunabilir bir ad ilişkilendirir. SQL önbellek bağımlılıklarını ile çalışırken, önbelleğe alınan verileri temel alan tablonun yanı sıra burada tanımlanan veritabanı adı başvurmak gerekir. Nasıl kullanılacağını görüyoruz `SqlCacheDependency` programlama yoluyla SQL önbellek bağımlılıklarını ile ilişkilendirmek için sınıf adım 6'daki verileri önbelleğe alınmış.

SQL önbellek bağımlılık kurulduktan sonra yoklama sistem tanımlı veritabanlarına bağlanacak `<databases>` öğeleri her `pollTime` milisaniye ve yürütme `AspNet_SqlCachePollingStoredProcedure` saklı yordamı. Eklendikten sonra bu saklı yordamı - geri adım 3'ü kullanarak içinde `aspnet_regsql.exe` komut satırı aracı - döndürür `tableName` ve `changeId` her kayıt için değerler `AspNet_SqlCacheTablesForChangeNotification`. Eski SQL önbellek bağımlılıklarını önbellekten çıkarılan.

`pollTime` Ayarı, performans ve veri eskime arasında bir denge tanıtır. Küçük `pollTime` değer veritabanına isteklerinin sayısı artar, ancak daha hızlı bir şekilde çıkarır eski verileri veritabanından. Daha büyük bir `pollTime` değeri veritabanı isteklerinin sayısını azaltır, ancak arka uç veri değiştiğinde ve ne zaman ilgili önbelleğe alınan öğe çıkarılan arasındaki gecikmeyi artırır. Neyse ki, veritabanı isteği, s, yalnızca birkaç satır basit ve basit bir tablo döndüren basit bir saklı yordam yürütüyor. Ancak farklı ile denemeler `pollTime` değerleri arasında bir ideal dengeyi bulmak için veritabanı, uygulama ve verilere erişim eskime durumu. En küçük `pollTime` izin 500 değerdir.

> [!NOTE]
> Yukarıdaki örnek, tek bir sağlar `pollTime` değerini `<sqlCacheDependency>` öğesi, ancak isteğe bağlı olarak belirtebilirsiniz `pollTime` değerini `<add>` öğesi. Belirtilen birden çok veritabanına sahip ve veritabanı başına yoklama sıklığı özelleştirmek istiyorsanız kullanışlıdır.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>5. adım: SQL önbellek bağımlılıklarını ile bildirimli olarak çalışma

1-4 arası adımlar, gerekli veritabanı altyapısını kurma ve yoklama sistemini yapılandırmak nasıl incelemiştik. Bu altyapı yerinde artık öğeleri veri önbelleğine programlı veya bildirim temelli tekniklerini kullanarak bir ilişkili SQL önbellek bağımlılık ile ekleyebiliriz. Bu adımda bildirimli olarak SQL önbellek bağımlılıklarını ile çalışma konusunda inceleyeceğiz. Programlı bir yaklaşım adım 6'da inceleyeceğiz.

[ObjectDataSource ile verileri önbelleğe alma](caching-data-with-the-objectdatasource-cs.md) öğretici ObjectDataSource bildirim temelli önbelleğe alma özelliklerini incelediniz. Yalnızca ayarlayarak `EnableCaching` özelliğini `true` ve `CacheDuration` özelliğin bazı zaman aralığına ObjectDataSource otomatik olarak veriyi önbelleğe alma için belirtilen zaman aralığı, temel alınan nesne döndürdü. ObjectDataSource bir veya daha fazla SQL önbellek bağımlılıklarını de kullanabilirsiniz.

Bildirimli olarak SQL önbellek bağımlılıklarını kullanma göstermek için açık `SqlCacheDependencies.aspx` sayfasını `Caching` klasörü ve GridView tasarımcı araç kutusundan sürükleyin. GridView s ayarlamak `ID` için `ProductsDeclarative` ve adlı yeni bir ObjectDataSource bağlamak, akıllı etiketten seçin `ProductsDataSourceDeclarative`.


[![ProductsDataSourceDeclarative adlı yeni bir ObjectDataSource oluşturma](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Şekil 5**: adlı yeni bir ObjectDataSource oluşturma `ProductsDataSourceDeclarative` ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image4.png))


ObjectDataSource kullanmak için yapılandırma `ProductsBLL` sınıfı ve seçin için sekmesinde açılır listede ayarlayın `GetProducts()`. Güncelleştirme sekmede seçin `UpdateProduct` aşırı üç giriş parametreleriyle - `productName`, `unitPrice`, ve `productID`. (Hiçbiri) açılan listeler, INSERT ve DELETE sekmeleri ayarlayın.


[![Üç giriş parametreleriyle UpdateProduct aşırı yüklemesini kullanın](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Şekil 6**: aşırı UpdateProduct üç giriş parametreleriyle kullanın ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image6.png))


[![(Hiçbiri) açılan liste ekleme ve silme sekmeler için ayarlayın](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Şekil 7**: INSERT ve DELETE sekmeler için açılır listede (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio BoundFields ve CheckBoxFields GridView içinde veri alanların her biri için oluşturur. Tüm alanları kaldırabilirsiniz ancak `ProductName`, `CategoryName`, ve `UnitPrice`ve bu alanlar gördüğünüz şekilde biçimlendirin. GridView s akıllı etiketten sayfalama etkinleştirme, etkinleştirme sıralama ve düzenlemeyi etkinleştir onay kutularını işaretleyin. Visual Studio ObjectDataSource s olarak ayarlanmadıysa `OldValuesParameterFormatString` özelliğini `original_{0}`. GridView s düzenleme özelliği düzgün çalışması için sırada ya da bu özelliği tamamen bildirim temelli söz dizimi veya, varsayılan değerine dönüp yeniden kümesi kaldırmak `{0}`.

Son olarak, GridView ve kümesi üzerinde bir etiket Web denetimi ekleyin, `ID` özelliğini `ODSEvents` ve kendi `EnableViewState` özelliğini `false`. Bu değişiklikleri yaptıktan sonra sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir. Not, ben ve yapılan estetik özelleştirmeleri SQL önbellek bağımlılık işlevselliğini göstermek gerekli olmayan GridView alanların sayısı.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Ardından, ObjectDataSource s için bir olay işleyicisi oluşturun `Selecting` olay ve içinde aşağıdaki kodu ekleyin:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Bu geri çağırma ObjectDataSource s `Selecting` olay, yalnızca veri alt nesnesini alma gerektiğinde ateşlenir. ObjectDataSource kendi önbellekten veri erişirse, bu olay harekete değil.

Artık, bir tarayıcı aracılığıyla bu sayfayı ziyaret edin. Şekil 8 gösterildiği gibi bu yana, sayfa, sıralama veya sayfa kılavuz Düzen her zaman önbelleğe alma henüz uygulamak ve metin, seçme olay harekete geçirildi, görüntüleriz.


[![Her GridView düzenlendi, havuzda zaman veya Sorted ObjectDataSource s seçme olay tetiklenir](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Şekil 8**: ObjectDataSource s `Selecting` olay harekete geçirilir her GridView disk belleğine alınan saati, düzenlenen veya Sorted ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image10.png))


İçinde gördüğümüz gibi [ObjectDataSource ile verileri önbelleğe alma](caching-data-with-the-objectdatasource-cs.md) ayarlama öğreticide `EnableCaching` özelliğini `true` tarafından belirtilen süre için verileri önbelleğe almak ObjectDataSource neden olur, `CacheDuration` özelliği. ObjectDataSource de sahip bir [ `SqlCacheDependency` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), önbelleğe alınmış verileri desenini kullanarak, bir veya daha fazla SQL önbellek bağımlılıklarını ekler:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Burada *databaseName* belirtildiği şekilde veritabanının adıdır `name` özniteliği `<add>` öğesinde `Web.config`, ve *tableName* veritabanı tablosunun adı. Örneğin, verileri süresiz olarak önbelleğe alan bir ObjectDataSource oluşturmak için temel alarak bir SQL önbellek bağımlılık Northwind s karşı `Products` tablo, ObjectDataSource s ayarlayın `EnableCaching` özelliğini `true` ve kendi `SqlCacheDependency` özelliği NorthwindDB:Products.

> [!NOTE]
> SQL önbellek bağımlılık kullanabileceğiniz *ve* ayarlayarak zamana bağlı bir bitiş `EnableCaching` için `true`, `CacheDuration` zaman aralığına ve `SqlCacheDependency` için veritabanı ve tablo adları. ObjectDataSource verilerini zamana bağlı süre sonu ulaşıldığında ya da temel alınan veritabanı veri değişti, hangisi olacağı yoklama sistem Not çıkarırsınız.


GridView içinde `SqlCacheDependencies.aspx` - iki tablodan verileri görüntüleyen `Products` ve `Categories` (s ürün `CategoryName` alanı aracılığıyla alınır bir `JOIN` üzerinde `Categories`). Bu nedenle, iki SQL önbellek bağımlılıklarını belirtmek istiyoruz: NorthwindDB:Products; NorthwindDB:Categories.


[![ObjectDataSource ürünleri ve kategorileri SQL önbellek bağımlılıklarını kullanma önbelleğe alma desteği için yapılandırma](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Şekil 9**: yapılandırmak için destek önbelleğe alma kullanarak SQL önbellek bağımlılıklarını ObjectDataSource `Products` ve `Categories` ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image12.png))


ObjectDataSource önbelleğe alma destekleyecek şekilde yapılandırdıktan sonra bir tarayıcı aracılığıyla sayfada yeniden ziyaret edin. Yeniden harekete metin seçme olay ilk sayfasını ziyaret edin görünür, ancak disk belleği, sıralama veya düzenleme veya İptal düğmesini tıklatarak kaybolması gerekir. Veriler ObjectDataSource s önbelleğe yüklendikten sonra orada kadar kalır olmasıdır `Products` veya `Categories` tabloları değiştirildiğinde veya veriler GridView güncelleştirilir.

Grid sayfalama ve seçme olay eksikliği belirtmeye harekete sonra metin, yeni bir tarayıcı penceresi açın ve düzenleme, ekleme ve bölüm silme temelleri öğreticide gidin (`~/EditInsertDelete/Basics.aspx`). Adı veya bir ürünün fiyatı güncelleştirin. Ardından, ilk tarayıcı penceresine verilerin farklı bir sayfa görünümü, ızgarayı sıralamak veya bir satır s Düzenle düğmesine tıklayın. Bu kez, harekete seçme olay hesaplandıktan temel alınan veritabanı (bkz. Şekil 10) değiştirilmiş olarak görünecektir. Metin görünmüyorsa, birkaç dakika bekleyin ve yeniden deneyin. Değişiklikleri için yoklama hizmeti denetleme unutmayın `Products` tablo her `pollTime` milisaniye cinsinden nedenle temel alınan veriler güncelleştirildiğinde ve önbelleğe alınan verilerin ne zaman çıkarıldığı arasında bir gecikme vardır.


[![Önbelleğe alınan ürün verileri çıkarır Ürünler tablosu değiştirme](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Şekil 10**: önbelleğe alınan ürün verileri çıkarır Ürünler tablosunun değiştirilmesini ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>6. adım: Program aracılığıyla çalışma`SqlCacheDependency`sınıfı

[Mimaride verileri önbelleğe alma](caching-data-in-the-architecture-cs.md) öğretici baktığı en sıkı bir şekilde önbelleğe alma ile ObjectDataSource eşlenmesiyle aksine mimarisinde ayrı bir önbellek katmanı kullanmanın avantajları. Bu öğreticide oluşturduğumuz bir `ProductsCL` program aracılığıyla veri önbelleği ile çalışma göstermek için sınıf. SQL önbellek bağımlılıklarını önbelleğe alma katmanında yararlanmak için kullanmak `SqlCacheDependency` sınıfı.

Yoklama sistemi, bir `SqlCacheDependency` nesne belirli bir veritabanı ve tablo çifti ile ilişkili olmalıdır. Örneğin, aşağıdaki kod oluşturur bir `SqlCacheDependency` nesne tabanlı Northwind veritabanı s `Products` tablosu:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

İki giriş parametreleri için `SqlCacheDependency` s Oluşturucusu olan veritabanı ve tablo adları, sırasıyla. ObjectDataSource s gibi `SqlCacheDependency` özelliği, kullanılan veritabanı adı belirtilen değerle aynı olan `name` özniteliği `<add>` öğesinde `Web.config`. Tablo adı, veritabanı tablosunun gerçek adıdır.

İlişkilendirilecek bir `SqlCacheDependency` verileri önbelleğe eklenen sahip bir öğe birini `Insert` bağımlılık kabul eden bir yöntem aşırı yüklemeleri. Aşağıdaki kodu ekler *değer* belirsiz bir süre için verileri önbelleğe, ancak ilişkilendirir ile bir `SqlCacheDependency` üzerinde `Products` tablo. Kısacası, *değer* bellek kısıtlamaları nedeniyle çıkarıldığı kadar ya da yoklama sistem algılandığı önbellekte kalır `Products` tablo önbelleğe alınmış bu yana değişti.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

Önbellek katmanı s `ProductsCL` sınıfı, şu anda verileri önbelleğe `Products` zamana bağlı süre sonu 60 saniye kullanarak tablo. Bunun yerine SQL önbellek bağımlılıklarını kullanır, böylece bu sınıf güncelleştirme s olanak tanır. `ProductsCL` s sınıfı `AddCacheItem` verileri önbelleğe eklenmesinden sorumludur, yöntem, şu anda aşağıdaki kodu içerir:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Bu kodu kullanmak için güncelleştirme bir `SqlCacheDependency` yerine nesne `MasterCacheKeyArray` önbelleğe bağımlılık:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Bu işlevi test etmek için sayfanın altındaki mevcut GridView ekleyin `ProductsDeclarative` GridView. Bu yeni GridView s ayarlamak `ID` için `ProductsProgrammatic` ve isteğe bağlı olarak akıllı etiketinde adlı yeni bir ObjectDataSource bağlama `ProductsDataSourceProgrammatic`. ObjectDataSource kullanmak için yapılandırma `ProductsCL` Seç açılan listeler ve güncelleştirme sekmeleri ayarlayarak `GetProducts` ve `UpdateProduct`sırasıyla.


[![ObjectDataSource ProductsCL sınıfını kullanmak için yapılandırma](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `ProductsCL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image16.png))


[![GetProducts yöntemi seçme sekmesinde s aşağı açılan listeden seçin.](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Şekil 12**: seçin `GetProducts` için sekmesinde s açılır listede yönteminden ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image18.png))


[![Güncelleştirme sekmesini s aşağı açılan listeden UpdateProduct yöntemi seçin](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Şekil 13**: UPDATE sekmesi s açılır listede UpdateProduct yöntemini seçin ([tam boyutlu görüntüyü görmek için tıklatın](using-sql-cache-dependencies-cs/_static/image20.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio BoundFields ve CheckBoxFields GridView içinde veri alanların her biri için oluşturur. Bu sayfaya eklenen ilk GridView ile tüm alanları kaldırma gibi ancak `ProductName`, `CategoryName`, ve `UnitPrice`ve bu alanlar gördüğünüz şekilde biçimlendirin. GridView s akıllı etiketten sayfalama etkinleştirme, etkinleştirme sıralama ve düzenlemeyi etkinleştir onay kutularını işaretleyin. Olduğu gibi `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio ayarlayacak `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` özelliğini `original_{0}`. GridView s düzenleme özelliği düzgün şekilde çalışması için bu özelliği ayarlamak için sırayla tekrar `{0}` (veya özellik ataması bildirim temelli söz dizimi tamamen kaldırın).

Bu görevleri tamamladıktan sonra elde edilen GridView ve ObjectDataSource bildirim temelli biçimlendirmeyi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

SQL test etmek için önbellek bağımlılık önbelleğe alma katmanında bir kesme noktası kümesinde `ProductCL` s sınıfı `AddCacheItem` yöntemi ve ardından hata ayıklamayı Başlat. İlk kez ziyaret ettiğinizde `SqlCacheDependencies.aspx`, verileri ilk kez istenen ve önbelleğine yerleştirilen kesme noktasına isabet. Ardından, başka bir sayfaya GridView taşıyın veya sütunlardan birine sıralamak. GridView'ın verilerini yeniden sorgulamak bu neden olur, ancak bu yana önbelleğindeki verileri bulundu `Products` veritabanı tablosu yapılmadı. Veriler önbellekte art arda bulunmazsa, yeterli bellek kullanılabilir bilgisayarınızda olduğundan emin olun ve yeniden deneyin.

GridView'ın birkaç sayfaları aracılığıyla disk belleği sonra ikinci bir tarayıcı penceresi açın ve düzenleme, ekleme ve bölüm silme temelleri öğreticide gidin (`~/EditInsertDelete/Basics.aspx`). Products tablosunda bir kaydı güncelleştirme ve ardından yeni bir sayfa ilk tarayıcı penceresinden görüntülemek veya sıralama üstbilgileri birine tıklayın.

Bu senaryoda ikisinden birini görürsünüz: belirten önbelleğe alınan verilerin; veritabanı değişiklik nedeniyle çıkarıldı ya da bir kesme noktasına ulaşılır veya, kesme noktasına, güncelleştirmeyeceği isabet ettirilmeyecek `SqlCacheDependencies.aspx` artık eski veri gösteriyor. Kesme noktasına isabet değil, verileri değiştirilmesinden bu yana yoklama hizmet henüz değil çünkü olasıdır. Değişiklikleri için yoklama hizmeti denetleme unutmayın `Products` tablo her `pollTime` milisaniye cinsinden nedenle temel alınan veriler güncelleştirildiğinde ve önbelleğe alınan verilerin ne zaman çıkarıldığı arasında bir gecikme vardır.

> [!NOTE]
> Bu gecikme GridView aracılığıyla ürünlerinden biri düzenlerken görünür olma olasılığı daha yüksektir `SqlCacheDependencies.aspx`. İçinde [mimaride verileri önbelleğe alma](caching-data-in-the-architecture-cs.md) öğreticide eklediğimiz `MasterCacheKeyArray` önbelleğe aracılığıyla düzenlenmekte olan veri emin olmak için bağımlılık `ProductsCL` s sınıfı `UpdateProduct` yöntemi, önbellekten çıkarılmasına. Ancak Biz bu önbellek bağımlılık değiştirirken yerine `AddCacheItem` Bu adımda yöntemi ve bu nedenle `ProductsCL` sınıfı yoklama sistem değişikliğini notları kadar önbelleğe alınmış verileri göstermek devam edecek `Products` tablo. Güçlendirebilir nasıl görüyoruz `MasterCacheKeyArray` önbelleğe adım 7'de bağımlılık.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>7. adım: Çoklu bağımlılıklar önbelleğe alınmış bir öğesiyle ilişkilendirme

Sözcüğünün `MasterCacheKeyArray` emin olmak için kullanılan önbellek bağımlılık *tüm* ürün ile ilgili veriler içinde ilişkili herhangi bir tek öğe güncelleştirildiğinde önbellekten çıkarılacak. Örneğin, `GetProductsByCategoryID(categoryID)` yöntemi önbellekler `ProductsDataTables` örnekleri her benzersiz *CategoryID* değeri. Bu nesnelerden birini kaldırıldıysa `MasterCacheKeyArray` önbellek bağımlılık sağlar diğerleri de kaldırılır. Önbelleğe alınan verilerin değiştirildiğinde bu önbellek bağımlılığı diğer önbelleğe alınan ürün verileri güncel olması olasılığı bulunmaktadır. Sonuç olarak, bu önemli olduğunu saklarız s `MasterCacheKeyArray` SQL önbellek bağımlılıklarını kullanma, bağımlılık önbellek. Ancak, s veriyi önbelleğe `Insert` yöntemi yalnızca bir tek bağımlılık nesnesi sağlar.

Ayrıca, SQL önbellek bağımlılıklarını ile çalışırken, bağımlılık olarak birden çok veritabanı tabloları ilişkilendirilecek ihtiyacımız. Örneğin, `ProductsDataTable` önbelleğe alınmış olarak `ProductsCL` sınıfı, her ürün için kategori ve tedarikçi adları içerir ancak `AddCacheItem` yöntemi yalnızca kullanan bir bağımlılık üzerinde `Products`. Bu durumda, kullanıcı adı bir kategori veya tedarikçi, güncelleştiriyorsa önbelleğe alınan ürün veriler önbellekte kalmasını ve güncel olmayabilir. Bu nedenle, önbelleğe alınan ürün veri bağımlı yapmak istiyoruz yalnızca `Products` , ancak tablosundaki `Categories` ve `Suppliers` tablolar da.

[ `AggregateCacheDependency` Sınıfı](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) çoklu bağımlılıklar bir önbellek öğesi ile ilişkilendirmek için bir yol sağlar. Oluşturarak başlayın bir `AggregateCacheDependency` örneği. ' I kullanarak bağımlılıkları kümesini ekleyeceğimize `AggregateCacheDependency` s `Add` yöntemi. Öğenin veri önbelleğine bundan sonra eklerken, geçirin `AggregateCacheDependency` örneği. Zaman *herhangi* , `AggregateCacheDependency` örneği s bağımlılıkları değiştirmek için önbelleğe alınan öğeyi çıkarılacak.

Aşağıdaki güncelleştirilmiş kodu gösterilir `ProductsCL` s sınıfı `AddCacheItem` yöntemi. Yöntemi oluşturur `MasterCacheKeyArray` önbelleğe bağımlılık ile birlikte `SqlCacheDependency` için nesneleri `Products`, `Categories`, ve `Suppliers` tablolar. Bu tüm birine birleştirilir `AggregateCacheDependency` adlı nesne `aggregateDependencies`, hangi sonra geçirilen içine `Insert` yöntemi.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Bu yeni kodu test edin. Artık değişikliklerini `Products`, `Categories`, veya `Suppliers` tabloları önbelleğe alınan verilerin çıkarılmasına neden. Ayrıca, `ProductsCL` s sınıfı `UpdateProduct` GridView aracılığıyla bir ürün düzenlerken çağrılan yöntem çıkarır `MasterCacheKeyArray` önbelleğe önbelleğe alınan neden bağımlılık `ProductsDataTable` çıkarılacak ve sonraki yeniden alınacak verileri İstek.

> [!NOTE]
> SQL önbellek bağımlılıklarını de kullanılabilir olan [çıktı önbelleği](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Bu işlev bir gösterimi için bkz: [kullanarak ASP.NET çıktı önbelleğini SQL Server ile](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Özet

Veritabanında değiştirilene kadar veritabanı verileri önbelleğe alma, verileri ideal olarak önbellekte kalır. ASP.NET 2.0 ile SQL önbellek bağımlılıklarını oluşturulur ve bildirim temelli ve programlı senaryolarda kullanılır. Bu yaklaşımın zorluklarını içindeki veri değiştirildiğinde keşfetme biridir. Microsoft SQL Server 2005'in tam sürümü sorgu sonucu değiştiğinde, uygulamanın uyarabilir bildirim yetenekleri sağlar. Express sürümü, SQL Server 2005 ve SQL Server'ın daha eski sürümleri için bir yoklama sistemde bunun yerine kullanılmalıdır. Neyse ki, gerekli yoklama altyapı Kurulumu oldukça basittir.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Microsoft SQL Server 2005'te sorgu bildirimleri kullanma](https://msdn.microsoft.com/library/ms175110.aspx)
- [Sorgu bildirimi oluşturma](https://msdn.microsoft.com/library/ms188669.aspx)
- [ASP.NET ile önbelleğe almayı `SqlCacheDependency` sınıfı](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server Kayıt Aracı (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Genel bakış `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Marko Rangel Teresa Murphy ve Hilton Giesenow yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](caching-data-at-application-startup-cs.md)
> [İleri](caching-data-with-the-objectdatasource-vb.md)
