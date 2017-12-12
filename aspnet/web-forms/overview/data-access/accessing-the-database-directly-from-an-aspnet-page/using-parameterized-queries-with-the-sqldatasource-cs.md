---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Parametreli sorgular ile SqlDataSource (C#) kullanarak | Microsoft Docs
author: rick-anderson
description: "Bu öğreticide SqlDataSource denetimi bizim bakma devam ve parametreli sorgular tanımlamak öğrenin. Parametrelerle belirtilen hem decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 7b32a664975254dcc1d015f2400df30d05346948
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Parametreli sorgular kullanmayı ile SqlDataSource (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) veya [PDF indirin](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> Bu öğreticide SqlDataSource denetimi bizim bakma devam ve parametreli sorgular tanımlamak öğrenin. Parametreleri bildirimli olarak ve programlı olarak belirtilebilir ve konumları querystring, oturum durumu, diğer denetimler ve gibi daha fazla sayıda çekebilir.


## <a name="introduction"></a>Giriş

Önceki öğreticide SqlDataSource denetimi doğrudan bir veritabanından veri almak için nasıl kullanılacağını gördük. Veri Kaynağı Yapılandırma Sihirbazı'nı kullanarak, biz veritabanı ve sonra da tercih edebilirsiniz: bir tablo veya Görünüm; döndürülecek olan sütunları seçin özel bir SQL deyimi girin; ya da bir saklı yordamı kullanın. Bir tablo veya Görünüm sütunları seçmek veya özel bir SQL deyimi girerek, SqlDataSource s denetimi olup olmadığını `SelectCommand` özelliği, sonuçta ortaya çıkan geçici SQL atandığında `SELECT` deyimi ve olduğundan bu `SELECT` , deyimi yürütülmesi SqlDataSource s `Select()` yöntemi çağrıldığında (programlı olarak veya otomatik olarak verilerden Web Denetimi).

SQL `SELECT` önceki öğretici s gösterileri olmadığı kullanılan ifadeleri `WHERE` yan tümceleri. İçinde bir `SELECT` deyimi, `WHERE` yan tümcesi, döndürülen sonuçları sınırlandırmak için kullanılabilir. Örneğin, birden fazla $50,00 maliyetlendirme ürünlerin adlarını görüntülemek için aşağıdaki sorguyu kullanıyoruz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

Genellikle, kullanılan değerler bir `WHERE` yan tümcesi bir sorgu dizesi değeri, oturum değişkeni ya da kullanıcı giriş sayfasında Web denetiminden gibi bazı dış kaynak tarafından belirleyin. İdeal olarak, kullanım yoluyla böyle girişleri belirtilen *parametreleri*. Microsoft SQL Server ile parametreleri kullanarak belirtilir `@parameterName`olarak içinde:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

Her ikisi için parametreli sorgular SqlDataSource destekleyen `SELECT` deyimleri ve `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Ayrıca, parametre değerlerini çeşitli kaynaklardan querystring, oturum durumu sayfadaki denetimleri otomatik olarak çekilen ve benzeri veya programlı olarak atanabilir. Bu öğreticide, bildirimli olarak ve program aracılığıyla parametreyi belirtmek için değerleri nasıl parametreli sorgular tanımlamak nasıl göreceğiz.

> [!NOTE]
> Önceki öğreticide aracımız tercih kendi kavramsal benzerlikler belirtmeye SqlDataSource ile ilk 46 öğreticileri üzerinden bırakıldı ObjectDataSource karşılaştırılan. Bu benzerlikler ayrıca parametreleri genişletir. İş mantığı katmanı yöntemlere giriş parametrelerini eşlenmiş ObjectDataSource s parametreleri. SqlDataSource ile parametreleri doğrudan SQL sorgu içinde tanımlanmıştır. Koleksiyonlar için parametrelerinin her iki denetimleriniz kendi `Select()`, `Insert()`, `Update()`, ve `Delete()` yöntemleri ve her ikisi de bu parametre değerlerini önceden tanımlanmış kaynaklardan (sorgu dizesi değerleri, oturum değişkenleri ve benzeri doldurulmuş olabilir ) veya programlı olarak atanmış.


## <a name="creating-a-parameterized-query"></a>Parametreleştirilmiş Sorgu Oluşturma

SqlDataSource denetim s veri kaynağı Yapılandırma Sihirbazı'nı veritabanına kayıtları almak için yürütülecek komut tanımlamak için üç seçenek sunar:

- Varolan bir tablo veya Görünüm sütunlarından seçerek
- Özel bir SQL deyimi girerek veya
- Saklı yordam seçerek

Varolan bir tabloyu veya görünümü, parametrelerini sütunlarından seçilmesinde `WHERE` Ekle yan tümcesinde belirtilen `WHERE` yan tümcesi iletişim kutusu. Ancak, özel bir SQL deyimi oluştururken, parametreleri doğrudan girebilirsiniz `WHERE` yan tümcesi (kullanarak `@parameterName` her bir parametreyi belirtmek için). A [saklı yordamı](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) bir veya daha fazla SQL deyimlerini, oluşur ve bu deyimleri parametreli olabilir. SQL deyimlerinde kullanılan parametreleri ancak, giriş parametreleri olarak saklı yordam geçirilmesi gerekir.

Parametreleştirilmiş sorgu oluşturma hakkında yönergeler bağlı olduğundan SqlDataSource s `SelectCommand` belirtilen, let s eylemleri göz hiç üç yaklaşımlar. Başlamak için açık `ParameterizedQueries.aspx` sayfasındaki `SqlDataSource` klasörünü SqlDataSource denetimini tasarımcıya araç çubuğuna sürükleyin ve ayarlayın, `ID` için `Products25BucksAndUnderDataSource`. Ardından, Denetim s akıllı etiket yapılandırma veri kaynağı bağlantısını tıklatın. Kullanmak istediğiniz veritabanını seçin (`NORTHWINDConnectionString`) ve İleri'yi tıklatın.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>1. adım: Ekleme bir`WHERE`sütunları bir tablo veya Görünüm seçilmesinde yan tümcesi

Veritabanı SqlDataSource denetimi ile döndürmek için verileri seçerken, veri kaynağı Yapılandırma Sihirbazı'nı yalnızca var olan bir tablo döndürür veya (bkz: Şekil 1) görüntülemek için sütunları çekme olanak tanır. Böylece otomatik olarak yapılması derlemeler SQL `SELECT` veritabanına gönderilen olan deyimi zaman SqlDataSource s `Select()` yöntemi çağrılır. Önceki öğreticide yaptığımız gibi Ürünler tablosuna aşağı açılan listeden seçin ve `ProductID`, `ProductName`, ve `UnitPrice` sütun.


[![Bir tablo veya Görünüm döndürülecek olan sütunları seçin](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Şekil 1**: bir tablo veya Görünüm dönmek sütunları seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Eklenecek bir `WHERE` yan tümcesinde `SELECT` deyimi, tıklatın `WHERE` Ekle getirir düğmesi `WHERE` yan tümcesi iletişim kutusu (bkz: Şekil 2). Tarafından döndürülen sonuçları sınırlandırmak için bir parametre eklemek için `SELECT` sorgu, öncelikle verileri göre filtre uygulamak için sütun seçin. Ardından, filtrelemek için kullanılacak işleci seçin (= &lt;, &lt;= &gt;, vb.). Son olarak, kaynağını parametreyi s gibi sorgu dizesi veya oturum durumundan seçin. İçine eklemek için Ekle düğmesini tıklatın parametresi yapılandırdıktan sonra `SELECT` sorgu.

Bu örnekte, let s yalnızca bu sonuçlar geri nerede `UnitPrice` değerdir $25.00 küçük veya buna eşit. Bu nedenle, çekme `UnitPrice` sütunun aşağı açılan listeden ve &lt;işleci aşağı açılan listeden =. Bir sabit kodlanmış parametre değeri (örneğin, $25.00) kullanırken veya programlı olarak belirtilmesi için parametre değeri geçerliyse, hiçbiri kaynak aşağı açılan listeden seçin. Ardından, sabit kodlanmış parametre değeri 25.00 değeri metin kutusuna girin ve Ekle düğmesine tıklayarak işlemi tamamlayın.


[![Döndürülen sonuçları sınırlandırmak ekleyin yan tümcesi iletişim kutusu](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Şekil 2**: Ekle sağlayıcıdan döndürülen sonuçları sınırlandırmak `WHERE` yan tümcesi iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Parametre ekledikten sonra için veri kaynağı Yapılandırma Sihirbazı'nı geri dönmek için Tamam'ı tıklatın. `SELECT` Sihirbazı'nın altındaki deyimi şimdi içermelidir bir `WHERE` yan tümcesinin adlı bir parametre ile `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> İçinde birden çok koşul belirtirseniz, `WHERE` Ekle from yan tümcesi `WHERE` yan tümcesi iletişim kutusu, sihirbazın birleştirir bunlarla `AND` işleci. Eklenecek gerekiyorsa bir `OR` içinde `WHERE` yan tümcesi (gibi `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) oluşturmak zorunda sonra `SELECT` deyimi özel SQL deyimi ekran aracılığıyla.


(Tıklatın ardından, daha sonra son) SqlDataSource yapılandırma işlemini tamamlayın ve SqlDataSource s bildirim temelli biçimlendirme inceleyin. Şimdi biçimlendirme içeren bir `<SelectParameters>` parametrelerinde kaynakları çıkışı harfe dönüştüren koleksiyonu `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Zaman SqlDataSource s `Select()` yöntemi çağrıldığında, `UnitPrice` parametre değeri (25.00) uygulanan `@UnitPrice` parametresinde `SelectCommand` veritabanına gönderilmeden önce. Yalnızca $25.00 küçük veya buna eşit sağlayıcıdan döndürülen ürünleri net sonucudur `Products` tablo. Bu, onaylayın eklemek GridView sayfasına, bu veri kaynağına bağlama ve tarayıcı üzerinden sayfa görüntülemek. Şekil 3 onaylar gibi $25.00, küçük veya buna eşit listelenen bu ürünleri yalnızca görmeniz gerekir.


[![Yalnızca bu ürünleri Less Than veya $25.00 eşit görüntülenir](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Şekil 3**: yalnızca bu ürünleri Less Than veya $25.00 eşit görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>2. adım: Özel SQL deyimi için parametre ekleme

Özel bir SQL deyimi eklerken girebilirsiniz `WHERE` yan tümcesi açıkça veya Sorgu Oluşturucusu filtre hücrede bir değer belirtin. Bunu göstermek için yalnızca bu ürünler, belirli bir eşiği değerinden fiyatlarıdır GridView görüntülemek s olanak tanır. Bir metin kutusuna eklemeye başlayın `ParameterizedQueries.aspx` sayfa kullanıcıdan Bu eşik değeri toplar. TextBox s ayarlamak `ID` özelliğine `MaxPrice`. Bir düğme Web denetimi ekleyin ve ayarlayın kendi `Text` görüntü eşleşen ürünlere özelliği.

Ardından, GridView sayfaya sürükleyin ve akıllı etiketten adlı yeni bir SqlDataSource oluşturmak için seçtiğiniz `ProductsFilteredByPriceDataSource`. Veri Kaynağı Yapılandırma Sihirbazı, bir özel bir SQL deyimi veya saklı yordam ekran belirt devam (Şekil 4'e bakın) ve şu sorguyu girin:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

(El ile veya Sorgu Oluşturucusu yoluyla) sorgu girdikten sonra İleri'yi tıklatın.


[![Yalnızca ürünleri değerinden küçük veya eşit bir parametre değerine döndürür](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Şekil 4**: iade yalnızca bu ürünleri Less Than veya eşit bir parametre değeri olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Sorgu parametreleri içeren olduğundan, sihirbazın sonraki ekranında bize parametreleri değerlerin kaynağı ister. Denetim parametresi kaynak aşağı açılan listeden seçin ve `MaxPrice` (TextBox denetimi s `ID` değeri) ControlId aşağı açılan listeden. Kullanıcı olmayan girmiş herhangi bir metin durumda kullanmak için bir isteğe bağlı varsayılan değerini de girebilirsiniz `MaxPrice` metin kutusu. Anda için varsayılan bir değer girmeyin.


[![Metin özelliği parametre kaynağı olarak kullanılan MaxPrice TextBox s](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Şekil 5**: `MaxPrice` TextBox s `Text` özelliği, parametrenin kaynağı olarak kullanılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Sonraki tıklayarak veri kaynağı Yapılandırma Sihirbazı'nı tamamlayın ve ardından son. GridView, metin kutusuna, düğme ve SqlDataSource için bildirim temelli biçimlendirme aşağıdaki gibidir:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Unutmayın SqlDataSource s içinde parametresi `<SelectParameters>` bölüm bir `ControlParameter`, gibi ek özellikler içeren `ControlID` ve `PropertyName`. Zaman SqlDataSource s `Select()` yöntemi çağrıldığında, `ControlParameter` belirtilen Web denetimi özelliği değerini alan ve karşılık gelen bir parametre atar `SelectCommand`. Bu örnekte, `MaxPrice` s metin özelliği olarak kullanılan `@MaxPrice` parametre değeri.

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. İlk sitesini ziyaret ettiğinde veya her `MaxPrice` TextBox hiçbir kayıt GridView görüntülenen bir değeri eksik.


[![Görüntülenen zaman MaxPrice metin kutusu boş olan hiçbir kayıtlarıdır](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Şekil 6**: Hayır kayıtlarıdır görüntülendiğinde `MaxPrice` metin kutusu boş olduğundan ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


Varsayılan olarak, bir veritabanına bir parametre değeri için boş bir dize dönüştürülür, hiç ürün gösterilen neden olduğundan `NULL` değeri. Bu yana karşılaştırmasını `[UnitPrice] <= NULL` her zaman değerlendirir False, hiçbir sonuç döndürmedi.

Textbox 5.00 gibi içine bir değer girin ve görüntü uyan ürünleri düğmesine tıklayın. Geri göndermede, parametre kaynaklarının birinin GridView değişti SqlDataSource bildirir. Sonuç olarak, eşit veya daha az bu ürünler için 5.00 görüntüleme SqlDataSource için GridView rebinds.


[![Ürünler Less Than veya $5.00 eşit görüntülenir](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Şekil 7**: ürünleri Less Than veya $5.00 eşit görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Başlangıçta tüm ürünleri görüntüleme

Sayfa ilk kez yüklendiğinde hiç ürün görüntülemek yerine, biz görüntülemek isteyebilirsiniz *tüm* ürünler. Tüm ürünleri listelemek için tek yönlü zaman `MaxPrice` metin kutusu boş parametre s varsayılan değeri bazı insanely yüksek değer olarak ayarlamak için 1000000 gibi bu yana s Northwind Traders herhangi bir zamanda sahip stoğu, birim fiyat olası 1.000.000 aşıyor. Ancak, bu yaklaşım shortsighted ve diğer durumlarda çalışmayabilir.

Önceki öğreticileri içinde- [bildirim temelli parametreleri](../basic-reporting/declarative-parameters-cs.md) ve [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) ile benzer bir sorunla karşılaştığı. Bu mantığı iş mantığı katmanı yerleştirilecek var. bizim çözüm bulunuyordu. Özellikle, gelen değeri BLL incelenmesi ve yazılmışsa `NULL` veya bazı ayrılmış değeri, tüm kayıtları döndürülen DAL yöntemi çağrısı yönlendirildi. Gelen değeri normal bir filtre değeri varsa, bir parametreli kullanılan bir SQL deyimi yürütülen DAL yöntemine bir çağrı yapıldı `WHERE` yan tümcesiyle birlikte sağlanan değer.

Ne yazık ki, biz mimarisi SqlDataSource kullanırken atlama. Bunun yerine, tüm kayıtlar varsa akıllıca şablonlarınızdan SQL deyimini özelleştirmek için ihtiyacımız `@MaximumPrice` parametresi `NULL` veya ayrılmış bir değer. Bu alıştırmada, let s sahip, bu nedenle olması durumunda `@MaximumPrice` parametresidir eşittir `-1.0`, ardından *tüm* kayıtları olan döndürülecek (`-1.0` hiçbir ürün negatif olabileceğiiçinayrılmışbirdeğerolarakçalışır`UnitPrice`değeri). Bunu gerçekleştirmek için aşağıdaki SQL deyimini kullanabilirsiniz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Bu `WHERE` yan tümcesi döndürür *tüm* ise kayıtlar `@MaximumPrice` parametresi eşittir `-1.0`. Parametre değeri değilse `-1.0`, yalnızca ürünleri, `UnitPrice` küçük veya eşittir `@MaximumPrice` parametre değeri döndürülür. Varsayılan değer olarak ayarlayarak `@MaximumPrice` parametresi `-1.0`, ilk sayfa yükleme (veya her `MaxPrice` TextBox boşsa), `@MaximumPrice` bir değeri olur `-1.0` ve tüm ürünleri görüntülenir.


[![Tüm ürünleri artık görüntülenen zaman MaxPrice metin kutusu boş değil](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Şekil 8**: şimdi tüm ürünleri olan görüntülendiğinde `MaxPrice` metin kutusu boş olduğundan ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Bu yaklaşımda Not Uyarılar birkaç vardır. İlk olarak, parametre s veri türü tarafından algılanır fark SQL sorgusunda s kullanımı. Değiştirirseniz `WHERE` from yan tümcesi `@MaximumPrice = -1.0` için `@MaximumPrice = -1`, çalışma zamanı parametresi tamsayı olarak değerlendirir. Ardından atamayı denerseniz `MaxPrice` (gibi 5.00) ondalık bir değeri metin kutusuna, bir hata 5.00 tamsayıya dönüştürülemiyor çünkü gerçekleşir. Bu sorunu gidermek için ya da kullandığınızdan emin olun `@MaximumPrice = -1.0` içinde `WHERE` yan tümcesi veya, daha iyi henüz ayarlayın `ControlParameter` s nesnesi `Type` ondalık özelliği.

Ekleyerek İkincisi, `OR @MaximumPrice = -1.0` için `WHERE` yan tümcesi, sorgu altyapısı kullanamaz dizin üzerinde `UnitPrice` (bir varsayılarak var), böylece bir tablo taraması sonuçlanır. Kayıtları yeterince çok sayıda varsa performansı etkileyebilir `Products` tablo. Bu mantık bir saklı yordam için taşımak için daha iyi bir yaklaşım olabilir burada bir `IF` deyimi gerçekleştirir ya da bir `SELECT` sorgu `Products` olmadan tablo bir `WHERE` tüm kayıtları döndürülecek gerektiğinde yan tümcesinde veya bir olan `WHERE` yan tümcesi içeriyor yalnızca `UnitPrice` ölçütleri, böylece bir dizini kullanılabilir.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>3. adım: Oluşturma ve parametreli saklı yordamları kullanma

Saklı yordamlar sonra kullanılabilir giriş parametreleri kümesini saklı yordam içinde tanımlanan SQL deyim dahil edebilirsiniz. Giriş parametreleri kabul eden bir saklı yordamı kullanmak için SqlDataSource yapılandırırken, bu parametre değerlerini geçici SQL deyimlerini gibi ile aynı teknikler kullanılarak belirtilebilir.

Let s SqlDataSource saklı yordamları kullanma göstermek için adlı Northwind veritabanına yeni bir saklı yordam oluşturma `GetProductsByCategory`, adlı bir parametre kabul eden `@CategoryID` ve tüm ürünlerin sütunları döndürür, `CategoryID` Sütun eşleşen `@CategoryID`. Bir saklı yordam oluşturmak için Sunucu Gezgini gidin ve ayrıntılarına `NORTHWND.MDF` veritabanı. (T tan Sunucu Gezgini görürseniz, bu Görünüm menüsüne giderek ve Sunucu Gezgini seçeneği ortaya çıkarmak.)

Gelen `NORTHWND.MDF` veritabanı, saklı yordamlar klasörü sağ tıklatın, ekleme yeni saklı yordam seçin ve aşağıdaki söz dizimini girin:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Saklı yordam kaydetmek için Kaydet simgesine (veya Ctrl + S) tıklayın. Saklı yordamlar klasöründen sağ tıklayıp yürütme seçerek saklı yordamı test edebilirsiniz. Bu saklı yordam s parametrelerini ister (`@CategoryID`, bu örnekte), sonra sonuçları, çıktı penceresinde görüntülenir.


[![GetProductsByCategory saklı yordamı ile çalıştırıldığında bir @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Şekil 9**: `GetProductsByCategory` saklı yordam ile çalıştırıldığında bir `@CategoryID` 1 ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


GridView içinde Meşrubat kategorideki tüm ürünleri görüntülemek için bu saklı yordamı kullanın s olanak tanır. Sayfaya yeni GridView ekleyin ve adlı yeni bir SqlDataSource bağlama `BeverageProductsDataSource`. Özel bir SQL deyimi veya saklı yordam ekran belirt devam, saklı yordam radyo düğmesini seçin ve çekme `GetProductsByCategory` saklı yordamı aşağı açılan listeden.


[![GetProductsByCategory seçin saklı yordamı aşağı açılan listeden](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Şekil 10**: seçin `GetProductsByCategory` saklı yordamı aşağı açılan listeden ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Saklı yordam giriş parametresi kabul bu yana (`@CategoryID`), İleri'yi tıklatmadan, ister bize bu parametreyi s kaynağı belirtin. Meşrubat `CategoryID` 1, bu nedenle parametre kaynak aşağı açılan listesi None bırakın ve 1 DefaultValue metin kutusuna girin.


[![Ürünleri Meşrubat kategorisinde döndürmek için 1 sabit kodlanmış değeri kullanın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Şekil 11**: Meşrubat kategorisinde ürünleri döndürmek için 1 Hard-Coded değerini kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Saklı yordam, SqlDataSource s kullanırken aşağıdaki gösterildiği gibi bildirim temelli biçimlendirme, `SelectCommand` özelliğini saklı yordamın adını ayarlayın ve [ `SelectCommandType` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) ayarlanır `StoredProcedure`, belirten `SelectCommand` geçici SQL deyimi yerine bir saklı yordam adıdır.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Bir tarayıcıda sayfası sınayın. Ancak Meşrubat kategorisine ait ürünleri görüntülenir *tüm* çarpımını itibaren alanları görüntülenir `GetProductsByCategory` saklı yordam tüm sütunları döndürür `Products` tablo. Biz, doğal olarak, sınırlamak veya GridView GridView s sütunları Düzenle iletişim kutusunda görüntülenen alanları özelleştirebilir.


[![Tüm Meşrubat görüntülenir](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Şekil 12**: tüm Meşrubat görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>4. adım: Program aracılığıyla SqlDataSource s çağırma`Select()`deyimi

Örnekler biz önceki öğretici ve Bu öğreticide şu ana kadar görülen ve doğrudan bir GridView SqlDataSource denetimleri bağlı. SqlDataSource denetimi s veri ancak, program aracılığıyla erişilebilen ve kodda numaralandırılır. Bunu incelemek için verileri sorgulamak gerekir, ancak tan t gereksinim görüntülemek bu özellikle yararlı olabilir. Tüm ortak veritabanına bağlanmak için komutu belirtin ve sonuçları almak için ADO.NET kod yazmaya gerek yerine bu ve sıkıcı kod işlemek SqlDataSource izin verebilirsiniz.

SqlDataSource s çalışmak göstermek için yöneticinizden rastgele seçilen bir kategoriye ve ilişkili ürünlerinden adını görüntüleyen bir web sayfası oluşturmak için bir istekle yaklaşmışsa verileri programlı olarak düşünün. Bir kullanıcı bu sayfayı ziyaret ettiğinde, diğer bir deyişle, rastgele bir kategori seçin istiyoruz `Categories` tablo, kategori adı görüntülemek ve ardından bu kategoriye ait ürünleri listeler.

Rastgele bir kategori şablonlarınızdan iki SqlDataSource denetimi bir ihtiyacımız bunu gerçekleştirmek için `Categories` ve kategori s ürünleri almak için başka bir tablo. Biz bu adımda bir rastgele kategorisi kayıt alan SqlDataSource yapı; 5. adım, kategori s ürünleri alır SqlDataSource hazırlayın adresindeki arar.

Bir SqlDataSource eklemeye başlayın `ParameterizedQueries.aspx` ve kendi `ID` için `RandomCategoryDataSource`. Aşağıdaki SQL sorgusunu kullanır şekilde yapılandırabilirsiniz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()`rastgele düzende sıralanmış kayıtları döndürür (bkz [kullanma `NEWID()` rastgele sıralama kayıtlara](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1`ilk kaydı sonuç kümesinden döndürür. Bu sorgunun döndürdüğü araya `CategoryID` ve `CategoryName` tek, rasgele seçilen kategori sütun değerleri.

S kategori görüntülenecek `CategoryName` değer, bir etiket Web denetimi sayfasına eklemesine kendi `ID` özelliğine `CategoryNameLabel`ve temizleyin, `Text` özelliği. Program aracılığıyla SqlDataSource denetimden veri almak için çağrılacak ihtiyacımız kendi `Select()` yöntemi. [ `Select()` Yöntemi](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sqldatasource.select.aspx) türünde tek bir giriş parametresi bekliyor [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.aspx), nasıl veri döndürülen önce messaged belirtir. Bu verileri sıralama ve filtreleme hakkında yönergeler içerebilir ve sıralama veya bir SqlDataSource denetimi verilerden aracılığıyla disk belleği Web denetimleri verileri tarafından kullanılır. Bizim örneğimizde rağmen biz güncelleştireceğinizi döndürülen önce değiştirilecek t gerek veri ve bu nedenle de geçer `DataSourceSelectArguments.Empty` nesnesi.

`Select()` Yöntemi uygulayan bir nesne döndürür `IEnumerable`. Kesin türü döndürdü bağlıdır SqlDataSource denetimi s değerini [ `DataSourceMode` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Önceki öğreticide açıklandığı gibi bu özellik için herhangi bir değer ayarlanabilir `DataSet` veya `DataReader`. Varsa kümesine `DataSet`, `Select()` yöntemi döndürür bir [DataView](https://msdn.microsoft.com/en-us/library/01s96x0z.aspx) nesne; kümesine IF `DataReader`, uygulayan bir nesne döndürür [ `IDataReader` ](https://msdn.microsoft.com/en-us/library/system.data.idatareader.aspx). Bu yana `RandomCategoryDataSource` SqlDataSource sahip kendi `DataSourceMode` özelliğini `DataSet` (varsayılan), biz DataView nesnesi ile çalışacaksınız.

Aşağıdaki kod kayıtları almak nasıl gösterir `RandomCategoryDataSource` SqlDataSource DataView olarak nasıl okunacağını yanı sıra `CategoryName` ilk DataView satırdan sütun değeri:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]`ilk döndürür `DataRowView` DataView. `randomCategoryView[0]["CategoryName"]`değerini döndürür `CategoryName` bu ilk satırı sütun. DataView geniş yazılmış olduğuna dikkat edin. Belirli bir sütun değeri başvurmak için sütun adına (Bu durumda, CategoryName) dize olarak geçirmek ihtiyacımız. Şekil 13 gösterir görüntülenen iletisi `CategoryNameLabel` sayfayı görüntülerken. Elbette, görüntülenen gerçek kategori adı rastgele tarafından seçilen `RandomCategoryDataSource` SqlDataSource (Geri göndermeler dahil) sayfası her ziyaret.


[![Adı görüntülenir rastgele seçilen kategori s](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Şekil 13**: rastgele seçilen kategori s adı görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> S SqlDataSource denetimi `DataSourceMode` özelliği şekilde ayarlanmış `DataReader`, gelen bir dönüş değeri `Select()` yöntemi gerektiği için dönüştürmek `IDataReader`. Okunacak `CategoryName` sütun değeri ilk satırdan, biz d kodu gibi kullanın:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

SqlDataSource ile bir kategori biz kategori s ürünleri listeler GridView eklemeye hazır re rastgele seçme.

> [!NOTE]
> Kategori s adını görüntülemek için bir etiket Web denetimi kullanmak yerine FormView veya DetailsView sayfasına SqlDataSource bağlama ekledik. Etiket kullanma, ancak, bize program aracılığıyla SqlDataSource s çağırmak nasıl keşfetmek izin verilen `Select()` deyimi ve kodda, sonuçta elde edilen verilerle çalışma.


## <a name="step-5-assigning-parameter-values-programmatically"></a>5. adım: Program aracılığıyla parametre değerler atama

Tüm örneklerin biz Bu öğreticide şu ana kadar görülen ve sabit kodlanmış parametre değeri ya da (bir sorgu dizesi değer, bir Web denetimi sayfası ve benzeri) önceden tanımlanmış parametre kaynaklardan biri alınan bir kullanmış. Ancak, SqlDataSource denetim s parametrelerini ayrıca program aracılığıyla ayarlanabilir. Geçerli örneğimizde tamamlamak için tüm belirtilen bir kategoriye ait ürünleri döndüren bir SqlDataSource ihtiyacımız var. Bu SqlDataSource sahip bir `CategoryID` parametre değeri ayarlanması gereken temel alarak `CategoryID` tarafından döndürülen sütun değeri `RandomCategoryDataSource` içinde SqlDataSource `Page_Load` olay işleyicisi.

Başlangıç sayfasına GridView ekleyerek ve adlı yeni bir SqlDataSource bağlama `ProductsByCategoryDataSource`. Adım 3'te yaptığımız gibi çok, böylece bu çağırır SqlDataSource yapılandırın `GetProductsByCategory` saklı yordamı. Parametre kaynak aşağı açılan liste kümesi None olarak bırakır, ancak bu varsayılan değer programlı olarak ayarlı şekilde bir varsayılan değer girmeyin.


[![Bir parametre kaynağı veya varsayılan değer belirtmeyin](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Şekil 14**: Parametre kaynağı veya varsayılan değer belirtmeyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


SqlDataSource Sihirbazı'nı tamamladıktan sonra sonuçta elde edilen bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Biz atayabilirsiniz `DefaultValue` , `CategoryID` program aracılığıyla giriş parametresi `Page_Load` olay işleyicisi:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Bu eklenmesiyle sayfa rastgele seçili kategoriyle ilişkili ürünleri gösteren GridView içerir.


[![Bir parametre kaynağı veya varsayılan değer belirtmeyin](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Şekil 15**: Parametre kaynağı veya varsayılan değer belirtmeyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Özet

SqlDataSource sayfa geliştiricileri parametre değerleri sabit kodlanmış, önceden tanımlanmış parametre kaynaklardan çekilen veya program aracılığıyla atanan parametreli sorgular tanımlamanızı sağlar. Bu öğreticide nasıl geçici SQL sorguları ve saklı yordamlar için veri kaynağı Yapılandırma Sihirbazı'ndan parametreli bir sorgu oluşturabilir gördük. Biz de sabit kodlanmış parametre kaynakları, bir parametre kaynağı olarak bir Web denetimi kullanarak ve program aracılığıyla parametre değeri belirterek arama.

Gibi ObjectDataSource ile SqlDataSource de temel alınan verileri değiştirmek için yetenekleri sağlar. Sonraki öğreticide tanımlamak üzere nasıl tümleştirildiği incelenmektedir `INSERT`, `UPDATE`, ve `DELETE` SqlDataSource ifadelerle. Bu deyimler eklendikten sonra biz ekleme, düzenleme ve silme GridView, DetailsView ve FormView denetimlere devralınmış özellikleri yerleşik kullanabilirler.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Scott Clyde, Randell Etikan ve Ken Pespisa yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](querying-data-with-the-sqldatasource-control-cs.md)
[sonraki](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
