---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Özelleştirilmiş sıralama kullanıcı arabirimi (C#) oluşturma | Microsoft Docs
author: rick-anderson
description: Uzun listesini görüntüleyen veri sıralandığında için çok yararlı olabilir grup ilgili verileri satır ayırıcı sunarak. Bu öğreticide görüyoruz ye nasıl...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 28926276e183ae7cf7ed4087d8c73044741389e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754421"
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Özelleştirilmiş sıralama kullanıcı arabirimi (C#) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) veya [PDF olarak indirin](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Uzun listesini görüntüleyen veri sıralandığında için çok yararlı olabilir grup ilgili verileri satır ayırıcı sunarak. Bu öğreticide bir tür bir sıralama kullanıcı arabirimi oluşturmak nasıl göreceğiz.


## <a name="introduction"></a>Giriş

Ne zaman uzun listesini görüntüleyen izin ver sıralanmış veriler yalnızca birkaç sıralanmış sütunda farklı değerlerin bulunduğu bir son kullanıcı, sabit, fark sınırları tam olarak ortaya elde edilmesi güç bulabilirsiniz. Örneğin, veritabanı, ancak yalnızca dokuz farklı kategori seçenekleri 81 ürünü mevcuttur (sekiz benzersiz kategorileri artı `NULL` seçeneği). Deniz ürünleri kategorisi altında kalan ürünleri İnceleme ilgileniyor bir kullanıcının bir durum düşünün. Listeleyen bir sayfadan *tüm* sonuçları birlikte gruplamak kategoriye göre sıralamak için kendi en iyi sonucu olan kullanıcı ürünlerde tek bir GridView karar verebilirsiniz tüm birlikte Deniz ürünleri ürünleri. Kategoriye göre sıraladıktan sonra kullanıcı sonra listede, hunt burada Deniz ürünleri gruplandırılmış ürünleri başlangıç ve bitiş için arayan gerekir. Sonuçlar alfabetik olarak sıralanır beri Deniz ürünleri ürünleri bulma kategori adı zor değildir ancak yine de yakından kılavuzundaki öğelerin listesini tarama gerektirir.

Sıralanmış grupları arasındaki sınırları vurgulayın yardımcı olmak için birçok Web sitesi gibi gruplar arasındaki ayırıcı ekleyen bir kullanıcı arabirimi uyguluyor. Şekil 1'de gösterilen olanlar gibi ayırıcılar daha hızlı bir şekilde belirli bir grubun bulun ve sınırlarını tanımlamak, hem de farklı grupları verilerinde mevcut olmadığından emin olmak bir kullanıcı etkinleştirir.


[![Açıkça tanımlanmış her kategori grubudur](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Şekil 1**: her kategori grubu olduğunu açıkça tanımlanmış ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


Bu öğreticide bir tür bir sıralama kullanıcı arabirimi oluşturmak nasıl göreceğiz.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>1. adım: standart, sıralanabilir GridView oluşturma

GridView'ın gelişmiş sıralama arabirim sağlayacağı şekilde genişletmek nasıl inceleyeceğiz önce ilk olduğu ürünleri listeler standart, sıralanabilir GridView oluşturun s olanak tanır. Başlangıç açarak `CustomSortingUI.aspx` sayfasını `PagingAndSorting` klasör. GridView sayfasına ekleyin, kendi `ID` özelliğini `ProductList`ve için yeni bir ObjectDataSource bağlayın. ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProducts()` kayıtları seçmek için yöntemi.

Ardından, GridView yalnızca içerdiği gibi yapılandırma `ProductName`, `CategoryName`, `SupplierName`, ve `UnitPrice` BoundFields ve kullanımdan CheckBoxField. Son olarak, GridView GridView s akıllı etiket sıralama etkinleştir onay kutusunu işaretleyerek sıralama destekleyecek şekilde yapılandırma (veya ayarlayarak onun `AllowSorting` özelliğini `true`). Bu eklemeler yaptıktan sonra `CustomSortingUI.aspx` sayfasında, bildirim temelli biçimlendirme görünmelidir aşağıdakine benzer:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

İlerlememizin şimdiye kadarki bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Şekil 2 verilerini alfabetik sırada kategoriye göre sıralandı sıralanabilir GridView gösterilir.


[![Sıralanabilir GridView s veri kategoriye göre sıralanır.](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Şekil 2**: veri kategoriye göre sıralandığına sıralanabilir GridView s ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>2. adım: Teknikleri ayırıcı satır eklemek için keşfetmek

Genel, sıralanabilir GridView ile tam, kalan tek şey GridView önce her benzersiz sıralanmış Grup ayırıcı satır eklemek için. Ancak böyle satırları GridView yerleştirilebilir nasıl? Temelde, biz GridView s satırları yineleme yapmak için burada sıralanmış sütundaki değerleri arasındaki farklar meydana belirleyin ve ardından uygun ayırıcı satır ekleyin. Bu sorun hakkında düşünürken, çözüm yere GridView s'te kaynaklandığını doğal hatırlıyorum `RowDataBound` olay işleyicisi. Açıkladığımız gibi [özel biçimlendirme sırasında verileri](../custom-formatting/custom-formatting-based-upon-data-cs.md) öğretici, bu olay işleyicisi satır düzeyinde biçimlendirme uygulamak satır s verilere dayalı olduğunda yaygın olarak kullanılır. Ancak, `RowDataBound` olay işleyicisi değil çözüm burada satırları GridView'a program aracılığıyla bu olay işleyiciden olarak eklenemiyor. GridView s `Rows` koleksiyonu, aslında, salt okunur.

GridView'a ek satır eklemek için üç seçenek sunuyoruz:

- GridView'a bağlı gerçek veriler için bu meta veri ayırıcı satır ekleme
- GridView veriye bağlı sonra ek ekleyin `TableRow` GridView s örneklerine denetim koleksiyonu
- GridView denetiminde genişleten ve GridView s yapısı oluşturmaktan sorumlu bu yöntemleri geçersiz kılan bir özel sunucu denetimi oluşturma

Bu işlevi birçok web sayfalarında ister birkaç Web sitesini gerekiyorsa özel sunucu denetimi oluşturma en iyi yaklaşım olacaktır. Ancak, kod ve derinlikleri içine kapsamlı bir araştırma GridView s iç işleyişi olayın gerektirdiği. Bu nedenle, ki bu öğretici için bu seçeneği göz önünde.

Diğer iki gerçek veri olmanın ekleme ayırıcı satırları GridView'a bağlı ve sonra GridView s denetim koleksiyonu düzenleme seçenekleri, bağlı - sorun farklı saldırı ve tartışma olarak belirlenmiştir.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>GridView'a bağlı veriler için satırlar ekleme

GridView bir veri kaynağına bağlandığında, oluşturduğu bir `GridViewRow` veri kaynağı tarafından döndürülen her kayıt için. Bu nedenle, biz gerekli ayırıcı satır ayırıcı kayıtlar veri kaynağına ekleyerek GridView'a bağlama önce ekleyebilir. Şekil 3'te bu kavramı gösterir.


![Veri kaynağına ayırıcı satırlar ekleme bir yöntem içerir.](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Şekil 3**: bir teknik içeren veri kaynağına ayırıcı satırlar ekleme


Hiçbir özel ayırıcı kayıt olduğundan tırnak içine terimi ayırıcı kayıtları kullanmam; Bunun yerine, şu şekilde veri kaynağındaki belirli bir kaydı normal veri satırı yerine bir ayırıcı olarak hizmet veren bayrak gerekir. Örneklerimizde, biz yeniden bağlama için bir `ProductsDataTable` oluşur GridView örneğine `ProductRows`. Biz bir kaydı bir ayırıcı satır olarak ayarlayarak bayrak, `CategoryID` özelliğini `-1` (bir tür değeri'sine bağlanamadık t olduğundan normalde).

Bu yöntemi kullanmak için d biz aşağıdaki adımları tamamlamanız gerekir:

1. Program aracılığıyla GridView'a bağlamak için veri alma (bir `ProductsDataTable` örneği)
2. GridView s verileri sıralama `SortExpression` ve `SortDirection` özellikleri
3. Yinelemek `ProductsRows` içinde `ProductsDataTable`sıralanmış sütun farklılıkları burada yer alan aranırken
4. Her Grup sınırında bir ayırıcı kaydı ekleme `ProductsRow` DataTable s olan bir örneğine `CategoryID` kümesine `-1` (veya hangi ataması kayıt bir ayırıcı kaydı olarak işaretlemek için bağlı kararın)
5. Ayırıcı satır ekleme sonra program aracılığıyla verilere GridView'a bağlayın

Bu beş adımı yanı sıra d de bir olay işleyicisi için GridView s sağlamak için ihtiyacımız `RowDataBound` olay. Burada, biz d her denetleyin `DataRow` ve ayırıcı olup olmadığını belirlemek satır, bir olan `CategoryID` ayarı olan `-1`. Bu durumda, d büyük olasılıkla biçimlendirmesi veya hücre içinde görüntülenen metni ayarlama istiyoruz.

Ayrıca bir olay işleyicisi GridView s için sağlamanız gereken şekilde sıralama grubu sınırları ekleme, yukarıda özetlenen olandan biraz daha fazla iş gerektirir, bu tekniği kullanarak `Sorting` olay ve canlı izlemek `SortExpression` ve `SortDirection` değerleri.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>S denetim koleksiyonu sonraki GridView s düzenleme Databound bırakıldı

GridView'a bağlama önce verileri Mesajlaşma yerine, ayırıcı satırları ekleyebiliriz *sonra* veri GridView'a bağlandı. Veri bağlama işlemini olan gerçekte GridView s kontrol hiyerarşisinde yukarı, yapılar sadece bir `Table` örneği oluşan her biri hücrelerinin koleksiyonu oluşur koleksiyonunu satır,. Özellikle GridView s denetim koleksiyonu içeren bir `Table` kendi kök nesnede bir `GridViewRow` (sınıfından türetilen `TableRow` sınıfı) her kayıt için `DataSource` GridView'a bağlı ve `TableCell` her nesnesinde`GridViewRow` örneği her veri alanı için `DataSource`.

Oluşturulduktan sonra sıralama her grubu arasındaki ayırıcı satır eklemek için sizi doğrudan bu denetim hiyerarşisi işleyebilirsiniz. GridView s denetim hiyerarşisi sayfa işlenen zamanına göre son kez oluşturulduğundan emin olabilir. Bu nedenle, bu yaklaşım geçersiz kılmalar `Page` s sınıfı `Render` yöntemi, bu noktada GridView s son denetim hiyerarşisi gerekli ayırıcı satırları içerecek şekilde güncelleştirilir. Şekil 4, bu işlemi göstermektedir.


[![Alternatif bir yöntem GridView s denetim hiyerarşisi yönetir](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Şekil 4**: GridView s denetim hiyerarşisi alternatif bir yöntem yönetir ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


Bu öğreticide, sıralama kullanıcı deneyimini özelleştirmek için ikinci bu yaklaşımı kullanacağız.

> [!NOTE]
> Kod ı m sunan Bu öğreticide sağlanan örnek üzerinden hesaplanmıştır [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s blog girişine [GridView sıralama gruplandırma bir Bit yürütme](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>3. adım: GridView s denetimi hiyerarşi için ayırıcı satır ekleme

Yalnızca denetim hiyerarşisi oluşturulur ve bu sayfayı ziyaret son kez oluşturulan sonra ayırıcı satırları GridView s denetim hiyerarşiye eklemek istediğimiz olduğundan, sayfa yaşam döngüsü, ancak gerçek GridView c önce sonunda bu ekleme yapmak istiyoruz Tim hiyerarşi HTML'e işlenip. Başlangıçtan biz gerçekleştirmek bu son olası nokta `Page` s sınıfı `Render` biz aşağıdaki yöntem imzasını kullanarak bizim arka plan kod sınıfı Geçersiz kılabileceğiniz olay:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Zaman `Page` s özgün sınıfı `Render` yöntemi çağrıldığında `base.Render(writer)` her sayfadaki denetimleri, bunların denetimi hiyerarşi temelinde biçimlendirme oluşturma işlenir. Bu nedenle hem diyoruz emin olunması zorunludur `base.Render(writer)`, böylece sayfa oluşturulur ve biz GridView s işlemek denetim çağrılmadan önce hiyerarşi `base.Render(writer)`, böylece ayırıcı satırları GridView s denetim hiyerarşisi kendisinden önce eklenmiş s edilmiş çizilir.

Sıralama Grup üstbilgilerinde eklemesine ilk kullanıcı verileri sıralanması istedi sağlamak ihtiyacımız var. Varsayılan olarak, GridView s içeriği sıralanmaz ve bu nedenle biz t üstbilgileri sıralama herhangi bir grup girin gerek ki.

> [!NOTE]
> Sayfa ilk yüklendiğinde, belirli bir sütuna göre sıralanacak GridView istiyorsanız GridView s çağrı `Sort` yöntemi ilk sayfasını ziyaret edin (ancak, sonraki Geri göndermeler göre değil). Bunu gerçekleştirmek için bu çağrıda ekleme `Page_Load` olay işleyicisinin içerisinde bir `if (!Page.IsPostBack)` koşullu. Kiracıurl [sayfalama ve sıralama rapor verileri](paging-and-sorting-report-data-cs.md) daha fazla bilgi edinmek için öğretici bilgi `Sort` yöntemi.


Varsayılarak sıralanmış verileri, bizim sıradaki görev, hangi sütunun belirlemektir verileri sıralayan ve s sütunu farklılıkları arayan satır tarama değerler. Aşağıdaki kod, veri sıralandıktan ve bulduğu verileri sıralandıktan sütun sağlar:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

GridView varsa henüz olacak şekilde sıralamak GridView s `SortExpression` özellik ayarlanmamış. Bu nedenle, yalnızca bu özellik belirli bir değeri varsa, ayırıcı satır eklemek istiyoruz. Aksi halde sonraki ait verileri sıralanmıştır sütun dizini belirlemek ihtiyacımız var. Bu döngü GridView s gerçekleştirilir `Columns` koleksiyon, sütun için ayarlanmış arama `SortExpression` özelliğini eşittir GridView s `SortExpression` özelliği. Sütunu s dizinine ek olarak, biz de almak `HeaderText` ayırıcı satırları görüntülenirken kullanılan özellik.

Tarafından veri sıralama sütunu dizinde GridView satırları listeleme son adımdır. Her satır için biz sıralanmış sütun s değeri önceki satır s sıralanmış sütun s değerden farklı olup olmadığını belirlemeniz gerekir. Bu nedenle, yeni bir eklemesine ihtiyacımız olursa `GridViewRow` denetim hiyerarşiye örneği. Bu, aşağıdaki kod ile gerçekleştirilir:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Bu kod, programlı olarak başvuruda bulunarak başlatır `Table` GridView s denetim hiyerarşisi kökünde bulunan nesne ve adlı bir dize değişkeni oluşturma `lastValue`. `lastValue` Geçerli satır s sıralanmış sütun değeri önceki satır s değeri ile karşılaştırmak için kullanılır. Ardından, GridView s `Rows` koleksiyonu öğrenmeniz ve sıralanmış sütunun değeri depolanır her satır için `currentValue` değişkeni.

> [!NOTE]
> Hücre s kullanabilirim belirli bir satır s sıralanmış sütun değerini belirlemek için `Text` özelliği. Bu da BoundFields için çalışır ancak değil TemplateField, CheckBoxFields, istendiği gibi çalışması ve benzeri. Alternatif GridView alanlar için kısa bir süre sonra hesap nasıl göz atacağız.


`currentValue` Ve `lastValue` değişkenler ardından karşılaştırılır. Bunlar farklıysa denetim hiyerarşiye yeni bir ayırıcı satır eklemek ihtiyacımız var. Bu dizini belirleyerek gerçekleştirilir `GridViewRow` içinde `Table` s nesnesi `Rows` yeni oluşturma koleksiyonu `GridViewRow` ve `TableCell` örnekleri ve ardından ekleyerek `TableCell` ve `GridViewRow` için denetim hiyerarşisi.

Ayırıcı s silmenizin satır Not `TableCell` GridView genişliğinin tamamını kapsayan şekilde biçimlendirilmiş kullanılarak biçimlendirilir `SortHeaderRowStyle` CSS sınıfı ve kendi `Text` gibi sıralama Grup gösterir name özelliği (Kategori gibi) ve Grup s değeri (örneğin, İçecekler). Son olarak, `lastValue` değerine güncelleştirildi `currentValue`.

Sıralama üst bilgi satırı biçimlendirmek için kullanılan CSS sınıfının `SortHeaderRowStyle` belirtilmesi gerekiyor `Styles.css` dosya. Stil ayarları ne olursa olsun size itiraz kullanmaktan çekinmeyin; Aşağıdaki kullandım:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Geçerli koduyla tarafından herhangi bir BoundField sıralarken sıralama arabirimi sıralama grup üstbilgileri ekler (bkz: Şekil 5, üretici tarafından sıralarken bir ekran görüntüsünde gösterilmiştir). Ancak, herhangi bir alan türü (örneğin, bir CheckBoxField veya TemplateField) sıralama, sıralama Grup üstbilgilerinde saklanıyorsa (bkz. Şekil 6) bulunması uygulanır.


[![Sıralama arabirimi tarafından BoundFields sıralarken Sırala ve Gruplandır üst bilgiler içerir](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Şekil 5**: sıralama arabirimi içeren sıralama grubu üst bilgiler olduğunda sıralayarak BoundFields ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Eksik sıralama bir CheckBoxField sıralama Grup üstbilgilerinde olan](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Şekil 6**: sıralama Grup üstbilgilerinde olan eksik sıralama bir CheckBoxField ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


Kodu şu anda yalnızca kullandığı için sıralama grup üstbilgileri, bir CheckBoxField tarafından sıralarken yok sebebi `TableCell` s `Text` her satır için sıralanmış sütun değerini belirlemek için özellik. CheckBoxFields için `TableCell` s `Text` özelliği boş bir dize; Bunun yerine, değeri içinde bulunduğu bir onay kutusu Web denetimi aracılığıyla kullanılabilir `TableCell` s `Controls` koleksiyonu.

Alan türleri BoundFields dışında işlemek için kod genişletmek ihtiyacımız burada `currentValue` değişkenine bir onay kutusu varlığını denetlemek için atandığı `TableCell` s `Controls` koleksiyonu. Yerine `currentValue = gvr.Cells[sortColumnIndex].Text`, bu kodu aşağıdakiyle değiştirin:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Sıralanmış sütun bu kodu inceler `TableCell` herhangi bir denetim olup olmadığını belirlemek geçerli satıra `Controls` koleksiyonu. Vardır ve bir onay kutusu ilk denetimidir `currentValue` Evet veya Hayır, onay kutusu s bağlı olarak değişkeni ayarlanır `Checked` özelliği. Aksi takdirde, değerin alındığı yer `TableCell` s `Text` özelliği. Bu mantık, GridView içinde mevcut tüm TemplateField için sıralama işlemek üzere çoğaltılabilir.

Yukarıdaki kod eklenmesiyle sıralama grup üstbilgileri artık tarafından kullanımdan CheckBoxField sıralarken yok (bkz. Şekil 7).


[![Şimdi mevcut olduğunda sıralama bir CheckBoxField sıralama Grup üstbilgilerinde olan](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Şekil 7**: sıralama grubu üst bilgilerdir artık mevcut sıralama bir CheckBoxField ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> Ürünleriyle varsa `NULL` veritabanı değerlerini `CategoryID`, `SupplierID`, veya `UnitPrice` alanları, bu değerleri olarak görünür GridView boş dizeler ilebuürünlereyönelikayırıcısatırsmetinanlamıvarsayılan`NULL`değerler kategorisi gibi: (diğer bir deyişle, orada s kategori sonra adı yok: kategorisiyle ister: İçecekler). Burada görüntülenen bir değeri istiyorsanız ya da BoundFields ayarlayabilirsiniz [ `NullDisplayText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) görüntülenmesini istediğiniz metni ya da bir koşullu ifade atarken işleme yöntemi ekleyebilirsiniz `currentValue` ayırıcı için Satır s `Text` özelliği.


## <a name="summary"></a>Özet

GridView sıralama arabirimi özelleştirmeye yönelik pek çok yerleşik seçenek içermez. Bununla birlikte, alt düzey kod biraz, s GridView s denetim hiyerarşisi daha özelleştirilmiş bir arabirim oluşturmak için ince ayarlamalar yapmak mümkün. Bu öğreticide daha kolay farklı gruplar ve bu grupları sınırlar tanımlayan bir sıralanabilir GridView için bir sıralama Grup ayırıcı satır ekleme gördük. Özelleştirilmiş sıralama arabirimleri ek örnekler için kullanıma [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [sıralama birkaç ASP.NET 2.0 GridView ipuçları ve püf noktaları](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) blog girişi.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](sorting-custom-paged-data-cs.md)
> [İleri](paging-and-sorting-report-data-vb.md)
