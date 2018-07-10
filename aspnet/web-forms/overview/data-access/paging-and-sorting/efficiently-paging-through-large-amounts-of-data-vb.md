---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: Büyük miktarlarda veri (VB) etkili bir şekilde sayfalama | Microsoft Docs
author: rick-anderson
description: Veri sunu denetimin varsayılan disk belleği seçeneği, büyük miktarlarda veriler, temel alınan veri kaynağı denetimi retriev ile çalışırken uygun değil...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b00e18287bdb791a353b7ebd1bbb6cc0ab586b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805512"
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Büyük miktarlarda veri (VB) etkili bir şekilde sayfalama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) veya [PDF olarak indirin](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> Yalnızca veri kümesini görüntülenmesine karşın, temel alınan veri kaynağı denetimi tüm kayıtları alır. bir veri sunu denetimin varsayılan disk belleği seçeneği büyük miktarda veri ile çalışırken uygun aynıdır. Böyle durumlarda, size özel etkinleştirmeniz gerekir sayfalama.


## <a name="introduction"></a>Giriş

Önceki öğreticide ele aldığımız gibi disk belleği iki yoldan biriyle uygulanabilir:

- **Varsayılan disk belleği** sayfalama etkinleştir seçeneğini işaretleyerek uygulanabilir verileri Web denetimi s akıllı etiketi; ancak, verilerin bir sayfa görüntüleme her ObjectDataSource alır *tüm* kayıtları, hatta yalnızca bir alt kümesini sayfada görüntülenir ancak
- **Özel disk belleği** varsayılan performansını artırır, belirli kullanıcı tarafından; istenen veri sayfasının görüntülenmesi gereken veritabanından yalnızca kayıtları alarak sayfalama ancak özel disk belleği uygulamak için biraz daha fazla çaba gerektirir varsayılan disk belleği daha

Uygulama yalnızca onay bir onay kutusu ve yeniden kolaylığı nedeniyle bitti! varsayılan disk belleği cazip bir seçenektir. Tüm kayıtlar alınırken, ad ve yaklaşım yine de bir implausible seçim yeterince büyük miktarlarda veri veya siteler için aracılığıyla ile çok sayıda eşzamanlı kullanıcıyı sayfalama kolaylaştırır. Böyle durumlarda, biz bir hızlı sistem sağlamak için disk belleği özel etkinleştirmeniz gerekir.

Özel disk belleği zorluk, hassas verilerin belirli bir sayfa için gerekli kayıt kümesini döndüren bir sorgu yazma çağrılabilmesidir. Neyse ki, Microsoft SQL Server 2005 yeni bir anahtar sözcük sıralama sonuçları almak için verimli bir şekilde kapsanır kayıtların alabileceğiniz bir sorgu yazma sağlıyor sağlar. Bu öğreticide bir GridView denetiminde özel disk belleği uygulamak için bu yeni SQL Server 2005'anahtar sözcüğünü kullanmayı göreceğiz. Özel disk belleği için kullanıcı arabirimi aynı sonraki kullanarak bir sayfadan Adımlama, varsayılan sayfalama iken özel disk belleği birkaç kat varsayılan disk belleği daha hızlı olabilir.

> [!NOTE]
> Özel disk belleği tarafından sergilenen tam performans kazancı aracılığıyla havuzda kayıtlarının ve veritabanı sunucusuna yerleştirilen yük toplam sayısına bağlı olarak değişir. Bu öğreticinin sonunda özel disk belleği aracılığıyla elde edilen performans avantajlarını başlanacağını gösteren kaba bazı ölçümlere göz atacağız.


## <a name="step-1-understanding-the-custom-paging-process"></a>1. adım: özel disk belleği işlemini anlama

Verileri sayfalama, istenen veri sayfasını ve sayfa başına görüntülenen kayıt sayısını bir sayfasında görüntülenen kesin kayıtlar bağlıdır. Örneğin, 81 ürünler sayfa sayfa başına 10 ürünleri görüntüleme istedik olduğunu düşünün. İlk sayfa görüntülerken, d 1-10 arasındaki ürünleri istiyoruz; ikinci sayfasında görüntülerken, biz d ürünleri 11 ile 20 ve benzeri.

Kayıtları alınacak gerekir ve disk belleği arabirimi nasıl işleneceğini dikte üç değişkenleri şunlardır:

- **Satır dizini Başlat** ilk sayfasında görüntülenecek veri satırı dizinini; sayfa başına görüntülenecek kayıt sayfa dizini çarparak ve eklenirken bir hesaplanan dizin okumaları oluşabilir. Örneğin, kayıtları arasında 10 birer ilk sayfa (sayfa dizini: 0) için disk belleği, başlangıç satır dizini 0'dır \* 10 + 1 veya 1; ikinci sayfa (sayfa dizini 1 olur) için başlangıç satır dizini 1'dir \* 10 + 1 , veya 11.
- **En fazla satır** en fazla sayfa başına görüntülenecek kayıt sayısı. İçin en son döndürülen sayfa boyutundan daha az sayıda kayıt sayfasını olabileceğinden bu değişkeni, maksimum satır adlandırılır. Örneğin, sayfa başına 81 ürünleri 10 kayıtları arasında disk belleği, dokuzuncu ve son sayfa yalnızca bir kaydına sahip. Sayfa yok ancak, en fazla satır değerden daha fazla kaydı gösterir.
- **Toplam kayıt sayısı** kayıtları aracılığıyla havuzda toplam sayısı. Bu değişken birincile t için belirli bir sayfayı almak için hangi kayıtların belirlemek için gereken, ancak disk belleği arabirimi gerektirir. Örneğin, havuzda aracılığıyla 81 ürün varsa, disk belleği arabirimi dokuz sayfa numaralarını sayfalama Kullanıcı Arabiriminde görüntülenecek bilir.

Varsayılan disk belleği ile en fazla satır yalnızca sayfa boyutunu bilgileriyse başlangıç satır dizini sayfa dizini ve sayfa boyutu artı bir ürün olarak hesaplanır. Varsayılan disk belleği tüm kayıtlarını alır. bu yana herhangi bir sayfanın verileri, her satır için dizin oluşturma sırasında veritabanı böylece başlangıç satır dizini satır basit bir görev için hareketli hale bilinir. Ayrıca, toplam kayıt sayısı gibi kullanıma hazır, s sadece DataTable (veya nesnenin veritabanı sonuçlarını tutmak için kullanılan) kayıt sayısı.

Satır dizini başlatmak ve en fazla satır değişkenleri göz önünde bulundurulduğunda, özel bir disk belleği uygulama yalnızca Başlat satır dizini ve kayıt maksimum satır sayısı kadar daha sonra başlayan kayıtları kesin kümesini döndürmesi gerekir. Özel disk belleği iki aşama sağlar:

- Biz verimli bir şekilde her satırın aracılığıyla ki belirtilen başlangıç satırı dizindeki kayıt döndürdüğünü başlayabilmesi disk belleği tüm verileri bir satır dizini ilişkilendiremezsiniz.
- Kayıt aracılığıyla havuzda toplam sayısını sağlamak için ihtiyacımız

Bu iki sorunlara yanıt için gereken SQL komut dosyasını sonraki iki adımda inceleyeceğiz. SQL betiğini yanı sıra, biz de yöntemleri BLL ve DAL içinde uygulama gerekir.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>2. adım: kayıt aracılığıyla havuzda toplam sayısını döndüren

Kayıt sayfasının görüntülenmesini kesin kümesini almak nasıl inceleyeceğiz önce ilk kayıt aracılığıyla havuzda toplam sayısını döndürmek nasıl bakmak s olanak tanır. Bu bilgiler, disk belleği kullanıcı arabirimi düzgün bir şekilde yapılandırmak için gereklidir. Belirli bir SQL sorgusu tarafından döndürülen kayıt toplam sayısını kullanılarak elde edilebilir [ `COUNT` toplama işlevi](https://msdn.microsoft.com/library/ms175997.aspx). Örneğin, kayıtlarının toplam sayısını belirlemek için `Products` tablo, aşağıdaki sorguyu kullanabiliriz:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Bu bilgiler döndüren bizim DAL için bir yöntem ekleyin s olanak tanır. Özellikle, adlı bir DAL yöntem oluşturacağız `TotalNumberOfProducts()` , yürütür `SELECT` yukarıda gösterilen deyimi.

Başlangıç açarak `Northwind.xsd` türü belirtilmiş veri kümesi dosyasında `App_Code/DAL` klasör. Ardından, sağ `ProductsTableAdapter` Tasarımcısı ve Sorgu Ekle öğesini seçin. Şöyle önceki öğreticilerde, görülen ve bu DAL için yeni bir yöntem eklemek bize izin verir, belirli bir SQL deyimi veya saklı yordam çağrıldığında yürütülür. Bizim TableAdapter yöntemleri gibi önceki öğreticilerde, bu biri için bir geçici SQL deyimini kullanmayı tercih.


![Geçici SQL deyimi kullan](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Şekil 1**: geçici SQL deyimi kullan


Sonraki ekranda biz ne tür bir sorgu oluşturmak için belirtebilirsiniz. Bu sorgu, skaler tek bir değer kayıtlarının toplam sayısını döndürür. bu yana `Products` Tablo Seç `SELECT` singe değer seçeneği döndürür.


![Tek bir değer döndüren bir SELECT deyimi kullanılacak sorgu yapılandırma](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Şekil 2**: tek bir değer döndüren bir SELECT deyimi kullanılacak sorgu yapılandırma


Kullanılacak sorgu türünü belirten sonra size sonraki sorgu belirtmeniz gerekir.


![SELECT COUNT(*) ürünleri SORGUDAN kullanın](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Şekil 3**: SELECT sayısını kullan (\*) FROM ürünleri sorgu


Son olarak, yöntemin adını belirtin. Yukarıda sözü edilen, let s olarak kullanmak `TotalNumberOfProducts`.


![DAL yöntemi TotalNumberOfProducts adı](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Şekil 4**: DAL yöntemi TotalNumberOfProducts adı


Son'a tıkladıktan sonra sihirbaz ekleyecektir `TotalNumberOfProducts` DAL için yöntemi. SQL sorgusu sonuç olması durumunda boş değer atanabilir türler, DAL skaler döndürme yöntemleri dönüş `NULL`. Bizim `COUNT` sorgu, ancak her zaman döndürecektir olmayan bir`NULL` değeri; ne olursa olsun, DAL yöntemi boş değer atanabilir bir tamsayı döndürür.

DAL yöntemin yanı sıra, ayrıca BLL bir yönteme ihtiyacımız var. Açık `ProductsBLL` ekleyin ve sınıf dosyası bir `TotalNumberOfProducts` DAL s yalnızca çağıran yöntemi `TotalNumberOfProducts` yöntemi:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s `TotalNumberOfProducts` yöntem boş değer atanabilir bir tamsayı döndürür; ancak biz oluşturulan ve `ProductsBLL` s sınıfı `TotalNumberOfProducts` olan standart bir tamsayı döndürmesini sağlayacak bir yöntem. Bu nedenle, sağlamak ihtiyacımız `ProductsBLL` s sınıfı `TotalNumberOfProducts` yöntemi dönüş DAL s tarafından döndürülen boş değer atanabilir bir tamsayı değer kısmı `TotalNumberOfProducts` yöntemi. Çağrı `GetValueOrDefault()` varsa; boş değer atanabilir bir tamsayı ise, boş değer atanabilir bir tamsayı değerini döndürür `null`, ancak varsayılan tamsayı değeri, 0 döndürür.

## <a name="step-3-returning-the-precise-subset-of-records"></a>3. adım: hassas kayıt alt kümesi döndüren

Bizim sonraki görev yöntemleri başlangıç satır dizini kabul BLL ve DAL oluşturma ve en fazla satır değişkenlerini daha önce açıklanan ve uygun kayıtları döndürür. Bunu yapmadan önce let s ilk bakış gereken SQL komut. Bize karşılıklı challenge biz verimli bir şekilde aracılığıyla yalnızca başlangıç satır dizini (ve kayıtları en fazla kayıt sayısı kadar) başlangıç kayıtları geri dönebilmek disk belleği tüm sonuçları her satır için bir dizin atayamazsınız olmasıdır.

Zaten varsa bir sütun veritabanı tablosundaki bir satır dizini hizmet veren bu zor değildir. Biz, ilk bakışta düşünebilirsiniz `Products` tablo s `ProductID` alan yeterli, ilk ürün sahibidir `ProductID` 1, 2, ikinci ve benzeri. Ancak, bir ürün siliniyor, bu yaklaşım nullifying dizisinin boşluk bırakır.

Böylece kesin alt alınacak kayıt etkinleştirme satır dizini aracılığıyla, sayfa için verileri etkili bir şekilde ilişkilendirmek için kullanılan iki genel teknikler vardır:

- **SQL Server 2005 s kullanarak `ROW_NUMBER()` anahtar sözcüğü** yeni SQL Server 2005 `ROW_NUMBER()` anahtar sözcüğü bir derecelendirme bazı sıralarına göre döndürülen her kaydı ilişkilendirir. Bu sıralama her satır için bir satır dizini kullanılabilir.
- **Tablo değişkeni kullanarak ve `SET ROWCOUNT`**  SQL Server s [ `SET ROWCOUNT` deyimi](https://msdn.microsoft.com/library/ms188774.aspx) toplamda kaç kaydın bir sorgunun sonlandırmadan önce; işlenmesi belirtmek için kullanılabilir [Tablo değişkenleri](http://www.sqlteam.com/item.asp?ItemID=9454) sekmeli veri için akin içerebileceği T-SQL yerel değişkenleri [geçici tablolar](http://www.sqlteam.com/item.asp?ItemID=2029). Bu yaklaşım eşit çalışır hem Microsoft SQL Server 2005 ve SQL Server 2000 ile (ise `ROW_NUMBER()` yaklaşım yalnızca SQL Server 2005'te çalışır).  
  
  Buradaki sahip bir tablo değişkeni oluşturmaktır bir `IDENTITY` sütun ve tabloda birincil anahtarlarını verisini disk belleğine alınan aracılığıyla sütun. Ardından, verileri disk belleğine alınan aracılığıyla tablosunun yazılan, böylece bir sıralı satır dizini ilişkilendirme tablo değişkenine (aracılığıyla `IDENTITY` sütun) tablosundaki her kayıt için. Tablo değişkeni doldurulmuş sonra bir `SELECT` deyimi tablo değişkeni üzerinde temel alınan tabloyla katılmış belirli kayıtları süzer çekmek için yürütülebilir. `SET ROWCOUNT` Deyimi, akıllı bir şekilde tablo değişkenine dökümünün gereken kayıt sayısını sınırlamak için kullanılır.  
  
  Bu yaklaşım s verimlilik istenen, sayfa sayısına göre belirlenmektedir olarak `SET ROWCOUNT` değeri yanı sıra en fazla satır satır dizini başlangıç değeri atanır. İlk gibi düşük numaralı sayfaları aracılığıyla veri birkaç sayfa sayfa bu çok verimli bir yaklaşımdır. Ancak, bir sayfa sonlarında alınırken varsayılan sayfalama benzer performans sergiler.

Bu öğreticide, özel disk belleği kullanarak uygulayan `ROW_NUMBER()` anahtar sözcüğü. Tablo değişkeni'ni kullanma hakkında daha fazla bilgi ve `SET ROWCOUNT` teknik bkz [A daha fazla verimli yöntemi, sayfalama aracılığıyla büyük sonuç kümeleri için](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

`ROW_NUMBER()` Anahtar sözcüğü bir belirli aşağıdaki sözdizimini kullanarak sıralama üzerinden döndürülen her bir kayıt sıralaması ilişkili:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` belirtilen sıralama bakımından her kayıt için boyut belirten bir sayısal değeri döndürür. Örneğin, en iyi sıralı, her ürün için boyut görmek için en az pahalı aşağıdaki sorguyu kullanabiliriz:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Şekil 5, bu sorgu Visual Studio sorgu penceresinde çalıştırdığınızda s sonuçları gösterir. Ürünler, her satır için bir fiyat derece birlikte fiyata göre sıralanır unutmayın.


![Fiyat boyut için dahil edilen her döndürülen kayıt](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Şekil 5**: Fiyat derece için dahil her döndürülen kayıt


> [!NOTE]
> `ROW_NUMBER()` SQL Server 2005'te birçok yeni sıralama işlevleri yalnızca biri kullanılabilir. Daha kapsamlı bir irdelemesi `ROW_NUMBER()`, diğer derecelendirme işlevleri yanı sıra okuma [Microsoft SQL Server 2005 ile sıralanmış sonuçları döndüren](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Sonuçları tarafından belirtilen sıralama sırasında `ORDER BY` sütununda `OVER` yan tümcesi (`UnitPrice`, yukarıdaki örnekte), SQL Server sonuçlarını sıralama gerekir. Bu sonuçları sıralanır, sütunların üzerinden kümelenmiş bir dizin ise hızlı bir işlemdir veya bir kapsayıcı olup olmadığını dizin, ancak Aksi halde daha yüksek maliyetli olabilir. Yeterli büyüklükte sorguların performansını artırmak için kümelenmemiş bir dizin olarak sonuçları göre sıralanmış sütun ekleme göz önünde bulundurun. Bkz: [sıralama işlevlerini ve SQL Server 2005'te performans](http://www.sql-server-performance.com/ak_ranking_functions.asp) performans konuları daha ayrıntılı bilgi için.

Tarafından döndürülen sıralama bilgileri `ROW_NUMBER()` doğrudan kullanılamaz `WHERE` yan tümcesi. Ancak, türetilmiş tablo döndürmek için kullanılabilir `ROW_NUMBER()` ardından görünebilen sonuç `WHERE` yan tümcesi. Örneğin, aşağıdaki sorguyu ProductName ve UnitPrice sütunları ile birlikte döndürülecek türetilmiş tablo kullanan `ROW_NUMBER()` sonuç ve kullandığı bir `WHERE` yan tümcesi bu ürünlerin yalnızca, fiyat derece döndürülecek olan 11 ile 20 arasında:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Bu kavramı biraz daha fazla genişletme, belirli bir sayfaya istediğiniz satır dizini başlatmak ve en fazla satır değerleri verilen veri almak için bu yaklaşım kullanabilir:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Bu öğreticide daha sonra göreceğiz gibi *`StartRowIndex`* tarafından sağlanan ObjectDataSource, sıfırdan başladığını ise dizinlenir `ROW_NUMBER()` SQL Server 2005 tarafından döndürülen değer 1'den başlayarak tarihine. Bu nedenle, `WHERE` yan tümcesi kayıtları döndürür burada `PriceRank` değerinden kesinlikle büyük *`StartRowIndex`* ve daha az veya buna eşit *`StartRowIndex`*  +  *`MaximumRows`*.


Artık görüyoruz ve nasıl ele alınan `ROW_NUMBER()` olabilir belirli bir sayfada satır dizini başlatmak ve en fazla satır değerlerine veri almak üzere kullanılan, artık BLL ve DAL yöntemler olarak bu mantığı uygulamak ihtiyacımız.

Bu sorgu sıralama karar gerekir oluştururken kullandığı sonuçları derece verilecek; ürün adlarına alfabetik sıralama s olanak tanır. Bu, bu öğreticide özel disk belleği uygulamasıyla biz de sıralanabilir daha özel bir disk belleğine alınan rapor oluşturmak mümkün olmayacaktır, anlamına gelir. Sonraki öğreticide, yine de bu işlevselliğin nasıl sağlanabilir göreceğiz.

Önceki bölümde geçici SQL deyimi DAL yöntemi oluşturduk. Ne yazık ki, T-SQL ayrıştırıcının TableAdapter Sihirbazı eklenmemişse t gibi tarafından kullanılan Visual Studio'da `OVER` tarafından kullanılan söz dizimi `ROW_NUMBER()` işlevi. Bu nedenle, ki bu DAL yöntemi bir saklı yordam oluşturmanız gerekir. Server Explorer Görünüm menüsünde (veya isabet Ctrl + Alt + S) seçin ve genişletin `NORTHWND.MDF` düğümü. Yeni bir saklı yordam eklemek, saklı yordamlar düğümüne sağ tıklayın ve yeni bir saklı yordam Ekle'yi seçin (bkz. Şekil 6).


![Sayfalama ürünler aracılığıyla yeni bir saklı yordam Ekle](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Şekil 6**: sayfalama ürünler aracılığıyla yeni bir saklı yordam Ekle


Bu saklı yordamı iki tamsayı giriş parametrelerini - kabul etmelidir `@startRowIndex` ve `@maximumRows` ve `ROW_NUMBER()` işlevi sıralı olarak `ProductName` alan, yalnızca bu satırları büyüktür belirtilen döndüren `@startRowIndex` ve küçüktür veya eşit `@startRowIndex`  +  `@maximumRow` s. Yeni saklı yordam aşağıdaki betiği girin ve ardından saklı yordamı veritabanına eklemek için Kaydet simgesine tıklayın.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Saklı yordam oluşturduktan sonra test etmek için bir dakikanızı ayırın. Sağ `GetProductsPaged` saklı yordam adı sunucu Gezgini'nde ve Çalıştır seçeneğini belirleyin. Visual Studio daha sonra sizden giriş parametreleri için `@startRowIndex` ve `@maximumRow` s (bkz. Şekil 7). Farklı değerler deneyin ve sonuçları inceleyin.


![İçin bir değer girin @startRowIndex ve @maximumRows parametreleri](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>Şekil 7</strong>: bir değer girin @startRowIndex ve @maximumRows parametreleri


Sonra bu seçme, giriş parametreleri değerleri, çıkış penceresine sonuçları gösterilir. Şekil 8, 10'da her ikisi için geçerken sonuçları gösterir `@startRowIndex` ve `@maximumRows` parametreleri.


[![Kayıt, görüneceği, ikinci sayfa veri döndürülür](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Şekil 8**: kayıtları, görüneceği, ikinci sayfa veri döndürülür ([tam boyutlu görüntüyü görmek için tıklatın](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


Bu saklı oluşturulan yordamı biz yeniden oluşturmak için hazır `ProductsTableAdapter` yöntemi. Açık `Northwind.xsd` türü belirtilmiş veri kümesi, sağ tıklatın `ProductsTableAdapter`ve Sorgu Ekle seçeneğini belirleyin. Geçici SQL deyimi kullanarak sorguyu oluşturmak yerine, varolan bir saklı yordam kullanarak oluşturun.


![Varolan bir saklı yordam kullanarak DAL yöntemi oluşturma](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Şekil 9**: DAL yöntemi kullanarak varolan bir saklı yordam oluşturma


Ardından, biz çağrılacak saklı yordamı seçmeniz istenir. Çekme `GetProductsPaged` saklı yordamı aşağı açılan listeden.


![GetProductsPaged seçin saklı yordamı aşağı açılan listeden](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Şekil 10**: GetProductsPaged seçin saklı yordamı aşağı açılan listeden


Sonraki ekranda sonra ne tür veriler saklı yordam tarafından döndürülen isteyen: tablo verisi, tek bir değer veya herhangi bir değer. Bu yana `GetProductsPaged` saklı yordamı birden çok kayıt döndüren, tablo verisi döndüren gösterir.


![Saklı yordam tablo verisi döndüren belirtin](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Şekil 11**: saklı yordam tablo verisi döndüren belirtin


Son olarak, oluşturduğunuz istediğiniz yöntemlerin adlarını belirtin. Öğreticilerimizden önceki gibi devam edin ve DataTable hem dolgusu kullanma yöntemleri oluşturun ve bir DataTable döndür. İlk yöntem adı `FillPaged` ve ikinci `GetProductsPaged`.


![Ad yöntemleri FillPaged ve GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Şekil 12**: ad yöntemleri FillPaged ve GetProductsPaged


Buna ek olarak oluşturulan yönteme ürünlerin belirli bir sayfaya dönmek için DAL, ayrıca BLL böyle işlevselliği sağlamak ihtiyacımız var. DAL yöntemi gibi BLL s GetProductsPaged yöntemi en fazla satır ve başlangıç satır dizini belirtmek için iki tamsayı giriş kabul etmesi gerekir ve yalnızca belirli bir aralıkta kalan kayıt döndürmesi gerekir. Böyle bir BLL yöntemi yalnızca DAL s GetProductsPaged yöntemi aşağı çağrılar gibi sağladığı bunu ProductsBLL sınıfı oluşturun:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Herhangi bir adı kullanabilirsiniz BLL s yöntemi giriş parametreleri için ancak, kısa bir süre içinde göreceğiz olarak kullanmayı seçmeden `startRowIndex` ve `maximumRows` bize ek kaydeder bu yöntemi kullanmak için bir ObjectDataSource yapılandırırken iş bit.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>4. adım: Özel disk belleği kullanmayı ObjectDataSource yapılandırma

Belirli bir alt kayıtları tam erişim BLL ve DAL yöntemleri ile biz GridView oluşturmak için hazır re özel disk belleği'ni kullanarak, temel alınan kayıtlarını bu sayfalarla denetler. Başlangıç açarak `EfficientPaging.aspx` sayfasını `PagingAndSorting` klasöründe GridView sayfaya ekleyin ve yeni bir ObjectDataSource Denetimi kullanacak şekilde yapılandırın. Son öğreticilerimizden genellikle kullanacak şekilde yapılandırılmış ObjectDataSource vardı `ProductsBLL` s sınıfı `GetProducts` yöntemi. Bu kez, ancak kullanmak istiyoruz `GetProductsPaged` yöntemi bunun yerine, bu yana `GetProducts` yöntemi döndürür *tüm* veritabanında ürünlerin ise `GetProductsPaged` yalnızca belirli bir alt kayıtları döndürür.


![ObjectDataSource s ProductsBLL sınıfı GetProductsPaged yöntemi kullanmak üzere yapılandırma](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Şekil 13**: ObjectDataSource s ProductsBLL sınıfı GetProductsPaged yöntemi kullanmak üzere yapılandırma


Biz salt okunur GridView oluşturma re beri INSERT, UPDATE, yöntem açılan listesinden ayarlamak için bir dakikanızı ayırın ve sekmeleri (hiçbiri) SİLİN.

Ardından, ObjectDataSource Sihirbazı bize kaynakları için ister `GetProductsPaged` metodu s `startRowIndex` ve `maximumRows` giriş parametre değerleri. Bu giriş parametreleri gerçekten GridView tarafından otomatik olarak ayarlanır, böylece yalnızca kaynak kümesi yok olarak bırakın ve Son'u tıklatın.


![Giriş parametresi kaynakları yok olarak bırakın.](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Şekil 14**: giriş parametresi kaynakları yok olarak bırakın.


ObjectDataSource sihirbazını tamamladıktan sonra GridView BoundField veya CheckBoxField her ürün veri alanlarını içerir. GridView görünümünü gördüğünüz şekilde uyarlamak çekinmeyin. Ben seçimi yaptıysanız ve yalnızca görüntülemek için `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, ve `UnitPrice` BoundFields. Ayrıca, disk belleği akıllı etiketinde sayfalama etkinleştir onay kutusunu işaretleyerek desteklemek için GridView yapılandırın. GridView ve ObjectDataSource bildirim temelli biçimlendirme bu değişikliklerden sonra aşağıdakine benzer görünmelidir:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Bir tarayıcı aracılığıyla sayfasını ziyaret edin, ancak GridView yer yok ise bulunacak.


![GridView görüntülenmeyen olduğu](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Şekil 15**: GridView olan görüntülenmiyor


ObjectDataSource şu anda 0 değerleri olarak her iki için kullandığından GridView eksik `GetProductsPaged` `startRowIndex` ve `maximumRows` giriş parametreleri. Bu nedenle, sonuçta elde edilen SQL sorgu kayıt döndüren ve bu nedenle GridView görüntülenmez.

Bu sorunu gidermek için biz ObjectDataSource özel sayfalama kullanmak için yapılandırmanız gerekir. Bu, aşağıdaki adımlarda gerçekleştirilebilir:

1. **ObjectDataSource s ayarlamak `EnablePaging` özelliğini `true`**  bu geçmesi gereken ObjectDataSource gösterir `SelectMethod` iki ek parametreler: bir başlangıç satır dizini belirtmek için ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) ve bir en fazla satır sayısını belirtin ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **ObjectDataSource s ayarlamak `StartRowIndexParameterName` ve `MaximumRowsParameterName` özellikleri uygun şekilde** `StartRowIndexParameterName` ve `MaximumRowsParameterName` özellikleri gösterir yöntemlere geçirilen giriş parametrelerinin adları `SelectMethod` özel disk belleği amacıyla. Varsayılan olarak, bu parametre adları olan `startIndexRow` ve `maximumRows`, neden, olduğu oluştururken `GetProductsPaged` yöntemi BLL bu değerler için giriş parametrelerini kullandım. BLL s farklı parametre adları kullanmayı seçerseniz `GetProductsPaged` yöntemi gibi `startIndex` ve `maxRows`ObjectDataSource s gerekir örnek ayarlamak için `StartRowIndexParameterName` ve `MaximumRowsParameterName` özellikleri uygun şekilde (örneğin, startIndex için `StartRowIndexParameterName` ve maxRows için `MaximumRowsParameterName`).
3. **ObjectDataSource s ayarlamak [ `SelectCountMethod` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) toplam sayı, kayıt olan disk belleğine alınan aracılığıyla döndüren yöntem adına (`TotalNumberOfProducts`)** sözcüğünün `ProductsBLL` s sınıfı`TotalNumberOfProducts`yöntemi toplam sayısı üzerinden yürüten bir DAL yöntemi kullanarak disk belleği kayıtları döndürür bir `SELECT COUNT(*) FROM Products` sorgu. Bu bilgiler ObjectDataSource tarafından doğru bir şekilde sayfalama arabirimi işlemek için gereklidir.
4. **Kaldırma `startRowIndex` ve `maximumRows` `<asp:Parameter>` ObjectDataSource s bildirim temelli işaretleme öğeleri** Sihirbazı ile ObjectDataSource yapılandırırken, Visual Studio otomatik olarak iki eklenen `<asp:Parameter>` öğeleri için `GetProductsPaged` s yöntemi giriş parametreleri. Ayarlayarak `EnablePaging` için `true`, bu parametreleri otomatik olarak geçirilir; bunlar da bir bildirim temelli söz diziminde görünüyorsa ObjectDataSource geçirilecek deneyecek *dört* parametreleri `GetProductsPaged` yöntemi ve iki parametre için `TotalNumberOfProducts` yöntemi. Bunları kaldırmak, parantezi unutsanız bile `<asp:Parameter>` sayfası gibi bir hata iletisi alırsınız tarayıcısından ziyaret edildiğinde öğeleri: *ObjectDataSource 'ObjectDataSource1' 'sahip TotalNumberOfProducts' genel olmayan bir yöntem bulamadı parametreleri: startRowIndex, maximumRows*.

Bu değişiklikleri yaptıktan sonra ObjectDataSource s bildirim temelli söz dizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Unutmayın `EnablePaging` ve `SelectCountMethod` özelliklerini ayarlamak ve `<asp:Parameter>` öğeleri kaldırıldı. Bu değişiklikleri yaptıktan sonra Şekil 16 Özellikler penceresinin ekran görüntüsü gösterilmektedir.


![Özel disk belleği kullanmak için ObjectDataSource Denetimi yapılandırma](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Şekil 16**: özel disk belleği kullanmak için ObjectDataSource Denetimi yapılandırma


Bu değişiklikleri yaptıktan sonra bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Listelenen, 10 ürünleri görmelisiniz alfabetik olarak sıralanmış. Bir kerede veri bir sayfadan adım için bir dakikanızı ayırın. Varsayılan disk belleği ve özel disk belleği arasında son kullanıcı s perspektifinden görsel fark olsa da, yalnızca belirli bir sayfa için görüntülenecek gereken kayıtları alır gibi özel daha etkili bir şekilde sayfalama büyük miktarlarda veri sayfaları.


[![Verileri, s adı, ürün tarafından sipariş edilmiş olan disk belleğine alınan kullanarak özel sayfalama](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Şekil 17**: verileri, s adı, ürün tarafından sipariş edilmiş olan disk belleğine alınan kullanarak özel sayfalama ([tam boyutlu görüntüyü görmek için tıklatın](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> Özel disk belleği ile sayfayı saymak ObjectDataSource s tarafından döndürülen değer `SelectCountMethod` GridView s görünüm durumuna depolanır. Diğer GridView değişkenleri `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` koleksiyonu vb. depolanır *denetim durumu*, GridView s değerine bakılmaksızın kalıcı `EnableViewState` özellik. Bu yana `PageCount` değeri kalıcıdır görünüm durumu, son sayfayla almak için bir bağlantı içeren bir disk belleği arabirimi kullanılırken kullanarak Geri göndermeler arasında GridView s Görünüm durumunun etkin olmasını zorunlu. (Disk belleği Arabiriminizin doğrudan bir bağlantı son içermiyorsa sayfa, Görünüm durumu devre dışı bırakabilir ardından.)


Son sayfa bağlantıyı tıklatarak geri göndermeye neden olur ve güncelleştirmek için GridView bildirir, `PageIndex` özelliği. Son sayfa bağlantıya tıklandığında GridView atar, `PageIndex` bir değere değerinden kendi `PageCount` özelliği. Devre dışı, Görünüm durumu ile `PageCount` değeri Geri göndermeler arasında kaybolur ve `PageIndex` en büyük tamsayı değeri yerine atanır. Ardından, GridView çarpılarak başlangıç satır dizini belirlemeye çalışır `PageSize` ve `PageCount` özellikleri. Sonuçlanır bir `OverflowException` ürün izin verilen en büyük tamsayı boyutu aştığından.

## <a name="implement-custom-paging-and-sorting"></a>Uygulama özel sayfalama ve sıralama

Geçerli özel disk belleği kararlılığımızın oluştururken kullandığı veri disk belleğine alınan üzerinden sipariş statik olarak belirtilmesini gerektirir `GetProductsPaged` saklı yordamı. Ancak, GridView s akıllı etiket sayfalama etkinleştir seçeneğine ek olarak sıralamayı etkinleştir onay kutusunu içerdiğini not almış olabilirsiniz. Ne yazık ki, geçerli özel disk belleği kararlılığımızın GridView sıralama desteği ekleme yalnızca veri şu anda görüntülenen sayfadaki kayıtları sıralanır. Örneğin, GridView de disk belleği desteği ve daha sonra azalan düzende ürün adına göre veri'nın ilk sayfasında görüntülerken sıralamak için yapılandırırsanız, ürün siparişi 1 sayfasında geri alacaksınız. Şekil 18 görüldüğü gibi gibi etiket formu ilk ürün alfabetik olarak etiket formu sonra gelen 71 diğer ürünler yoksayar ters alfabetik sırayla sıralarken gösterir; yalnızca ilk sayfasındaki kayıtları sıralama olarak kabul edilir.


[![Yalnızca veri gösterilen geçerli sayfadaki sıralandı](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Şekil 18**: yalnızca veri gösterilen geçerli sayfadaki sıralanır ([tam boyutlu görüntüyü görmek için tıklatın](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


Sıralama BLL s verilerin alındıktan sonra gerçekleştirilmekte olduğundan sıralama yalnızca verilerin geçerli sayfa için geçerlidir `GetProductsPaged` yöntemi ve bu yöntem yalnızca belirli sayfa için kayıtları döndürür. Doğru sıralama uygulamak için sıralama ifadesi için geçirilecek ihtiyacımız `GetProductsPaged` yöntemi böylece veriler belirli bir sayfa veri göndermeden önce uygun şekilde sıralanabilir. Bu bizim sonraki öğreticide yerine getirmeyi göreceğiz.

## <a name="implementing-custom-paging-and-deleting"></a>Sayfalama ve silme özel uygulama

Verileri disk belleğine bulacaksınız, son sayfasından en son kaydını silme sırasında özel disk belleği teknikleri kullanarak GridView silme işlevleri etkinleştirme GridView uygun şekilde azaltma yerine GridView s kaybolur,`PageIndex`. Bu hatayı yeniden oluşturmak için yeni oluşturduğumuz öğretici için silme etkinleştirin. Burada biz 81 ürünleri, aynı anda 10 ürünler aracılığıyla sayfalama olduğundan tek bir ürün görmeniz gerekir (sayfa 9), son sayfasına gidin. Bu ürün silin.

GridView son ürünü silme bağlı *gereken* otomatik olarak sekizinci sayfasına gidin ve varsayılan disk belleği ile bu işlevselliğin izleyen gösteriyordu. Özel disk belleği ile ancak son sayfasında, son ürünün sildikten sonra GridView sadece ekrandan tamamen kaybolur. Kesin nedeni *neden* bu olan bir bit Bu öğreticinin kapsamı dışındadır gerçekleşir; bkz [GridView özel disk belleği ile son sayfasında son kaydını silmeye](http://scottonwriting.net/sowblog/posts/7326.aspx) kaynağı için alt düzey ayrıntıları Bu sorun oluştu. Özet olarak, aşağıdaki Sil düğmesine tıklandığında GridView tarafından gerçekleştirilen adımlar dizisini nedeniyle s:

1. Kaydı Sil
2. Belirtilen görüntülemek için uygun kayıtları almak `PageIndex` ve `PageSize`
3. Emin olmak için onay `PageIndex` GridView s otomatik olarak azaltma varsa, veri kaynağındaki; sayfa sayısını aşmadığından `PageIndex` özelliği
4. 2. adımda elde edilen kayıtları kullanarak GridView uygun bir sayfayı veri bağlama

Sorun gerçekler ile söz konusu adım 2 kaynaklandığını `PageIndex` görüntülenecek kayıt kapmasını hala olduğunda kullanılan `PageIndex` son sayfa tek kaydını yalnızca silindi. Bu nedenle, içinde 2. adım, *hiçbir* kayıtları, verileri son sayfasını artık tüm kayıtlar içerdiğinden döndürülür. Ardından, adım 3'te GridView, fark etti kendi `PageIndex` özellik sayfaları'nda veri kaynağı toplam sayısından büyük (bu yana ve son sayfasında son kayıtta sildik) ve bu nedenle azaltır, `PageIndex` özelliği. Adım 4'te GridView kendisini 2. adımda alınan verilere bağlamak çalışır; Ancak, hiçbir kaydın döndürüldüğünü 2. adımda, bu nedenle boş bir GridView kaynaklanan. Varsayılan sayfalama ile bu sorun eklenmemişse t yüzey çünkü 2. adım *tüm* veri kaynağından kayıtlar alınır.

Bu sorunu gidermek için şu iki seçeneğiniz vardır. İlk GridView s için bir olay işleyicisi oluşturmaktır `RowDeleted` kaç tane kaydın yalnızca silinen sayfasında görüntülenen belirleyen bir olay işleyicisi. Yalnızca bir kayıtla oluştu sonra silinen kaydı son verilmiş olması gerekir ve GridView s azaltma gerekiyor `PageIndex`. Elbette, yalnızca güncelleştirme istiyoruz `PageIndex` silme işlemini gerçekten başarılı olduysa, hangi belirlenebilir, sağlayarak `e.Exception` özelliği `null`.

Bu yaklaşım, güncelleştirdiğinden çalışır `PageIndex` 1. adım sonra ancak 2. adım önce. Bu nedenle, adım 2'de uygun kayıt kümesi döndürülür. Bunu yapmak için aşağıdaki gibi bir kod kullanın:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

ObjectDataSource s için bir olay işleyicisi oluşturmak için alternatif bir geçici çözüm olan `RowDeleted` olay ve ayarlamak için `AffectedRows` özelliğini 1 değeri. 1. adımında (ancak yeniden adım 2'de veri alma önce) kaydı sildikten sonra GridView güncelleştirir, `PageIndex` işlem tarafından bir veya daha fazla satır etkilendi, özellik. Ancak, `AffectedRows` tarafından ObjectDataSource özelliği ayarlı değil ve bu nedenle bu adım atlanır. Yürütülen Bu adım için bir yol olduğunu el ile ayarlamak için `AffectedRows` silme işlemi başarıyla tamamlarsa özelliği. Bu kod aşağıdaki gibi kullanarak gerçekleştirilebilir:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Bu olay işleyicilerini her ikisi de kodunu arka plan kod sınıfının içinde bulunabilir `EfficientPaging.aspx` örnek.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Varsayılan ve özel disk belleği performans karşılaştırma

Özel disk belleği varsayılan disk belleği döndürür ancak yalnızca gerekli kayıtları alır. bu yana *tüm* görüntülenmekte olan her sayfa için kayıtlar, s Temizle özel disk belleği varsayılan disk belleği değerinden daha etkilidir. Ancak özel disk belleği nasıl çok daha verimli olur? Ne tür bir performans kazancı elde edildi özel disk belleği için varsayılan disk belleği'nden taşıyarak görülebilir?

Ne yazık ki, orada s hiçbir uygun bir yanıt almak için buraya, tüm. Performans kazancı bir dizi etkene bağlıdır, en yaygın iki aracılığıyla havuzda kayıtları ve yük sayısı olan web sunucusu ve veritabanı sunucusu arasındaki veritabanı sunucusu ve iletişim kanalları yerleştirildiği. Yalnızca birkaç düzine kayıtları içeren küçük tablolar için göz ardı edilebilir bir performans farkı olabilir. Yüz binlerce satır binlerce ile büyük tablolar, bir performans farkı acute içindir.

Benim, bir makalenin [özel disk belleği, SQL Server 2005'te ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), çalıştırdım ile bir veritabanı tablosu aracılığıyla sayfalama bu iki disk belleği teknikleri arasındaki performans farkı gösteren bazı performans testleri içeriyor 50.000 kaydeder. Bu sınamalarda miyim SQL sunucu düzeyinde sorguyu yürütmek için her iki saat incelenmesi (kullanarak [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) ve ASP.NET sayfasını kullanarak [ASP.NET s izleme özellikleri](https://msdn.microsoft.com/library/y13fw6we.aspx). Bu testleri tek bir etkin kullanıcı ile my geliştirme kutusunda çalıştırılmış olan ve bu nedenle Bilimsel olmayan ve tipik bir Web sitesi yük düzenleri taklit değil olduğunu unutmayın. Ne olursa olsun, sonuçları yeterince büyük miktarlarda veri ile çalışırken, yürütme süresi için varsayılan ve özel disk belleği göreli farklılıkları gösterir.


|  | **Ort. Süre (sn)** | **Okur** |
| --- | --- | --- |
| **Varsayılan disk belleği SQL Profiler** | 1.411 | 383 |
| **Özel disk belleği SQL Profiler** | 0.002 | 29 |
| **Varsayılan disk belleği ASP.NET izleme** | 2.379 | *YOK* |
| **Özel disk belleği ASP.NET izleme** | 0.029 | *YOK* |


Gördüğünüz gibi belirli bir sayfada veri alma ortalama okuma daha az 354 gerekli ve kesir süre içinde tamamlanır. ASP.NET sayfasında, özel sayfa 1/100 yakın olarak işleme mümkün<sup>th</sup> varsayılan disk belleği kullanırken geçen süre. Bkz: [my makale](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) kodu ve veritabanı ile birlikte bu sonuçları hakkında daha fazla bilgi için bu testleri kendi ortamında yeniden oluşturmak için indirebilirsiniz.

## <a name="summary"></a>Özet

Disk belleği etkinleştir onay kutusunu veri Web denetimi s akıllı etiketin yalnızca onay uygulamak bağlamayı varsayılan disk belleği olan ancak böyle Basitlik karşılığında performans artırılabilir gelir. Bir kullanıcı veri herhangi bir sayfa istediğinde varsayılan disk belleği ile *tüm* kayıtları döndürüldüğü, rağmen bunların yalnızca küçük bir kesir gösterilebilir. Bu performans düşüklüğü mücadele etmek için alternatif bir disk belleği seçeneği özel sayfalama ObjectDataSource sunar.

Özel disk belleği görüntülenmesi gereken kayıtları alarak s performans sorunlarını disk belleği varsayılan üzerine artırır ancak, özel disk belleği uygulamak daha karmaşık s. İlk olarak, belirli alt kümesini istenen kaydeder, doğru (ve verimli bir şekilde) erişen bir sorgu yazılmalıdır. Bu, çeşitli yollarla gerçekleştirilebilir; Biz bu öğreticide incelenirken bir SQL Server 2005 s yeni kullanmaktır `ROW_NUMBER()` derece işleve sonuçları gruplayan ve ardından yalnızca döndürülecek olan derecelendirme belirtilen bir aralığa denk gelen sonuçları. Ayrıca, biz kayıtları aracılığıyla havuzda toplam sayısını belirlemek için bir yol eklemeniz gerekir. Bu DAL sayfalar ve BLL yöntemleri oluşturduktan sonra biz de toplamda kaç kaydın üzerinden havuzda ve satır dizini başlatmak ve en fazla satır değerleri için BLL doğru geçirebilirsiniz belirleyebilirsiniz ObjectDataSource yapılandırmanız gerekir.

Özel disk belleği uygulama birkaç adımı gerektirir ve değil yaklaşık olarak varsayılan sayfalama kadar basit hale getirir ancak özel disk belleği bir yeterince büyük miktarlarda veri sayfalama zorunluluktur. Sonuçları incelenirken olarak gösterilen, özel sayfalama ASP.NET sayfası işleme süresi dışına saniye boşaltmaktır ve yükü veritabanı sunucusunda bir veya birden fazla onlarca kat rengini açabilirsiniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](paging-and-sorting-report-data-vb.md)
> [İleri](sorting-custom-paged-data-vb.md)
