---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: "Verimli bir şekilde büyük miktarlarda verinin (VB) disk belleği | Microsoft Docs"
author: rick-anderson
description: "Varsayılan disk belleği veri sunu denetiminin büyük miktarlarda verinin, temel alınan veri kaynağı denetimi retriev olarak çalışırken uygun seçenektir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 00a5358361fa3f37d13ea74d61c437088b388ece
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Verimli bir şekilde büyük miktarlarda verinin (VB) disk belleği
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) veya [PDF indirin](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> Yalnızca bir veri alt kümesini görüntülenmesine karşın, temel alınan veri kaynağı denetimi tüm kayıtları alır gibi varsayılan disk belleği veri sunu denetimi büyük miktarlarda verilerin çalışırken uygunsuz seçeneğidir. Böyle durumlarda, biz özel etkinleştirmelisiniz sayfalama.


## <a name="introduction"></a>Giriş

Biz önceki öğreticide açıklandığı gibi disk belleği iki yoldan biriyle uygulanabilir:

- **Varsayılan disk belleği** etkinleştirmek disk belleği seçeneği denetleyerek uygulanabilir veri Web denetimi s akıllı etiketi; ancak, bir sayfa veri görüntüleme her ObjectDataSource alır *tüm* kayıtları, hatta ancak yalnızca bir alt kümesini görüntülenir sayfasında
- **Özel sayfalama** varsayılan performansını artırır, kullanıcı tarafından; istenen veri belirli sayfasının görüntülenmesi gereken veritabanından yalnızca kayıtları alarak disk belleği ancak, özel sayfalama uygulamak için biraz daha fazla çaba içerir varsayılan disk belleği'den

Uygulama yalnızca onay bir onay kutusu ve yapıldığı kolaylığı nedeniyle bitti! varsayılan disk belleği çekici bir seçenektir. Tüm kayıtları alınırken na ullanıcı yaklaşım yine de bir implausible seçim yeterince büyük miktarlarda veri veya siteler için aracılığıyla ile çok sayıda eşzamanlı kullanıcıyı sayfalama kolaylaştırır. Böyle durumlarda, biz esnek bir sistem sağlamak için disk belleği özel kapatmanız gerekir.

Özel sayfalama sınama hassas verilerin belirli bir sayfa için gerekli kayıt kümesini döndüren bir sorgu yazabilirsiniz. Neyse ki, Microsoft SQL Server 2005'in yeni bir anahtar sözcük derecelendirme sonuçlar için bize kapsanır kayıtların verimli bir şekilde alabilir bir sorgu yazmak sağlayan sağlar. Bu öğreticide bir GridView denetiminde özel sayfalama uygulamak için bu yeni SQL Server 2005'in anahtar sözcüğü kullanmayı göreceğiz. Özel sayfalama için kullanıcı arabirimi aynı varsayılan disk belleği, sonraki kullanarak bir sayfadan diğerine atlama için özel sayfalama birkaç varsayılan disk belleği daha hızlı olabilir.

> [!NOTE]
> Özel sayfalama tarafından sergilenen tam performans kazancı aracılığıyla havuzda kayıtlarının ve veritabanı sunucusuna yerleştirilen yük toplam sayısına bağlı olarak değişir. Bu öğreticinin sonunda avantajları elde size özel disk belleği performans sergiler bazı kaba ölçümleri inceleyeceğiz.


## <a name="step-1-understanding-the-custom-paging-process"></a>1. adım: özel disk belleği işlemi anlama

Verilerine disk belleği, istenen veri sayfasını ve sayfa başına görüntülenen kayıtlarının sayısı üzerinde bir sayfasında görüntülenen kesin kayıt bağlıdır. Örneğin, biz 81 ürünleri üzerinden sayfasında sayfa başına 10 ürün görüntüleme istediğinizi düşünelim. İlk sayfa görüntülerken, d ürünleri 1 ile 10 istiyoruz; İkinci sayfa görüntülerken d biz ürünleri 11 ile 20 ve benzeri içindir.

Kayıtları alınması gerekenler ve disk belleği arabirimi nasıl işleneceğini dikte üç değişkenleri şunlardır:

- **Satır dizini Başlat** ilk satırının sayfasında görüntülenecek veri dizinini; sayfa başına görüntülenecek kayıt tarafından sayfa dizini çarparak ve eklenirken bir hesaplanan dizin olabilir. Örneğin, 10, her seferinde için kayıtları (sayfa dizinini: 0) ilk sayfa aracılığıyla disk belleği, başlangıç satır dizini 0'dır \* 10 + 1 veya 1; (1, sayfa dizini'dir) İkinci sayfa için 1 Başlat satır dizini \* 10 + 1 , veya 11.
- **En fazla satır** en fazla sayfa başına görüntülenecek kayıt sayısı. İçin en son döndürülen sayfa boyutundan daha az kayıt sayfasını olabileceğinden bu değişkeni, en fazla satır adlandırılır. Örneğin, sayfa başına 81 ürünleri 10 kayıt üzerinden disk belleği, dokuzuncu ve son sayfası yalnızca bir kayıt olur. Hiçbir sayfa rağmen en fazla satır değerden daha fazla kayıt gösterir.
- **Toplam kayıt sayısı** aracılığıyla havuzda kayıtlarının toplam sayısı. Hangi kayıtların için belirli bir sayfa alınacağını belirlemek bu değişkeni olmayan t gerekli olsa da, disk belleği arabirimi dikte etmez. Örneğin, aracılığıyla havuzda 81 ürün varsa, disk belleği UI dokuz sayfa sayıları görüntülemek için sayfalama arabirimi bilir.

Varsayılan disk belleği ile en fazla satır yalnızca sayfa boyutu iken Başlat satır dizini sayfa dizini ve sayfa boyutu ve bir ürün olarak hesaplanır. Varsayılan disk belleği tüm kayıtlarını alır. bu yana böylece Başlat satır dizini satır karmaşık bir görev taşıma yapmadan herhangi bir sayfayı veri, her satır için dizin oluşturma sırasında veritabanı denir. Ayrıca, toplam kayıt sayısı olarak kullanıma hazır s yalnızca DataTable (veya nesneyi veritabanı sonuçları tutmak için kullanılan) kayıt sayısı.

Satır dizini başlatmak ve en fazla satır değişkenleri göz önüne alındığında, özel bir disk belleği uygulamasını yalnızca kesin kayıt alt kümesini Başlat satır dizini ve kayıt en fazla satır sayısı kadar bundan sonra Başlangıç döndürmesi gerekir. Özel sayfalama iki zorluk sağlar:

- Biz verimli bir şekilde aracılığıyla biz belirtilen başlangıç satırı dizinde kayıtları döndürme başlayabilmeniz için disk belleği tüm veri her satırda bir satır dizini ilişkilendirmek için
- Biz aracılığıyla havuzda kayıtlarının toplam sayısı sağlanması gerekiyor

Sonraki iki adımda Biz bu iki zorluk yanıt vermek için gereken SQL komut dosyası inceleyeceğiz. SQL komut dosyası ek olarak, biz de DAL ve BLL yöntemleri uygulamak gerekir.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>2. adım: aracılığıyla havuzda kayıtlarının toplam sayısı döndürme

Sayfa görüntülenmesine kayıtları hassas alt alma inceleyeceğiz önce aracılığıyla havuzda kayıtlarının toplam sayısını döndürmek nasıl ilk bakmak s olanak tanır. Bu bilgiler, disk belleği kullanıcı arabirimi düzgün bir şekilde yapılandırmak için gereklidir. Belirli bir SQL sorgusu tarafından döndürülen kayıt toplam sayısını kullanarak elde edilebilir [ `COUNT` toplama işlevi](https://msdn.microsoft.com/en-US/library/ms175997.aspx). Örneğin, kayıtlarının toplam sayısını belirlemek için `Products` tablo, aşağıdaki sorguyu kullanırız:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Bu bilgiler verir bizim DAL için bir yöntem ekleyin s olanak tanır. Adlı bir DAL yöntemi özellikle oluşturacağız `TotalNumberOfProducts()` yürütmelerinin `SELECT` yukarıda gösterilen deyimi.

Başlangıç açarak `Northwind.xsd` yazılan veri kümesi dosyasında `App_Code/DAL` klasör. Ardından, sağ tıklayın `ProductsTableAdapter` Tasarımcısı'nda ve Sorgu Ekle'i seçin. Biz olarak önceki eğitimlerine görülen ve bu yeni bir yöntemi DAL ile eklemek için bize izin verir, belirli SQL deyimi veya saklı yordam çağrıldığında, yürütülür. Olarak geçici SQL deyimi kullanmak için bu bir önceki öğreticileri yöntemlerinde bizim TableAdapter ile tercih.


![Geçici SQL deyimini kullanın](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Şekil 1**: geçici SQL deyimini kullanın


Sonraki ekranda biz oluşturmak için sorgu türünü belirtebilirsiniz. Bu sorgu kayıtlarının toplam sayısı skaler, tek bir değer döndürür beri `Products` tablo seçin `SELECT` singe değeri seçeneği döndürür.


![Sorgu tek bir değer döndüren bir SELECT deyimi kullanacak şekilde yapılandırma](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Şekil 2**: tek bir değer döndüren bir SELECT deyimi kullanılacak sorguyu yapılandırın


Kullanmak için sorgu türünü gösteren sonra biz sonraki sorgu belirtmeniz gerekir.


![SELECT COUNT(*) ürünleri SORGUDAN kullanın](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Şekil 3**: SELECT sayısını kullan (\*) FROM ürünleri sorgu


Son olarak, yöntemin adını belirtin. Daha önce bahsedilen, let s olarak kullanmak `TotalNumberOfProducts`.


![DAL yöntemi TotalNumberOfProducts adı](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Şekil 4**: DAL yöntemi TotalNumberOfProducts adı


Son'u tıklattıktan sonra Sihirbazı ekleyecek `TotalNumberOfProducts` DAL yöntemi. SQL sorgusu sonuç olması durumunda boş değer atanabilir türler DAL skaler döndürmeyi yöntemlerinde dönüş `NULL`. Bizim `COUNT` sorgu, ancak her zaman döndürecektir olmayan bir`NULL` değeri; ne olursa olsun, DAL yöntemi boş değer atanabilir bir tamsayı döndürür.

DAL yöntemi ek olarak, biz de BLL yönteminde gerekir. Açık `ProductsBLL` sınıf dosya ve ekleme bir `TotalNumberOfProducts` DAL s yalnızca çağıran yöntemi `TotalNumberOfProducts` yöntemi:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s `TotalNumberOfProducts` yöntemi boş değer atanabilir bir tamsayı döndürür; ancak biz oluşturulan ve `ProductsBLL` s sınıfı `TotalNumberOfProducts` olan standart bir tamsayı döndürecek şekilde yöntemi. Bu nedenle, biz gerek `ProductsBLL` s sınıfı `TotalNumberOfProducts` döndürme DAL s tarafından döndürülen boş değer atanabilir tamsayı değeri kısmı `TotalNumberOfProducts` yöntemi. Çağrı `GetValueOrDefault()` varsa; boş değer atanabilir tamsayı ise, boş değer atanabilir tamsayı değerini döndürür `null`, ancak varsayılan tamsayı değeri, 0 döndürür.

## <a name="step-3-returning-the-precise-subset-of-records"></a>3. adım: kayıtları hassas alt döndürme

Bizim sonraki görev DAL ve başlangıç satır dizini kabul BLL yöntemleri oluşturmak için ve en fazla satır değişkenleri daha önce bahsedilen ve uygun kayıtları döndürür. Bunu yapmadan let s ilk bakış gereken SQL komut. Bize karşılıklı biz verimli bir şekilde biz yalnızca bu kayıtları Başlat satır dizini (ve kayıtları en fazla kayıt sayısı kadar) başlangıç dönebilmek üzerinden disk belleği tüm sonuçları her satır için bir dizin atamak sahibi olması iştir.

Bu bir sınama değil, zaten varsa bir sütun veritabanı tablosundaki satır dizini görev yapar. İlk bakışta, düşünüyoruz `Products` s tablosu `ProductID` alan yeterli, ilk ürün taşıdığından `ProductID` 1, 2, ikinci ve benzeri. Ancak, bir ürün silinmesi bu yaklaşım nullifying dizisinin bir boşluk bırakır.

Böylece alınması kayıtların kesin alt etkinleştirme satır dizini aracılığıyla, sayfa için verileri etkin bir şekilde ilişkilendirmek için kullanılan iki genel teknikler şunlardır:

- **SQL Server 2005 s kullanarak `ROW_NUMBER()` anahtar sözcüğü** SQL Server 2005, yeni `ROW_NUMBER()` anahtar sözcüğü bir derecelendirme sırasını bazı temel her döndürülen kayıtla ilişkilendirir. Bu derecelendirme her satır için bir satır dizini kullanılabilir.
- **Bir tablo değişkeni kullanarak ve `SET ROWCOUNT`**  SQL Server s [ `SET ROWCOUNT` deyimi](https://msdn.microsoft.com/en-us/library/ms188774.aspx) sorguda işlem sonlandırmadan önce; toplam kayıt sayısını belirtmek için kullanılır [Tablo değişkenleri](http://www.sqlteam.com/item.asp?ItemID=9454) akin için tablo veri tutabilen yerel T-SQL değişkenleri [geçici tablolar](http://www.sqlteam.com/item.asp?ItemID=2029). Bu yaklaşım eşit çalışır Microsoft SQL Server 2005 ve SQL Server 2000 ile (ancak `ROW_NUMBER()` yaklaşımı, yalnızca SQL Server 2005'te çalışır).  
  
 Buradaki sahip bir tablo değişkeni oluşturmaktır bir `IDENTITY` sütunu ve verileri disk belleği aracılığıyla tablosunun birincil anahtarlar için sütunları. Ardından, verileri disk belleği aracılığıyla tablosunun içeriğini yazılan, böylece bir sıralı satır dizini ilişkilendirme tablo değişkenine (aracılığıyla `IDENTITY` sütun) tablosundaki her kayıt için. Tablo değişkeni doldurulmuş sonra bir `SELECT` deyimi tablo değişkeni üzerinde temel alınan tabloda ile birleştirilmiş, belirli kayıtları çıkarmak için çalıştırılabilir. `SET ROWCOUNT` Deyimi akıllıca tablo değişkenine dökümünün gerek kayıt sayısını sınırlamak için kullanılır.  
  
 Bu yaklaşım s verimliliği istenen, sayfa numarası dayalı olarak `SET ROWCOUNT` değeri Başlat satır dizini artı en fazla satır değeri atanır. İlk gibi düşük numaralı sayfaları aracılığıyla veri birkaç sayfa sayfalama bu çok verimli bir yaklaşımdır. Ancak, bir sayfa sonlarında alınırken varsayılan disk belleği benzeri performans sergiler.

Disk belleği özel kullanarak bu öğreticinin uygulayan `ROW_NUMBER()` anahtar sözcüğü. Tablo değişkeni kullanma hakkında daha fazla bilgi için ve `SET ROWCOUNT` teknik bkz [bir disk belleği aracılığıyla büyük sonuç kümeleri için daha fazla verimli yöntemi](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

`ROW_NUMBER()` Anahtar sözcüğü bir belirli aşağıdaki sözdizimini kullanarak sıralama üzerinden döndürülen her kayıt sıralaması ilişkili:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()`belirtilen sıralama göre her bir kayıt için derecesini belirtir sayısal bir değer döndürür. Örneğin, en iyi sıralı her ürün için derecesini görmek için en az pahalı biz aşağıdaki sorguyu kullanabilirsiniz:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Şekil 5 bu sorgu Visual Studio sorgu penceresinde aracılığıyla çalıştırdığınızda s sonuçları gösterir. Ürünleri fiyat, her satır için bir fiyat derece birlikte tarafından sıralanır unutmayın.


![Fiyat derecesini için dahil edilen her döndürülen kayıt](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Şekil 5**: için fiyat derece dahil her döndürülen kayıt


> [!NOTE]
> `ROW_NUMBER()`birçok yeni derecelendirme işlevleri yalnızca biri, SQL Server 2005'te kullanılabilir. Daha kapsamlı bir irdelemesi `ROW_NUMBER()`, diğer derecelendirme işlevler yanı sıra, okuma [Microsoft SQL Server 2005'te derece sonuçları döndüren](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Sonuçları tarafından belirtilen sıralama zaman `ORDER BY` sütununda `OVER` yan tümcesi (`UnitPrice`, yukarıdaki örnekte), SQL Server sonuçlarını sıralama gerekir. Bu sonuçları sipariş edilen tarafından sütunları üzerinden kümelenmiş bir dizin ise hızlı bir işlemdir veya bir kapsayıcı olup olmadığını dizin ancak Aksi halde daha pahalı olabilir. Yardımcı olmak için yeterince büyük sorgular performansını olarak sonuçları göre sıralanmış sütun için bir kümelenmemiş dizin eklemeyi düşünün. Bkz: [sıralaması işlevleri ve SQL Server 2005'te performans](http://www.sql-server-performance.com/ak_ranking_functions.asp) başarım düşünceleri daha ayrıntılı bir bakış için.

Tarafından döndürülen derecelendirme bilgi `ROW_NUMBER()` doğrudan kullanılamaz `WHERE` yan tümcesi. Ancak, türetilmiş tablo döndürmek için kullanılabilir `ROW_NUMBER()` ardından görüntülenebilir sonuç `WHERE` yan tümcesi. Örneğin, aşağıdaki sorguyu ile birlikte ProductName ve UnitPrice sütunlarını döndürmek için bir türetilmiş tablo kullanır `ROW_NUMBER()` sonucu ve kullandığı bir `WHERE` sadece bu ürünler, fiyat derece döndürmek için yan tümcesi olan 11 ile 20 arasında:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Bu kavram biraz daha fazla genişletme, belirli bir sayfayı istenen satır dizini başlatmak ve en fazla satır değerleri verilen veri almak için bu yaklaşım kullanabilir:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Bu öğreticide daha sonra göreceğiz gibi  *`StartRowIndex`*  tarafından sağlanan ObjectDataSource sıfırda, başlangıç ancak dizinlenir `ROW_NUMBER()` SQL Server 2005 tarafından döndürülen değer dizini 1'den başlayarak. Bu nedenle, `WHERE` yan tümcesi döndürür kayıtların nerede `PriceRank` değerinden kesinlikle büyük olan  *`StartRowIndex`*  ve küçük veya eşit  *`StartRowIndex`*   +  *`MaximumRows`*.


Şimdi görüyoruz nasıl ele alınan ve `ROW_NUMBER()` olabilir satır dizini başlatmak ve en fazla satır değerleri verilen verileri belirli bir sayfa almak için kullanılan, artık bu mantık DAL ve BLL yöntemleri olarak uygulamak ihtiyacımız.

Biz sıralama karar vermelisiniz bu sorguyu oluştururken kullanacağı sonuçları derece verilecek; ürünleri adı alfabetik sıralama s olanak tanır. Bu, özel bir disk belleği uygulama Bu öğreticide biz de sıralanabilir daha özel bir disk belleğine alınan rapor oluşturabilmek için olmayacağını anlamına gelir. Sonraki öğreticide, ancak bu tür işlevselliği nasıl sağlanabilir göreceğiz.

Önceki bölümde bir geçici SQL deyimi DAL yöntemi oluşturduk. Ne yazık ki, T-SQL ayrıştırıcı Visual Studio'da gibi TableAdapter Sihirbazı mevcut değil t tarafından kullanılan `OVER` tarafından kullanılan sözdizimi `ROW_NUMBER()` işlevi. Bu nedenle, biz bu DAL yöntemi bir saklı yordam oluşturmanız gerekir. Sunucu Gezgini Görünüm menüsünde (veya isabet Ctrl + Alt + S) seçin ve genişletin `NORTHWND.MDF` düğümü. Yeni bir saklı yordam eklemek için saklı yordamlar düğümüne sağ tıklayın ve yeni bir saklı yordam Ekle'yi seçin (bkz. Şekil 6).


![Sayfalama ürünleri aracılığıyla yeni bir saklı yordam ekleyin](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Şekil 6**: sayfalama ürünleri aracılığıyla yeni bir saklı yordam ekleyin


Bu saklı yordam iki tamsayı giriş parametreleri - kabul etmelidir `@startRowIndex` ve `@maximumRows` ve `ROW_NUMBER()` işlevi göre sıralayarak `ProductName` alan, yalnızca bu satırları büyük belirtilen döndüren `@startRowIndex` ve küçüktür veya eşit `@startRowIndex`  +  `@maximumRow` s. Yeni bir saklı yordam aşağıdaki betiği girin ve ardından saklı yordamı veritabanına eklemek için Kaydet simgesine tıklayın.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Saklı yordam oluşturduktan sonra onu test etmek için bir dakikanızı ayırın. Sağ `GetProductsPaged` saklı yordam adı Server Explorer'da ve yürütme seçeneğini belirleyin. Visual Studio sonra sorar giriş parametreleri için `@startRowIndex` ve `@maximumRow` s (bkz. Şekil 7). Farklı değerler deneyin ve sonuçları inceleyin.


![İçin bir değer girin @startRowIndex ve @maximumRows parametreleri](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

**Şekil 7**: için bir değer girin @startRowIndex ve @maximumRows parametreleri


Sonra bu seçme giriş parametreleri değerleri, çıktı penceresinde sonuçları gösterilir. Şekil 8, 10'da her ikisi için geçirilirken sonuçları gösterir `@startRowIndex` ve `@maximumRows` parametreleri.


[![Kayıtları, görüneceği, ikinci sayfa veri döndürülür](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Şekil 8**: kayıtları olduğunu görüneceği, ikinci sayfa veri döndürülür ([tam boyutlu görüntüyü görüntülemek için tıklatın](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


Bu saklı oluşturulan yordamı biz oluşturmak için hazır re `ProductsTableAdapter` yöntemi. Açık `Northwind.xsd` yazılan veri kümesi, sağ tıklatın `ProductsTableAdapter`ve Sorgu Ekle seçeneğini belirleyin. Geçici SQL deyimi kullanarak sorguyu oluşturmak yerine var olan bir saklı yordamı kullanarak oluşturun.


![Varolan bir saklı yordamı kullanarak DAL yöntemi oluşturma](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Şekil 9**: varolan bir saklı yordamı kullanarak DAL yöntemi oluşturma


Ardından, biz çağırmak için saklı yordam seçmeniz istenir. Çekme `GetProductsPaged` saklı yordamı aşağı açılan listeden.


![GetProductsPaged seçin saklı yordamı aşağı açılan listeden](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Şekil 10**: GetProductsPaged seçin saklı yordamı aşağı açılan listeden


Sonraki ekranda sonra ne tür veriler saklı yordam tarafından döndürülen ister: Tablo verileri, tek bir değer veya değer yok. Bu yana `GetProductsPaged` saklı yordam birden çok kayıt iade, tablo veri döndürdüğünü gösterir.


![Saklı yordam tablo verisi döndüren belirtin](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Şekil 11**: saklı yordamı tablo verisi döndüren belirtin


Son olarak, oluşturduğunuz istediğiniz yöntemleri adlarını gösterir. Önceki öğreticilerimizi gibi ile devam edin ve hem dolgusu kullanma yöntemleri DataTable oluşturma ve DataTable döndürür. İlk yöntem adı `FillPaged` ve ikinci `GetProductsPaged`.


![Ad yöntemleri FillPaged ve GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Şekil 12**: ad yöntemleri FillPaged ve GetProductsPaged


Ayrıca oluşturulan yönteme ürünlerin belirli bir sayfaya dönmek için DAL, biz de BLL böyle işlevindeki sağlamanız gerekir. DAL yöntemi gibi BLL s GetProductsPaged yöntemi satır dizini başlatmak ve en fazla satır belirtmek için iki tamsayı girdi kabul etmeniz gerekir ve yalnızca belirtilen aralıkta kayıtları döndürmesi gerekir. Bu tür bir BLL yöntemi yalnızca çağrılarını aşağı DAL s GetProductsPaged yöntemi sözlüğüdür olduğunu ProductsBLL sınıfında oluşturun:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Herhangi bir ad kullanabilirsiniz BLL s yöntemi giriş parametreleri için ancak, kısa bir süre içinde göreceğiz olarak kullanmayı seçmeden `startRowIndex` ve `maximumRows` bize fazladan kaydeder bu yöntemi kullanmak için bir ObjectDataSource yapılandırırken iş bit.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>4. adım: Özel sayfalama kullanacak biçimde ObjectDataSource yapılandırma

Belirli bir alt kayıtların tam erişim BLL ve DAL yöntemleri ile biz GridView oluşturmak için hazır re özel sayfalama kullanarak temel kayıtlarını bu sayfalarıyla denetler. Başlangıç açarak `EfficientPaging.aspx` sayfasındaki `PagingAndSorting` klasörü, sayfaya GridView eklemek ve yeni ObjectDataSource Denetimi kullanacak şekilde yapılandırın. Son öğreticilerimizi biz genellikle kullanmak üzere yapılandırılmış ObjectDataSource vardı `ProductsBLL` s sınıfı `GetProducts` yöntemi. Bu süre, ancak biz kullanmak istediğiniz `GetProductsPaged` yöntemi bunun yerine, bu yana `GetProducts` yöntemi döndürür *tüm* veritabanındaki ürünlerin ancak `GetProductsPaged` yalnızca belirli kayıtların bir alt kümesini döndürür.


![ObjectDataSource s ProductsBLL sınıfı GetProductsPaged yöntemi kullanmak üzere yapılandırma](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Şekil 13**: ObjectDataSource s ProductsBLL sınıfı GetProductsPaged yöntemi kullanmak üzere yapılandırma


Biz salt okunur GridView oluşturma re itibaren INSERT, UPDATE, yöntem açılan listesi ayarlamak için bir dakikanızı ayırın ve sekmeleri (hiçbiri) SİLİN.

Ardından, ObjectDataSource Sihirbazı bize kaynakları için ister `GetProductsPaged` s yöntemi `startRowIndex` ve `maximumRows` giriş parametre değerleri. Bu giriş parametreleri gerçekte GridView tarafından otomatik olarak ayarlanır, böylece yalnızca kaynak kümesi None olarak bırakın ve Son'u tıklatın.


![Giriş parametresi kaynakları hiçbiri olarak bırakın](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Şekil 14**: giriş parametresi kaynakları hiçbiri olarak bırakın


ObjectDataSource Sihirbazı'nı tamamladıktan sonra GridView BoundField veya CheckBoxField ürün veri alanların her biri için içerir. GridView s görünüm uygun gördüğünüz şekilde uyarlamak çekinmeyin. I ullanıcı seçti yalnızca görüntülenecek `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, ve `UnitPrice` BoundFields. Ayrıca, disk belleği akıllı etiket sayfalama etkinleştir onay kutusunu işaretleyerek desteklemek için GridView yapılandırın. Bu değişikliklerden sonra GridView ve ObjectDataSource bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Bir tarayıcı aracılığıyla sayfasını ziyaret edin, ancak GridView hiçbir yerdir bulunma.


![GridView gösterilmez olduğu](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Şekil 15**: GridView olduğu görüntülenmiyor


ObjectDataSource şu anda 0 değerleri olarak her iki için kullandığından GridView eksik `GetProductsPaged` `startRowIndex` ve `maximumRows` giriş parametreleri. Bu nedenle, sonuçta elde edilen SQL sorgu hiç kayıt döndürüyor ve bu nedenle GridView görüntülenmiyor.

Bu sorunu gidermek için şu ObjectDataSource özel disk belleği kullanacak şekilde yapılandırmanız gerekir. Bu aşağıdaki adımlarda gerçekleştirilebilir:

1. **ObjectDataSource s ayarlamak `EnablePaging` özelliğine `true`**  bu geçmesi gereken ObjectDataSource gösterir `SelectMethod` iki ek parametreler: bir başlangıç satır dizini belirtmek için ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)), diğeri en fazla satır belirtmek için ([`MaximumRowsParameterName`](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **ObjectDataSource s ayarlamak `StartRowIndexParameterName` ve `MaximumRowsParameterName` uygun şekilde özellikleri** `StartRowIndexParameterName` ve `MaximumRowsParameterName` özellikleri içine geçirilen giriş parametreleri adlarını gösterir `SelectMethod` özel disk belleği amacıyla . Varsayılan olarak, bu parametre adları olan `startIndexRow` ve `maximumRows`, neden, olduğu oluştururken `GetProductsPaged` yöntemi BLL bu değerleri giriş parametreleri için kullandım. Farklı parametre adları BLL s için kullanmayı tercih `GetProductsPaged` yöntemi gibi `startIndex` ve `maxRows`gerekir örnek ObjectDataSource s ayarlamak için `StartRowIndexParameterName` ve `MaximumRowsParameterName` özellikleri buna göre (örneğin, startIndex için `StartRowIndexParameterName` ve maxRows için `MaximumRowsParameterName`).
3. **ObjectDataSource s ayarlamak [ `SelectCountMethod` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) toplam sayı, kayıtları olan disk belleği aracılığıyla döndüren yöntemi adına (`TotalNumberOfProducts`)** sözcüğünün `ProductsBLL` s sınıfı`TotalNumberOfProducts`yöntemi döndürür yürüten bir DAL yöntemi kullanılarak üzerinden disk belleği kayıtlarının toplam sayısı bir `SELECT COUNT(*) FROM Products` sorgu. Bu bilgiler ObjectDataSource tarafından doğru disk belleği arabirimini oluşturmak için gereklidir.
4. **Kaldırma `startRowIndex` ve `maximumRows` `<asp:Parameter>` ObjectDataSource s bildirim temelli biçimlendirme öğelerinden** Sihirbazı aracılığıyla ObjectDataSource yapılandırırken, Visual Studio otomatik olarak iki eklenen `<asp:Parameter>` öğeler için `GetProductsPaged` s yöntemi giriş parametreleri. Ayarlayarak `EnablePaging` için `true`, bu parametreleri otomatik olarak geçirilir; bunlar da bildirim temelli sözdiziminde görünüyorsa, ObjectDataSource geçirmek deneyecek *dört* parametreleri `GetProductsPaged` yöntemi ve iki parametre `TotalNumberOfProducts` yöntemi. Bunlar kaldırmak unutursanız `<asp:Parameter>` gibi bir hata iletisi alacaksınız bir tarayıcı aracılığıyla sitesini ziyaret ettiğinde öğeleri: *ObjectDataSource 'ObjectDataSource1' genel olmayan yöntemi 'sahip TotalNumberOfProducts' bulamadı Parametreler: startRowIndex, maximumRows*.

Bu değişiklikleri yaptıktan sonra ObjectDataSource s tanımlayıcı sözdizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Unutmayın `EnablePaging` ve `SelectCountMethod` özelliklerini ayarlamak ve `<asp:Parameter>` öğeler kaldırıldı. Bu değişiklikler yapıldıktan sonra Şekil 16 penceresinin ekran görüntüsü gösterilmektedir.


![ObjectDataSource Denetimi özel sayfalama kullanacak biçimde yapılandırın](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Şekil 16**: özel sayfalama kullanacak biçimde ObjectDataSource Denetimi yapılandırma


Bu değişiklikleri yaptıktan sonra bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Listelenen, 10 ürünler görmelisiniz alfabetik sıraya göre. Bir kerede bir sayfa veri adım için bir dakikanızı ayırın. Son kullanıcı s perspektifinden görsel fark varsayılan disk belleği ve özel sayfalama arasında olsa da, yalnızca belirli bir sayfa için görüntülenecek gereken kayıtları alır gibi daha verimli bir şekilde özel disk belleği büyük miktarlarda verinin sayfaları.


[![Veriler, sipariş edilmiş s adı, ürün tarafından olan disk belleği, kullanma, özel disk belleği](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Şekil 17**: verileri, sipariş edilmiş s adı, ürün tarafından olan disk belleği, kullanma, özel disk belleği ([tam boyutlu görüntüyü görüntülemek için tıklatın](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> Özel disk belleği ile sayfayı saymak ObjectDataSource s tarafından döndürülen değer `SelectCountMethod` GridView s görünüm durumuna depolanır. Diğer GridView değişkenleri `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` koleksiyonu vb. depolanır *denetim durumu*, GridView s değerini bakılmaksızın kalıcı `EnableViewState` özellik. Bu yana `PageCount` değeri kalıcıdır görünüm durumu, son sayfaya götüren bir bağlantı içeren bir disk belleği arabirimi kullanılırken kullanarak Geri göndermeler arasında GridView s Görünüm durumunun etkinleştirilmesi gerekir. (Disk belleği Arabiriminizin doğrudan bir bağlantı son içermiyorsa sayfa, Görünüm durumu devre dışı bırakabilir ardından.)


Son sayfa bağlantısını tıklatarak geri gönderimin neden olur ve güncelleştirmek için GridView bildirir, `PageIndex` özelliği. Son sayfayı bağlantıya tıkladıysanız GridView atar kendi `PageIndex` özelliğini bir değere değerinden kendi `PageCount` özelliği. Devre dışı, Görünüm durumu ile `PageCount` değeri arasında Geri göndermeler kaybolur ve `PageIndex` en büyük tamsayı değeri yerine atanır. Ardından, GridView çarparak başlangıç satır dizini belirlemeye çalışır `PageSize` ve `PageCount` özellikleri. Bunun bir `OverflowException` ürün izin verilen en büyük tamsayı boyutu aştığından.

## <a name="implement-custom-paging-and-sorting"></a>Uygulama özel sayfalama ve sıralama

Bizim geçerli özel disk belleği uygulaması oluştururken kullandığı veri disk belleği aracılığıyla sipariş statik olarak belirtilmesini gerektirir `GetProductsPaged` saklı yordamı. Ancak, GridView s akıllı etiket etkinleştirmek disk belleği seçeneği yanı sıra sıralama etkinleştir onay kutusunu içeren ettiğiniz. Ne yazık ki, bizim geçerli bir özel disk belleği uygulama GridView sıralama desteği ekleme yalnızca veri şu anda görüntülenen sayfadaki kayıtları sıralanır. Örneğin, disk belleği da destekler ve daha sonra azalan sırada, ürün adı bazında veri'nın ilk sayfasında görüntülerken sıralamak için GridView yapılandırırsanız, ürünleri sırasını 1 sayfasında ters. Şekil 18 görüldüğü gibi etiket formu ilk ürün etiket formu alfabetik olarak gelen 71 diğer ürünler yoksayar ters alfabetik sırayla sıralarken gösterir; yalnızca ilk sayfasındaki kayıtları sıralama olarak kabul edilir.


[![Yalnızca veri gösterilen geçerli sayfada sıralanır](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Şekil 18**: yalnızca veri gösterilen geçerli sayfada sıralanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


Sıralama verileri BLL s'den aldıktan sonra oluşturduğumuzdan sıralama yalnızca geçerli sayfasına veri uygulanır `GetProductsPaged` yöntemi ve bu yöntem yalnızca belirli sayfa için kayıtları döndürür. Doğru sıralama uygulamak için sıralama ifadesi geçirmek ihtiyacımız `GetProductsPaged` yöntemi böylece veriler belirli sayfa veri göndermeden önce uygun şekilde sıralanabilir. Bu bizim sonraki öğreticide gerçekleştirmek nasıl göreceğiz.

## <a name="implementing-custom-paging-and-deleting"></a>Disk belleği ve silme özel uygulama

Veri disk belleği bulacaksınız, son sayfasından son kaydı silinirken özel disk belleği teknikler kullanılarak GridView içinde silme işlevini etkinleştirme GridView uygun şekilde azaltma yerine GridView s kaybolur,`PageIndex`. Bu hata yeniden oluşturmak için yalnızca yeni oluşturduğumuz öğretici için silme etkinleştirin. Burada size 81 ürünler, aynı anda 10 ürün üzerinden disk belleği bu yana tek bir ürün görmeniz gerekir (sayfa 9), son sayfasına gidin. Bu ürün silin.

GridView son ürün silme bağlı *gereken* otomatik olarak sekizinci sayfasına gidin ve bu tür işlevselliği varsayılan disk belleği ile sergilenen. Özel disk belleği ile ancak son sayfasında, son bu ürünü sildikten sonra GridView yalnızca ekranından tamamen kaybolur. Kesin neden *neden* bu biraz Bu öğretici kapsamında oluşur; bkz [GridView özel disk belleği ile son sayfasındaki son kayıt silmeye](http://scottonwriting.net/sowblog/posts/7326.aspx) kaynakla alt düzey ayrıntılar için Bu sorun. Özet olarak, aşağıdaki Sil düğmesine tıklandığında, GridView tarafından gerçekleştirilen adımlar dizisini nedeniyle s:

1. Kaydını sil
2. Belirtilen görüntülemek için uygun kayıtların tamamını `PageIndex` ve`PageSize`
3. Emin olmak için onay `PageIndex` otomatik olarak GridView s azaltma varsa, veri kaynağındaki; sayfaların sayısını aşmadığından `PageIndex` özelliği
4. 2. adımda elde edilen kayıtları kullanarak GridView veri uygun sayfaya bağlamak

Sorunun zorluk ortaya söz konusu adım 2 kaynaklandığını `PageIndex` görüntülenecek kayıt kapmasını hala olduğunda kullanılan `PageIndex` son sayfasının tek kaydını yalnızca silindi. Bu nedenle, adım 2'deki *hiçbir* kayıtları veri son bu sayfayı artık herhangi bir kayıt içerdiğinden döndürülür. Sonra adım 3'te GridView, gerçekleştirir, `PageIndex` özellik sayfaları veri kaynağındaki toplam sayısından daha (biz bu yana ve son sayfasındaki son kaydı silinir) ve bu nedenle azaltır, `PageIndex` özelliği. Adım 4'te GridView kendisini 2. adımda alınan veri bağlamak çalışır; Ancak, hiçbir kaydı döndürülmedi 2. adımda, bu nedenle boş bir GridView elde edilmesidir. Varsayılan disk belleği ile bu sorunu içermiyor t yüzey çünkü adım 2'deki *tüm* kayıtları, veri kaynağından alınır.

Bu sorunu gidermek için şu iki seçeneğiniz vardır. İlk GridView s için bir olay işleyicisi oluşturmaktır `RowDeleted` kaç kayıtları yalnızca silindi sayfasında görüntülenen her belirler olay işleyicisi. Yalnızca tek bir kayıt vardı sonra silinen kaydı sonuncu verilmiş olması gerekir ve GridView s düşürmek ihtiyacımız `PageIndex`. Elbette, yalnızca güncelleştirme istiyoruz `PageIndex` silme işlemi gerçekte başarılı olduysa, hangi belirlenebilir, sağlayarak `e.Exception` özelliği `null`.

Bu yaklaşım, güncelleştirdiğinden çalışır `PageIndex` 1. adım sonra ancak 2. adım önce. Bu nedenle, adım 2'de uygun kayıt kümesi döndürülür. Bunu başarmak için aşağıdaki gibi bir kod kullanın:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Alternatif bir geçici çözüm ObjectDataSource s için bir olay işleyicisi oluşturmaktır `RowDeleted` olay ve ayarlamak için `AffectedRows` için 1 değerini özelliği. 1. adımda (ancak adım 2'deki verileri yeniden alma önce) kaydını sildikten sonra GridView güncelleştirmeleri kendi `PageIndex` bir veya daha fazla satır işlem tarafından etkilenen varsa özelliği. Ancak, `AffectedRows` ObjectDataSource tarafından özelliği ayarlı değil ve bu nedenle bu adım atlanır. Yürütülen Bu adım için bir yol olduğundan el ile ayarlamak için `AffectedRows` silme işlemi başarıyla tamamlarsa özelliği. Bu, aşağıdaki gibi kod kullanarak gerçekleştirilebilir:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Bu olay işleyicilerini her ikisi de kodunu arka plan kodu sınıfında bulunabilir `EfficientPaging.aspx` örnek.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Varsayılan ve özel disk belleği performans karşılaştırma

Varsayılan disk belleği verir kullanılabilirken özel sayfalama yalnızca gereken kayıtları alır beri *tüm* görüntülenmesini, her bir sayfa için kayıt, s temizleyin özel sayfalama varsayılan disk belleği değerinden daha etkilidir. Ancak özel sayfalama ne kadar daha verimli olur? Ne tür bir performans kazancı özel disk belleği için varsayılan disk belleği'nden taşıyarak görülebilir mi?

Ne yazık ki, tek bir boyut var. s uyan tüm yanıt burada. Performans kazancı bir dizi etkene bağlıdır, en belirgin iki aracılığıyla havuzda kaydeder ve yük sayısı olan web sunucusu ve veritabanı sunucusu arasındaki veritabanı sunucusu ve iletişim kanalları eklenir. Yalnızca birkaç düzine kayıtlarla küçük tablolar için bir performans farkı durum göz ardı edilebilir. Yüz binlerce satır binlerce ile büyük tablolar için yine de performans acute farktır.

Benim, bir makalenin [ASP.NET 2. 0'sql Server 2005'te özel sayfalama](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), çalıştırdım performans ile bir veritabanı tablosu aracılığıyla sayfalama bu iki disk belleği teknikler arasındaki farklılıkları göstermesi için bazı performans testleri içerir 50.000 kaydeder. Bu sınamalarda t SQL sunucu düzeyinde sorguyu yürütmek için her iki saat incelenmesi (kullanarak [SQL Profiler](https://msdn.microsoft.com/en-us/library/ms173757.aspx)) ve ASP.NET sayfası kullanarak [ASP.NET s izleme özellikleri](https://msdn.microsoft.com/en-US/library/y13fw6we.aspx). Bu testler tek bir etkin kullanıcı my geliştirme kutusuyla çalıştırmak olan ve bu nedenle Bilimsel olmayan ve tipik Web sitesi yük desenlerini taklit değil olduğunu göz önünde bulundurun. Ne olursa olsun, sonuçları yeterince büyük miktarlarda veri çalışırken varsayılan ve özel sayfalama için yürütme süresi göreli farklar gösterilmektedir.


|  | **Ort. Süresi (saniye)** | **Okur** |
| --- | --- | --- |
| **Varsayılan disk belleği SQL Profil Oluşturucu** | 1.411 | 383 |
| **Özel disk belleği SQL Profil Oluşturucu** | 0.002 | 29 |
| **Varsayılan disk belleği ASP.NET izleme** | 2.379 | *YOK* |
| **Özel sayfalama ASP.NET izleme** | 0.029 | *YOK* |


Gördüğünüz gibi verilerin belirli bir sayfa okuma daha az 354 ortalama gerekli ve alma süresi bir kısmı tamamlandı. ASP.NET sayfasında, özel sayfa 1/100 yakın işlenecek kurtararak<sup>th</sup> saati varsayılan disk belleği kullanırken sürdü. Bkz: [my makale](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) kodu ve bir veritabanı ile birlikte bu sonuçlar hakkında daha fazla bilgi için bu testleri kendi ortamınızda yeniden yükleyebilirsiniz.

## <a name="summary"></a>Özet

Yalnızca onay sayfalama etkinleştir onay kutusunu veri Web denetim s akıllı etiketi içinde uygulamak için bağlamayı varsayılan disk belleği olan ancak bu tür Basitlik performans artırılabilir gelir. Kullanıcı verilerinin herhangi bir sayfayı istediğinde varsayılan disk belleği ile *tüm* kayıtları döndürüldü, yalnızca küçük bir kesir bunların gösterilen olsa bile. Bu performans yükünü mücadele etmek için alternatif bir disk belleği seçeneği özel sayfalama ObjectDataSource sunar.

S performans sorunlarını görüntülenmesi gereken kayıtları alarak disk belleği varsayılan bağlı özel sayfalama artırır ancak, özel sayfalama uygulamak daha karmaşık s. İlk olarak, bir sorgu, doğru (etkin) istenen kayıtların belirli alt erişir ve yazılmış olmalıdır. Bu çeşitli şekillerde gerçekleştirilebilir; Biz bu öğreticide incelenmesi bir SQL Server 2005 s yeni kullanmaktır `ROW_NUMBER()` derece işleve sonuçları ve ardından yalnızca döndürülecek belirli bir aralıkta, derecelendirme döner sonuçlanır. Ayrıca, biz aracılığıyla havuzda kayıtlarının toplam sayısını belirlemek için bir yol eklemeniz gerekir. Bu DAL ve BLL yöntemleri oluşturduktan sonra biz de kaç toplam kayıt aracılığıyla havuzda ve satır dizini başlatmak ve en fazla satır değerleri BLL doğru geçirebilirsiniz belirleyebilmesi ObjectDataSource yapılandırmanız gerekir.

Özel disk belleği uygulama birkaç adımı gerektirir ve varsayılan disk belleği olarak değil neredeyse kadar basittir olsa da, özel disk belleği ile yeteri kadar büyük miktarlarda verinin sayfalama reddedilir. Sonuçları incelenmesi olarak gösterilen, özel disk belleği saniye ASP.NET sayfası işleme süresi dışına shed ve veritabanı sunucusu üzerindeki yükü tarafından bir veya daha fazla büyüklük rengini açabilirsiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Önceki](paging-and-sorting-report-data-vb.md)
[sonraki](sorting-custom-paged-data-vb.md)
