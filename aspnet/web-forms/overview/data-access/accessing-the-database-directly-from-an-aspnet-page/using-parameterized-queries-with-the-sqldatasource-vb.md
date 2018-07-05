---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: (VB) SqlDataSource ile parametreli sorgular kullanma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, bizim göz SqlDataSource denetimi devam ve parametreli sorgular tanımlamayı öğrenin. Parametrelerle belirtilen her iki decla...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: fd6361d7b8981b7caee996ff3af165416af860a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395492"
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>(VB) SqlDataSource ile parametreli sorgular kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) veya [PDF olarak indirin](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> Bu öğreticide, bizim göz SqlDataSource denetimi devam ve parametreli sorgular tanımlamayı öğrenin. Parametreleri bildirimli ve programlı olarak belirtilebilir ve sorgu dizesi, oturum durumu, diğer denetimler ve diğer konumlara bir dizi çekilebilir.


## <a name="introduction"></a>Giriş

Önceki öğreticide SqlDataSource denetimi doğrudan bir veritabanından veri almak için nasıl kullanılacağını gördük. Veri Kaynağı Yapılandırma Sihirbazı'nı kullanarak, biz veritabanını ve ardından ya da tercih edebilirsiniz: bir tablo veya görünümden; döndürülecek olan sütunları seçin özel bir SQL deyimi girin. veya bir saklı yordamı kullanın. Bir tablo veya görünümden sütunların seçilmesi veya özel bir SQL ifadesi girerek, SqlDataSource s denetimi olup olmadığını `SelectCommand` özelliği, sonuçta ortaya çıkan geçici SQL atandığı `SELECT` deyimi ve bunu, bu `SELECT` deyimi yürütülmesi SqlDataSource s `Select()` yöntemi çağrılır (programlı olarak veya otomatik olarak bir veri Web Denetimi).

SQL `SELECT` önceki öğretici s tanıtımları koduk kullanılan ifadeleri `WHERE` yan tümceleri. İçinde bir `SELECT` deyimi `WHERE` yan tümcesi, döndürülen sonuçlar sınırlamak için kullanılabilir. Örneğin, birden fazla $50.00 maliyeti ürünlerin adlarını görüntülemek için aşağıdaki sorguyu kullanabiliriz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

Genellikle, kullanılan değerler bir `WHERE` yan tümcesi bir sorgu dizesi değeri, oturum değişkeni veya bir Web denetimi sayfasında kullanıcı girdisinden gibi bazı dış kaynak tarafından belirlemek. İdeal olarak, bu girişleri kullanımının belirtilir *parametreleri*. Microsoft SQL Server ile parametreleri kullanarak belirtilir `@parameterName`, olarak:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

SqlDataSource parametreli sorgular için her ikisini de destekler `SELECT` deyimleri ve `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Ayrıca, parametre değerlerini otomatik olarak çeşitli kaynaklardan sorgu dizesi, oturum durumu sayfadaki denetimleri çekilebilir ve benzeri veya programlı bir şekilde atanabilir. Bu öğreticide, parametreli sorgular bildirimli ve programlı olarak parametreyi belirtmek için değerleri nasıl tanımlama göreceğiz.

> [!NOTE]
> Önceki öğreticide biz aracımızı tercih ettiğiniz kavramsal benzerlikleri belirtmeye SqlDataSource ile ilk 46 öğreticiler üzerinden kaldırıldı ObjectDataSource karşılaştırması. Bu benzerlikler parametreleri de genişletin. İş mantığı katmanı yöntemleri için giriş parametrelerini eşlenen ObjectDataSource s parametreleri. SqlDataSource ile parametreler doğrudan SQL sorgu içinde tanımlanır. Koleksiyonlar için parametrelerinin her iki denetim sahip kullanıcıların `Select()`, `Insert()`, `Update()`, ve `Delete()` yöntemleri ve her ikisi de (sorgu dizesi değerleri, oturum değişkenleri ve benzeri önceden tanımlanmış kaynaktan doldurulan bu parametrelere sahip olabilir ) ya da programlı olarak atanır.


## <a name="creating-a-parameterized-query"></a>Parametreleştirilmiş Sorgu Oluşturma

SqlDataSource denetimi s veri kaynağı Yapılandırma Sihirbazı'nı veritabanına kayıtları almak için çalıştırılacak komutu tanımlamaya yönelik üç seçenek sunar:

- Varolan bir tablo veya Görünüm sütunlarından seçerek
- Özel bir SQL ifadesi girerek veya
- Bir saklı yordam seçerek

Var olan bir tablo veya görünüm, parametrelerini sütunlarından alınırken `WHERE` Ekle yan belirtilen `WHERE` yan tümce iletişim kutusu. Ancak, özel bir SQL deyimini oluştururken parametreleri doğrudan girebilirsiniz `WHERE` yan tümcesi (kullanarak `@parameterName` her parametre göstermek için). A [saklı yordamı](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) bir veya daha fazla SQL deyimleri oluşur ve bu deyimler parametreli olabilir. SQL deyimlerinde kullanılan parametreler ancak, giriş parametreleri olarak saklı yordamı geçirilmelidir.

Parametreleştirilmiş sorgu oluşturma hakkında yönergeler bağlı olduğundan SqlDataSource s `SelectCommand` belirtilen, let s eylemleri göz hiç üç yaklaşım. Başlamak için açık `ParameterizedQueries.aspx` sayfasını `SqlDataSource` klasöründe SqlDataSource denetimi Tasarımcısı araç kutusundan sürükleyin ve ayarlayın, `ID` için `Products25BucksAndUnderDataSource`. Ardından, Denetim s akıllı etiket yapılandırmak veri kaynağı bağlantısından tıklayın. Kullanmak istediğiniz veritabanını seçin (`NORTHWINDConnectionString`) ve İleri'ye tıklayın.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>1. adım: Ekleme bir`WHERE`sütunlar bir tablo veya görünümden alınırken yan tümcesi

SqlDataSource denetimi ile veritabanından döndürmek için verileri seçerken, veri kaynağı Yapılandırma Sihirbazı'nı yalnızca var olan bir tablodan veya (bkz. Şekil 1) görüntülemek için sütunları çekme olanak sağlıyor. SQL'yi yedeklemek derlemeler otomatik olarak yapılması `SELECT` veritabanına gönderilen deyimi olduğunda SqlDataSource s `Select()` yöntemi çağrılır. Önceki öğreticide yaptığımız gibi ürünler tablosu aşağı açılan listeden seçin ve `ProductID`, `ProductName`, ve `UnitPrice` sütun.


[![Bir tablo veya görünümden döndürülecek olan sütunları seçin](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Şekil 1**: bir tablo veya Görünüm dönmek sütunları seçin ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


Eklenecek bir `WHERE` yan tümcesinde `SELECT` deyimi, tıklayın `WHERE` Ekle getiren düğmesi `WHERE` yan tümce iletişim kutusu (bkz: Şekil 2). Tarafından döndürülen sonuçları sınırlandırmak için bir parametre eklemek için `SELECT` sorgulama, önce verileri filtrelemek için sütun seçin. Ardından, filtreleme için kullanılacak işleci seçin (= &lt;, &lt;= &gt;, vb.). Son olarak, kaynak parametre s değerinin gibi sorgu dizesi veya oturum durumu seçin. Parametre yapılandırdıktan sonra projeyi eklemek için Ekle düğmesini tıklatın `SELECT` sorgu.

Bu örnekte, let s yalnızca bu sonuçları döndürür burada `UnitPrice` değerdir $25.00 küçüktür veya eşittir. Bu nedenle, çekme `UnitPrice` sütunun aşağı açılan listeden ve &lt;işleci aşağı açılan listeden =. Bir sabit kodlanmış bir parametre değeri (örneğin $25.00) kullanırken veya programlı olarak belirtilen parametre değeri ise, hiçbir kaynak aşağı açılan listeden seçin. Ardından, sabit kodlanmış parametre değerini 25.00 değeri metin kutusuna girin ve Ekle düğmesine tıklayarak işlemi tamamlayın.


[![Öğesinden döndürülen sonuçları sınırlandırmak ekleyin yan tümce iletişim kutusu](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Şekil 2**: Ekle sağlayıcıdan döndürülen sonuçları sınırlandırmak `WHERE` yan tümce iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


Parametre ekledikten sonra için veri kaynağı Yapılandırma Sihirbazı'nı dönmek için Tamam'ı tıklatın. `SELECT` Sihirbazı'nın altındaki deyimi artık içermelidir bir `WHERE` adlı bir parametre yan tümcesinde `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Birden çok koşulu belirtirseniz `WHERE` Ekle from yan tümcesi `WHERE` yan tümce iletişim kutusunda, Sihirbazı birleşimler kendileriyle `AND` işleci. Dahil etmek gerekiyorsa bir `OR` içinde `WHERE` yan tümcesi (gibi `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) oluşturmak sahip `SELECT` deyimi aracılığıyla özel SQL deyimi ekran.


SqlDataSource (tıklatın ardından, daha sonra son) yapılandırma tamamlayın ve ardından SqlDataSource s bildirim temelli biçimlendirme inceleyin. Artık biçimlendirme içeren bir `<SelectParameters>` kaynakları parametreleri için kullanıma harfe dönüştüren koleksiyonunun `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Zaman SqlDataSource s `Select()` yöntemi çağrılır, `UnitPrice` parametre değeri (25.00) uygulanan `@UnitPrice` parametresinde `SelectCommand` veritabanına gönderilmeden önce. Yalnızca gelen döndürülür $25.00 küçüktür veya eşittir ürünleri net sonucudur `Products` tablo. Bu, onaylayın eklemek GridView sayfasına, bu veri kaynağına bağlama ve ardından sayfanın bir tarayıcı aracılığıyla görüntüleyin. Şekil 3 onaylar gibi $25.00, küçük veya ona eşit olan, listelenen bu ürünlerin yalnızca görmeniz gerekir.


[![Yalnızca bu ürünleri Less Than ya da $25.00 eşit görüntülenir](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Şekil 3**: yalnızca bu ürünleri Less Than veya $25.00 eşit görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>2. adım: Bir özel SQL deyimine parametreler ekleme

Özel bir SQL deyimi eklerken girebilirsiniz `WHERE` yan tümcesi açıkça veya Sorgu Oluşturucu filtre hücresi içinde bir değer belirtin. Bunu göstermek için bu ürünlerin, belirli bir eşiği değerinden fiyatlarıdır GridView görüntülemek s olanak tanır. Başlangıç TextBox'a ekleyerek `ParameterizedQueries.aspx` kullanıcıdan Bu eşik değeri toplamak için sayfa. TextBox s ayarlamak `ID` özelliğini `MaxPrice`. Bir düğme Web denetimi ekleyin ve ayarlayın, `Text` görünen eşleşen ürünlere özelliği.

Adlı yeni bir SqlDataSource oluşturmak, akıllı etiketi seçin ve ardından, GridView sayfaya sürükleyin `ProductsFilteredByPriceDataSource`. Veri Kaynağı Yapılandırma Sihirbazı'ndan özel bir SQL deyimi veya saklı yordam ekran devam etmek için belirtin (bkz: Şekil 4) ve aşağıdaki sorguyu girin:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Sorgu (el ile veya Sorgu Tasarımcısı aracılığıyla) girdikten sonra İleri'ye tıklayın.


[![Yalnızca bu ürünlerin değerinden küçük veya eşit bir parametre değeri döndürür](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Şekil 4**: iade yalnızca bu ürünleri Less Than veya bir parametre değeri eşittir ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


Sorgu parametreleri içerdiğinden, sihirbazın sonraki ekranında bize parametre değerlerinin kaynağı ister. Parametre kaynak aşağı açılan listeden denetimini seçin ve `MaxPrice` (s TextBox denetimi `ID` değeri) ControlId aşağı açılan listeden. Kullanıcı olmayan girmiş herhangi bir metin durumunda kullanmak için bir isteğe bağlı varsayılan değerini de girebilirsiniz `MaxPrice` metin. Şimdilik, varsayılan değer girmeyin.


[![Metin özelliği MaxPrice TextBox s parametresi kaynağı olarak kullanılır](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Şekil 5**: `MaxPrice` TextBox s `Text` özelliği, parametre kaynağı olarak kullanılır ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


Sonraki tıklayarak veri kaynağı Yapılandırma Sihirbazı'nı tamamlamak ve son. GridView, metin, düğme ve SqlDataSource için bildirim temelli biçimlendirme aşağıdaki gibidir:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Unutmayın SqlDataSource s içindeki parametre `<SelectParameters>` bölüm bir `ControlParameter`, gibi ek özellikler içeren `ControlID` ve `PropertyName`. Zaman SqlDataSource s `Select()` yöntemi çağrılır, `ControlParameter` belirtilen Web denetimi özellik değerini Dallarınızla ve karşılık gelen parametre atar `SelectCommand`. Bu örnekte, `MaxPrice` s metin özelliği olarak kullanılan `@MaxPrice` parametre değeri.

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Sayfa ilk ziyaret edildiğinde veya her `MaxPrice` TextBox kayıt GridView içinde görüntülenen bir değeri eksik.


[![Hiçbir kayıt görüntülenen zaman MaxPrice metin kutusu boş olduğunu gösterir.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Şekil 6**: No kayıtlardır görüntülendiğinde `MaxPrice` metin kutusu boş olduğundan ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


Varsayılan olarak, bir veritabanına bir parametre değeri için boş bir dize dönüştürülür çünkü ürün gösterilen sebebi `NULL` değeri. Yana karşılaştırmasını `[UnitPrice] <= NULL` değerlendirilir her zaman False sonuç döndürülür.

5.00 gibi metin kutusu içine bir değer girin ve görüntü eşleşen ürünleri düğmesine tıklayın. Geri göndermede, parametre kaynaklarının birinin GridView değişti SqlDataSource bildirir. Sonuç olarak, eşit veya ondan daha az bu ürünler için 5.00 görüntüleme SqlDataSource için GridView rebinds.


[![Ürünleri Less Than ya da eşit $ 5.00 görüntülenir](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Şekil 7**: ürünleri Less Than ya da eşit $ 5.00 görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Başlangıçta tüm ürünleri görüntüleme

Sayfa ilk yüklendiğinde ürün görüntülemek yerine, biz görüntülemek isteyebilirsiniz *tüm* ürünleri. Tüm ürünler listelemek için tek yönlü zaman `MaxPrice` metin kutusu boş parametre s varsayılan değeri bazı ait, İnanılmaz derecede yüksek değerine ayarlamaktır 1000000 gibi olduğundan s Northwind Traders hiç sahip sayım gerçekleştireceğini emin olan birim fiyatı olası 1.000.000 aşıyor. Ancak, bu yaklaşım shortsighted ve diğer durumlarda çalışmayabilir.

Önceki öğreticilerde - [bildirim temelli parametreler](../basic-reporting/declarative-parameters-vb.md) ve [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) biz benzer bir soruna karşılaştığı. Bu mantık iş mantığı katmanı koymak Çözümümüzü var. oluştu. Özellikle, gelen değeri BLL incelenir ve bu durumda `NULL` veya bazı ayrılmış değeri, tüm kayıtları döndüren DAL yöntemine çağrı yönlendirildi. Gelen bir değer normal bir filtre değeri varsa, kullanılan bir parametreli bir SQL deyimi yürütülen DAL yönteme bir çağrı yapıldı `WHERE` yan tümcesiyle birlikte sağlanan değer.

Ne yazık ki, biz mimarisi SqlDataSource kullanırken atlama. Bunun yerine, akıllı bir şekilde, tüm kayıtları almak için SQL deyimini özelleştirme ihtiyacımız `@MaximumPrice` parametresi `NULL` veya ayrılmış bir değer. Bu alıştırma için let s sahip, bu nedenle olması durumunda `@MaximumPrice` parametresi eşittir `-1.0`, ardından *tüm* kayıtlarını olan döndürülecek (`-1.0` hiçbir ürün negatifolabileceğiiçinayrılmışbirdeğerolarakçalışır`UnitPrice`değeri). Bunu gerçekleştirmek için aşağıdaki SQL deyimini kullanabilirsiniz:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

Bu `WHERE` yan tümcesi döndürür *tüm* ise kayıtlar `@MaximumPrice` parametresi eşittir `-1.0`. Parametre değeri değilse `-1.0`, yalnızca bu ürünlerin, `UnitPrice` küçüktür veya eşittir `@MaximumPrice` parametre değeri döndürülür. Varsayılan değerini ayarlayarak `@MaximumPrice` parametresi `-1.0`, ilk sayfa yüklenmesinden üzerinde (veya herhangi bir zamanda `MaxPrice` metin kutusu boşsa), `@MaximumPrice` değerine sahip `-1.0` ve tüm ürünleri görüntülenir.


[![Tüm ürünler artık görüntülenen zaman MaxPrice metin kutusu boş olduğundan](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Şekil 8**: artık tüm ürünler olan görüntülendiğinde `MaxPrice` metin kutusu boş olduğundan ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


Bu yaklaşımı izleme konusunda dikkat edilecek uyarılar birkaç vardır. İlk olarak, parametre s veri türü tarafından algılanır fark SQL sorgusunda s kullanım. Değiştirirseniz `WHERE` from yan tümcesi `@MaximumPrice = -1.0` için `@MaximumPrice = -1`, çalışma zamanı parametresi tamsayı olarak değerlendirir. Ardından atamayı denerseniz `MaxPrice` metin bir ondalık değere (5.00 gibi) bir hata tamsayıya 5.00 geçiremeyeceğiniz gerçekleşir. Bu sorunu gidermek için ya da kullandığınızdan emin olun `@MaximumPrice = -1.0` içinde `WHERE` yan tümcesi veya, daha iyi henüz ayarlayın `ControlParameter` s nesnesi `Type` onlu özelliği.

Ekleyerek İkincisi, `OR @MaximumPrice = -1.0` için `WHERE` yan tümcesi, sorgu altyapısı kullanamaz dizin üzerinde `UnitPrice` (bir varsayılarak var), böylece bir tablo tarama sonucu. Yeterince çok sayıda kayıtlar varsa bu performansı etkileyebilir `Products` tablo. Bu mantık için bir saklı yordam taşımak için daha iyi bir yaklaşım olacaktır burada bir `IF` deyimi gerçekleştirir ya da bir `SELECT` sorgu `Products` olmadan tablo bir `WHERE` tüm kayıtları döndürülecek gerektiğinde yan tümcesi veya bir olan `WHERE` yan tümcesi içeren yalnızca `UnitPrice` ölçütleri, böylece bir dizin kullanılabilir.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>3. adım: Oluşturma ve parametreli saklı yordamları kullanma

Saklı yordamlar ardından kullanılabilir giriş parametreleri kümesini saklı yordam içinde tanımlanan SQL deyim dahil edebilirsiniz. SqlDataSource giriş parametrelerini kabul eden bir saklı yordam kullanmak için yapılandırırken, bu parametre değerleri gibi geçici SQL deyimleri ile aynı teknikleri kullanılarak belirtilebilir.

Let s SqlDataSource içinde saklı yordamları kullanarak göstermek için Northwind veritabanına adlı yeni bir saklı yordam oluşturma `GetProductsByCategory`, adlı bir parametre kabul eden `@CategoryID` ve tüm ürünlerin sütunları döndürür, `CategoryID` Sütun eşleşen `@CategoryID`. Bir saklı yordam oluşturmak için Sunucu Gezginine geçin ve detayına `NORTHWND.MDF` veritabanı. (T don Sunucu Gezgini görürseniz, bu Görünüm menüsüne giderek ve Sunucu Gezgini seçeneğini belirleyerek ortaya çıkarmak.)

Gelen `NORTHWND.MDF` veritabanı, saklı yordamlar klasörü sağ tıklatın, yeni saklı yordam Ekle öğesini seçin ve aşağıdaki söz dizimini girin:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Saklı yordam kaydetmek için Kaydet simgesine (veya Ctrl + S) tıklayın. Saklı yordam saklı yordamlar klasöründen sağ tıklayıp Yürüt'i seçerek test edebilirsiniz. Bu saklı yordam s parametreleri isteyecektir (`@CategoryID`, bu örnekte), sonra sonuçları, çıkış penceresinde görüntülenir.


[![GetProductsByCategory saklı yordamı ile yürütüldüğünde bir @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Şekil 9**: `GetProductsByCategory` saklı yordamı ile yürütüldüğünde bir `@CategoryID` 1 ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


GridView İçecekler kategorisindeki tüm ürünleri görüntülemek için bu saklı yordamı kullanın. s olanak tanır. Sayfaya yeni GridView ekleyin ve adlı yeni bir SqlDataSource bağlamak `BeverageProductsDataSource`. Özel bir SQL deyimi veya saklı yordam ekran belirleme için devam, saklı yordam radyo düğmesini seçin ve çekme `GetProductsByCategory` saklı yordamı aşağı açılan listeden.


[![GetProductsByCategory seçin saklı yordamı aşağı açılan listeden](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Şekil 10**: seçin `GetProductsByCategory` saklı yordam aşağı açılan listeden ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


Saklı yordam giriş parametresi kabul eden bu yana (`@CategoryID`), İleri'ye tıklama, ister bize bu parametreyi s kaynağı belirtin. İçecekler `CategoryID` 1, bu nedenle parametre kaynak aşağı açılan listesi None bırakın ve 1 DefaultValue metin kutusuna girin.


[![İçecekler kategorisindeki ürünlerin döndürmek için sabit kodlanmış bir 1 değerini kullanın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Şekil 11**: İçecekler kategorisindeki ürünlerin döndürülecek Hard-Coded 1 değerini kullanın ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


SqlDataSource s bir saklı yordam kullanırken aşağıdaki gösterildiği gibi bildirim temelli biçimlendirme, `SelectCommand` özelliği, saklı yordam adı için ayarlanır ve [ `SelectCommandType` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) ayarlanır `StoredProcedure`gösteren `SelectCommand` geçici SQL deyimi yerine bir saklı yordam adıdır.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Bir tarayıcıda sayfası test edin. Ancak İçecekler kategoriye ait ürünleri görüntülenir *tüm* çarpımını beri alanlar görüntülenir `GetProductsByCategory` saklı yordam tüm sütunları döndürür `Products` tablo. Biz, Elbette sınırlamak veya GridView GridView s sütunları Düzenle iletişim kutusunda görüntülenen alanları özelleştirebilirsiniz.


[![Tüm İçecekler görüntülenir](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Şekil 12**: tüm İçecekler görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>4. adım: Program aracılığıyla SqlDataSource s çağırma`Select()`deyimi

Örnekler ediyoruz ve önceki öğreticide ve Bu öğretici şu ana kadar görülen SqlDataSource denetimi doğrudan GridView'a bağlı. SqlDataSource denetimi s veri ancak program aracılığıyla erişilebilen ve kodda numaralandırılır. Verileri sorgulamak, incelemek için gereken, ancak rsquo gereksinim görüntülemek bu özellikle yararlı olabilir. Tüm ortak veritabanına bağlanmak için komutu belirtin ve sonuçlarını almak için ADO.NET kod yazmak zorunda kalmak yerine işlemek ve sıkıcı bu kodu SqlDataSource izin verebilirsiniz.

SqlDataSource s ile çalışma göstermek için Patronunuzdan rastgele seçilen bir kategoriye ve onun ilişkili ürün adını görüntüleyen bir web sayfası oluşturmak için bir istekle yaklaşmışsa verileri programlı olarak düşünün. Bir kullanıcı bu sayfayı ziyaret ettiğinde, diğer bir deyişle, rastgele bir kategori seçmek istediğimiz `Categories` tablo, kategori adını görüntüler ve ardından bu kategoriye ait ürünleri listeler.

Bunu gerçekleştirmek için rastgele bir kategoriden almak için iki SqlDataSource denetimi bir gerekiyor `Categories` tablo ile diğeri s ürün kategorisini almak için. Bu adımda bir rastgele kategori kaydı alır SqlDataSource oluşturacağız; 5. adım, kategori s ürünleri alır SqlDataSource hazırlayın arar.

Başlamak için bir SqlDataSource ekleyerek `ParameterizedQueries.aspx` ve kendi `ID` için `RandomCategoryDataSource`. Aşağıdaki SQL sorgusunu kullanmasını sağlayacak şekilde yapılandırın:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` rastgele düzende sıralanmış kayıtları döndürür (bkz [kullanma `NEWID()` rastgele sıralama kayıtlara](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` sonuç kümesinden ilk kaydı döndürür. Bu sorgunun döndürdüğü araya `CategoryID` ve `CategoryName` tek, rasgele seçilen kategorisinden sütun değerleri.

Kategori s görüntülenecek `CategoryName` değer, sayfa için bir etiket Web denetimi ekleyin, kendi `ID` özelliğini `CategoryNameLabel`ve temizleyin, `Text` özelliği. Program aracılığıyla SqlDataSource denetimden verileri almak için çağrılacak ihtiyacımız kendi `Select()` yöntemi. [ `Select()` Yöntemi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) türünde tek bir giriş parametresi bekliyor [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), nasıl veri döndürülmeden önce messaged belirtir. Bu verileri sıralama ve filtreleme hakkında yönergeler içerebilir ve verileri sıralama veya verileri SqlDataSource denetimi ile sayfalama Web denetimleri tarafından kullanılır. Bizim örneğimizde, biz ki döndürülmeden önce değiştirilecek t gerek veri ve, bu nedenle geçer `DataSourceSelectArguments.Empty` nesne.

`Select()` Yöntemini uygulayan bir nesne döndürür `IEnumerable`. Kesin türü döndürülen değerin SqlDataSource denetimi s bağlıdır [ `DataSourceMode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Önceki öğreticide açıklandığı gibi bu özellik değerlerinden birine ayarlanabilir `DataSet` veya `DataReader`. Varsa kümesine `DataSet`, `Select()` yöntemi döndürür bir [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) nesne; Eğer kümesine `DataReader`, uygulayan bir nesne döndürür [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Bu yana `RandomCategoryDataSource` SqlDataSource sahip kendi `DataSourceMode` özelliğini `DataSet` (varsayılan), biz bir DataView nesnesi ile çalışacaksınız.

Aşağıdaki kod nasıl kayıtlardan alınacağını göstermektedir `RandomCategoryDataSource` SqlDataSource DataView olarak nasıl okunacağını yanı sıra `CategoryName` ilk DataView satırdaki sütun değeri:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` ilk döndürür `DataRowView` DataView. `randomCategoryView(0)("CategoryName")` değerini döndürür `CategoryName` ilk satırda sütun. DataView geniş yazılmış olduğuna dikkat edin. Belirli bir sütun değerine başvurmak üzere sütun adını bir dize (Bu durumda, CategoryName) olarak geçirilecek ihtiyacımız var. Şekil 13 gösterir görüntülenen ileti `CategoryNameLabel` sayfayı görüntülerken. Elbette, görüntülenen gerçek kategori adı rastgele seçim `RandomCategoryDataSource` SqlDataSource (Geri göndermeler dahil) sayfası her ziyaret.


[![Rastgele seçilen kategori s adı görüntülenir](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Şekil 13**: rastgele seçilen kategori s adı görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> SqlDataSource denetimi s `DataSourceMode` özelliği belirlenen `DataReader`, dönüş değeri `Select()` yöntemi gerekli başvurusuna `IDataReader`. Okunacak `CategoryName` ilk satırında, biz d sütun değerini kod gibi kullanın:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

SqlDataSource ile bir kategori biz kategori s ürünleri listeler GridView eklemek için hazır re rastgele seçme.

> [!NOTE]
> Kategori adı görüntülemek için bir etiket Web denetimi kullanmak yerine FormView veya DetailsView sayfaya SqlDataSource için bağlama ekledik. Bir etiket kullanmayı, ancak program aracılığıyla SqlDataSource s çağırmak nasıl keşfetmek olduk `Select()` deyimi ve sonuç verilerini kod ile çalışma.


## <a name="step-5-assigning-parameter-values-programmatically"></a>5. adım: Parametre değerlerini programlı olarak atama

Tüm örneklerin biz Bu öğreticide şu ana kadar görülen ve kullanmış parametresi sabit kodlu değer veya (bir sorgu dizesi değeri, bir Web denetim sayfası ve benzeri) önceden tanımlanmış bir parametre kaynaklardan birinden alınan bir. Ancak, SqlDataSource denetimi s parametreleri ayrıca programlı olarak ayarlanabilir. Geçerli Örneğimizdeki tamamlamak için tüm ürünlerin belirli bir kategoriye ait bir döndüren bir SqlDataSource ihtiyacımız var. Bu SqlDataSource sahip bir `CategoryID` parametre değeri ayarlanması gereken temel alarak `CategoryID` tarafından döndürülen sütun değeri `RandomCategoryDataSource` SqlDataSource içinde `Page_Load` olay işleyicisi.

Başlangıç sayfasına GridView ekleyerek ve adlı yeni bir SqlDataSource bağlamak `ProductsByCategoryDataSource`. Adım 3'te yaptığımız gibi çok, böylece çağırdığı SqlDataSource yapılandırma `GetProductsByCategory` saklı yordamı. Parametre kaynak açılır listede yok olarak bırakın, ancak bu varsayılan değer programlı olarak ayarlayacağız gibi varsayılan bir değer girmeyin.


[![Bir parametre kaynağı veya varsayılan değeri belirtmeyin](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Şekil 14**: Parametre kaynağı veya varsayılan değer belirtmezseniz ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


SqlDataSource sihirbazını tamamladıktan sonra ortaya çıkan bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

Biz atayabilirsiniz `DefaultValue` , `CategoryID` parametresi program `Page_Load` olay işleyicisi:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Bu eklenmesiyle, rasgele seçilen kategori ile ilişkili ürünleri gösteren GridView sayfası içerir.


[![Bir parametre kaynağı veya varsayılan değeri belirtmeyin](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Şekil 15**: Parametre kaynağı veya varsayılan değer belirtmezseniz ([tam boyutlu görüntüyü görmek için tıklatın](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>Özet

SqlDataSource parametre değerleri sabit kodlanmış, önceden tanımlanmış bir parametre kaynaklardan alınıp veya program aracılığıyla atanan parametreli sorgular tanımlamak sayfa geliştiricileri sağlar. Bu öğreticide hem geçici SQL sorguları ve saklı yordamlar için veri kaynağı Yapılandırma Sihirbazı'ndan parametreli bir sorgu çalışıyorlardı nasıl gördük. Ayrıca parametresi sabit kodlanmış kaynakları, bir parametre kaynağı olarak bir Web denetimi kullanarak ve parametre değeri programlı olarak belirtme incelemiştik.

Gibi ObjectDataSource ile SqlDataSource ayrıca temel alınan verileri değiştirmek için özellikler sunar. Sonraki öğreticide tanımlamak üzere nasıl tümleştirildiği incelenmektedir `INSERT`, `UPDATE`, ve `DELETE` SqlDataSource ile deyimleri. Bu deyimler ekledikten sonra biz ekleme, düzenleme ve özellikler için FormView GridView ve DetailsView denetimlerini devralınan silme yerleşik kullanabilir.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler, Ken Pespisa Scott Clyde ve Randell Etikan yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](querying-data-with-the-sqldatasource-control-vb.md)
> [İleri](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
