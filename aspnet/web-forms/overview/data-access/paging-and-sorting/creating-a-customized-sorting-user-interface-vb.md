---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Özelleştirilmiş sıralama kullanıcı arabirimi (VB) oluşturma | Microsoft Docs
author: rick-anderson
description: Uzun listesini görüntüleyen veri sıralandığında için çok yararlı olabilir grup ilgili verileri ayırıcı satırları sunarak. Bu öğreticide göreceğiz proxy nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 35144907aceda6ece56d91b24aba15ef951ed99a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892679"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>Özelleştirilmiş sıralama kullanıcı arabirimi (VB) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) veya [PDF indirin](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Uzun listesini görüntüleyen veri sıralandığında için çok yararlı olabilir grup ilgili verileri ayırıcı satırları sunarak. Bu öğreticide bir böyle bir sıralama kullanıcı arabirimi oluşturma göreceğiz.


## <a name="introduction"></a>Giriş

Ne zaman uzun listesini görüntüleyen izin ver sıralanmış veri sıralanmış sütundaki farklı değerleri sayıda bulunduğu bir son kullanıcı, tam olarak fark sınırları gerçekleştiği keşfedilir zor bulmak. Örneğin, veritabanı, ancak yalnızca dokuz farklı kategori seçimler 81 ürünlerinde vardır (sekiz benzersiz kategorileri artı `NULL` seçeneği). Deniz ürünleri kategorisi altında kalan ürünleri inceleniyor isteyen kullanıcı durumunu göz önünde bulundurun. Listeler bir sayfadan *tüm* tek GridView ürünler, kullanıcının kendi en iyi sonucu olan sonuçları birlikte gruplandırır kategoriye göre sıralamak için karar verebilirsiniz birlikte Deniz ürünleri ürünlerin tamamı. Kategoriye göre sıralama sonra kullanıcı ardından listede ilerleyin hunt burada Deniz ürünleri gruplandırılmış ürünleri başlangıç ve bitiş için arayan gerekir. Sonuçları alfabetik sıraya göre sıralanır beri Deniz ürünleri ürünleri bulma kategori adı zor değil, ancak hala yakından kılavuzunda öğeleri listesi tarama gerektirir.

Sıralanmış grupları arasındaki sınırların vurgulayın yardımcı olmak için bu grupları arasında ayırıcı ekler bir kullanıcı arabirimi birçok Web sitesi kullanın. Şekil 1'de gösterilen olanlar gibi ayırıcılar daha hızlı bir şekilde belirli bir grubun bulmak ve sınırlarını tanımlamak, yanı sıra verileri hangi ayrı grubu mevcut olmadığından emin olmak bir kullanıcı sağlar.


[![Her kategori açıkça tanımlanmış grubudur.](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Şekil 1**: her kategori grubu olduğunu açıkça tanımlanmış ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


Bu öğreticide bir böyle bir sıralama kullanıcı arabirimi oluşturma göreceğiz.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>1. adım: standart, sıralanabilir GridView oluşturma

Biz Gelişmiş sıralama arabirimi sağlamak için önce standart, sıralanabilir GridView oluşturan s izin GridView büyütmek üzere nasıl keşfedin önce ürünleri listeler. Başlangıç açarak `CustomSortingUI.aspx` sayfasındaki `PagingAndSorting` klasör. GridView sayfasına ekleme, Ayarla kendi `ID` özelliğine `ProductList`ve için yeni bir ObjectDataSource bağlayın. ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProducts()` kayıtları seçme yöntemi.

Ardından, GridView yalnızca içeriyor gibi yapılandırma `ProductName`, `CategoryName`, `SupplierName`, ve `UnitPrice` BoundFields ve kullanımdan CheckBoxField. Son olarak, GridView s akıllı etiket sıralama etkinleştir onay kutusunu işaretleyerek sıralama desteklemek için GridView yapılandırın (veya ayarlayarak kendi `AllowSorting` özelliğine `true`). Bu eklemeler yaptıktan sonra `CustomSortingUI.aspx` sayfasında, bildirim temelli biçimlendirme görünmelidir aşağıdakine benzer:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Bizim ilerleme bugüne kadarki bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Şekil 2 verilerini alfabetik sırada kategoriye göre sıralanır sıralanabilir GridView gösterir.


[![Sıralanabilir GridView s veri kategoriye göre sıralanır](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Şekil 2**: veri kategoriye göre sıralanmış sıralanabilir GridView s ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>2. adım: Ayırıcı satır eklemek teknikleri keşfetme

Genel, sıralanabilir GridView ile tam, kalan tek şey her benzersiz sıralanmış Grup önce GridView ayırıcı satır eklemek için. Ancak bu tür satırları GridView nasıl yerleştirilebilir? Esas olarak, biz GridView s satırları yinelemek için sıralanmış sütundaki değerleri arasındaki farklar gerçekleştiği belirlemek ve ardından uygun ayırıcı satır ekleyin. Bu sorun hakkında düşünürken, çözüm yere GridView s arasındadır doğal görünüyor `RowDataBound` olay işleyicisi. Biz anlatıldığı gibi [özel biçimlendirme göre bağlı verileri](../custom-formatting/custom-formatting-based-upon-data-vb.md) öğretici, bu olay işleyicisi satır düzeyi biçimlendirme uygulama satır s verileri temel alan, yaygın olarak kullanılan. Ancak, `RowDataBound` olay işleyicisi değil çözüm burada satırları GridView program aracılığıyla bu olay işleyiciden olarak eklenemiyor. GridView s `Rows` koleksiyonu, aslında, salt okunur.

GridView ek satır eklemek için şu üç seçeneğiniz vardır:

- Bu meta verileri ayırıcı satırlar GridView bağlı gerçek veri ekleyin
- GridView veriye bağlı sonra ek eklemek `TableRow` GridView s örneklerine denetim koleksiyonu
- GridView denetimini genişleten ve GridView s yapısı oluşturmaktan sorumlu olan yöntemlerini geçersiz kılar özel sunucu denetimi oluşturma

Bu işlev birçok web sayfalarında veya birkaç Web siteleri arasında gerekli ise özel sunucu denetimi oluşturma en iyi yaklaşımı olabilir. Ancak, oldukça biraz kod ve derinliklerine içine kapsamlı araştırması GridView s iç işleyişi gerektirdiği. Bu nedenle, biz Bu öğretici için bu seçeneği ele alacağız değil.

Gerçek veri olmanın ekleme ayırıcı satırları seçenekleri diğer iki GridView bağlı ve sonra GridView s denetim koleksiyonu düzenleme onun bağlı - sorun farklı saldırı ve tartışma belirlenmiştir.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>GridView bağlı satır veri ekleme

GridView bir veri kaynağına bağlandığında oluşturduğu bir `GridViewRow` veri kaynağı tarafından döndürülen her kayıt için. Bu nedenle, gerekli ayırıcı satırları GridView bağlama önce veri kaynağına ayırıcı kayıtları ekleyerek ekliyoruz. Şekil 3'te bu kavramı gösterir.


![Veri kaynağına ayırıcı satır ekleme bir tekniği içerir](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Şekil 3**: bir tekniği içerir ayırıcı satırlar için veri kaynağı ekleme


Özel ayırıcı kaydı olduğundan tırnak içine terim ayırıcı kayıtları kullanmam; Bunun yerine, biz şekilde veri kaynağı belirli bir kayıtta normal veri satırı yerine bir ayırıcı olarak hizmet veren bayrak gerekir. Örneklerde, biz re bağlama için bir `ProductsDataTable` oluşur GridView örneğine `ProductRows`. Biz kayıt ayırıcı satır olarak ayarlayarak bayrak, `CategoryID` özelliğine `-1` (bir tür değeri değeri t olduğundan normal olarak).

Bu teknik faydalanmak için d aşağıdaki adımları gerçekleştirmek ihtiyacımız var:

1. Program aracılığıyla GridView bağlamak için veri alma (bir `ProductsDataTable` örneği)
2. GridView s göre verileri sıralamak `SortExpression` ve `SortDirection` özellikleri
3. Yinelemek `ProductsRows` içinde `ProductsDataTable`, burada sıralanmış sütun farklılıkları kalan arayanlar
4. Her Grup sınırında bir ayırıcı kayıt ekleme `ProductsRow` DataTable s sahip bir örneğine `CategoryID` kümesine `-1` (veya ne olursa olsun ataması kayıt ayırıcı kaydı olarak işaretlemek için bağlı kararın)
5. Ayırıcı satırları injecting sonra program aracılığıyla verileri GridView Bağla

Beş adımları yanı sıra d ayrıca GridView s'için bir olay işleyicisi sağlamak ihtiyacımız `RowDataBound` olay. Burada, biz d her denetleyin `DataRow` ve ayırıcı olup olmadığını belirlemek satır, bir olan `CategoryID` ayarı varmış `-1`. Bu durumda, d büyük olasılıkla biçimlendirmesi veya hücre görüntülenen metni ayarlama istiyoruz.

Ayrıca bir olay işleyicisi için GridView s sağlamak gerek duyduğunuz sıralama grubu sınırları injecting, yukarıda özetlenen olandan biraz daha fazla iş gerektirir, bu teknik kullanılarak `Sorting` olay ve canlı izlemek `SortExpression` ve `SortDirection` değerleri.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>S denetim koleksiyonu sonraki GridView s düzenleme veriye bağlı edildi

GridView bağlama önce verileri Mesajlaşma yerine, ayırıcı satırları ekleyebiliriz *sonra* veri GridView bağlı. Veri bağlama işlemini olan gerçekte GridView s denetim hiyerarşisinde yukarı, derlemeler yalnızca bir `Table` örneği her biri hücrelerinin koleksiyonu oluşur koleksiyonu satır, oluşur. Özellikle, GridView s denetim koleksiyonu içeren bir `Table` kendi kök nesnede bir `GridViewRow` (türetilen `TableRow` sınıfı) her kayıt için `DataSource` GridView için bağlı ve `TableCell` her nesnesinde`GridViewRow` her veri alanı örneği `DataSource`.

Oluşturulduktan sonra sıralama her grubu arasındaki ayırıcı satır eklemek için sizi doğrudan bu denetim hiyerarşisi yönetebilirsiniz. GridView s denetim hiyerarşisi için en son ne zaman sayfa işlendiği zaman tarafından oluşturulan emin olabilir. Bu nedenle, bu yaklaşım geçersiz kılmaları `Page` s sınıfı `Render` yöntemi, bu noktada GridView s son denetim hiyerarşisi gerekli ayırıcı satırları içerecek şekilde güncelleştirilir. Şekil 4, bu işlemi gösterilmektedir.


[![Alternatif bir teknik GridView s denetim hiyerarşisi yönetir](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Şekil 4**: alternatif bir teknik GridView s denetim hiyerarşisi yönetir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


Bu öğretici için sıralama kullanıcı deneyimini özelleştirmek için bu ikinci yaklaşımı kullanacağız.

> [!NOTE]
> Kod ı m Bu öğreticide sunan temel sağlanan örnek [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s blog girişi, [biraz GridView sıralama gruplandırma ile çalma](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>3. adım: ayırıcı satırları GridView s denetim hiyerarşisi ekleme

Biz yalnızca kendi denetim hiyerarşisi oluşturduktan ve en son ne zaman bu sayfayı ziyaret için oluşturulan sonra ayırıcı satırları GridView s denetim hiyerarşisi eklemek istediğiniz olduğundan, bu eklenmesini sonunda sayfa yaşam döngüsünün ancak gerçek GridView c önce gerçekleştirme istiyoruz Tim hiyerarşi HTML'e işlenip işlenmediğini. Hangi biz gerçekleştirmek bu son olası nokta `Page` s sınıfı `Render` bizim arka plan kodu sınıfında aşağıdaki yöntemi imzası kullanarak geçersiz kılabilirsiniz olay:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Zaman `Page` s özgün sınıfı `Render` yöntemi çağrıldığında `base.Render(writer)` her sayfasındaki denetimleri, kendi denetim hiyerarşisi temel alınarak biçimlendirme oluşturma işlenir. Bu nedenle hem diyoruz, kesinlik temelli `base.Render(writer)`sayfa işlendiği böylece ve biz GridView s işlemek denetimi hiyerarşi çağrılmadan önce `base.Render(writer)`, böylece ayırıcı satırları önceki GridView s denetim hiyerarşisi eklendi s edilmiş çizilir.

Sıralama grup üstbilgileri eklemesine biz öncelikle kullanıcı verileri sıralanması istedi emin olmalısınız. Varsayılan olarak, GridView s içeriği sıralanmaz ve bu nedenle biz t üstbilgileri sıralama herhangi bir grup girin gerek güncelleştireceğinizi.

> [!NOTE]
> Sayfa ilk kez yüklendiğinde belirli bir sütuna sıralanacak GridView istiyorsanız GridView s çağrı `Sort` yöntemi ilk sayfasını ziyaret edin (ancak, sonraki Geri göndermeler üzerinde değil). Bunu gerçekleştirmek için bu çağrıyı ekleyin `Page_Load` olay işleyicisi içinde bir `if (!Page.IsPostBack)` koşullu. Geri başvurmak [disk belleği ve rapor verilerini sıralama](paging-and-sorting-report-data-vb.md) öğretici bilgi hakkında daha fazla bilgi için `Sort` yöntemi.


Varsayılarak veri sıralanmış, bizim sonraki görev hangi sütun belirlemektir veri göre sıralanmıştır ve s sütunu farklılıkları arayan satır taramak için değerleri. Aşağıdaki kod, veriler sıralanmıştır ve veri sıralandıktan sütun bulur sağlar:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

GridView varsa henüz olacak şekilde sıralanmış, GridView s `SortExpression` özellik ayarlanmamış. Bu nedenle, yalnızca bu özellik bir değer varsa ayırıcı satır eklemek istiyoruz. Destekliyorsa, biz sonraki olarak veri sıralanmıştır sütun dizini belirlemeniz gerekir. Bu GridView s döngüyle gerçekleştirilir `Columns` toplama, sütun için arama `SortExpression` özelliği eşittir GridView s `SortExpression` özelliği. Sütun s dizini ek olarak, biz de yakalayın `HeaderText` ayırıcı satırları görüntülenirken kullanılan özellik.

Tarafından verileri sıralama sütunu dizinde GridView satırlarını listeleme son adımdır. Her satır için biz sıralanmış sütun s değeri önceki satır sıralanmış s sütun s değerden farklı olup olmadığını belirlemeniz gerekir. Bu nedenle, yeni bir eklemesine ihtiyacımız olmadığını `GridViewRow` denetim hiyerarşiye örneği. Bu, aşağıdaki kod ile gerçekleştirilir:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Bu kod, program aracılığıyla başvurarak başlatır `Table` GridView s denetim hiyerarşisi kök dizininde bulunan nesne ve adlı bir dize değişkeni oluşturma `lastValue`. `lastValue` Geçerli satır sıralanmış s sütun değeri önceki satır s değerle karşılaştırmak için kullanılır. İleri, GridView s `Rows` koleksiyon numaralandırılan ve sıralanmış sütunun değeri depolanır her satır için `currentValue` değişkeni.

> [!NOTE]
> S hücre kullanmam belirli bir satır sıralanmış s sütunun değeri belirlemek için `Text` özelliği. Bu da BoundFields için çalışır ancak değil TemplateFields, CheckBoxFields, istendiği gibi çalışması ve benzeri. Alternatif GridView alanlar için kısa bir süre sonra hesap nasıl tümleştirildiği incelenmektedir.


`currentValue` Ve `lastValue` değişkenleri sonra karşılaştırılır. Bunlar farklıysa biz denetimi hiyerarşi için yeni bir ayırıcı satır eklemeniz gerekir. Bu dizini belirleyerek gerçekleştirilir `GridViewRow` içinde `Table` s nesnesi `Rows` koleksiyon, yeni oluşturma `GridViewRow` ve `TableCell` örnekleri ve ardından ekleyerek `TableCell` ve `GridViewRow` için denetim hiyerarşisi.

Ayırıcı s silmenizin satır Not `TableCell` GridView genişliğinin tamamını yayılan şekilde biçimlendirilmiş kullanılarak biçimlendirilmiş `SortHeaderRowStyle` CSS sınıfı ve onun `Text` gibi sıralama Grup gösterir name özelliği (Kategori gibi) ve Grup s değeri (örneğin, Meşrubat). Son olarak, `lastValue` değerine güncelleştirildi `currentValue`.

Sıralama Grup üstbilgi satırındaki biçimlendirmek için kullanılan CSS sınıfı `SortHeaderRowStyle` içinde belirtilmesi gerekiyor `Styles.css` dosya. Hangi stili ayarları için itiraz etmek kullanılacak çekinmeyin; Aşağıdaki kullandım:


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Geçerli koduyla herhangi BoundField tarafından sıralarken sıralama arabirimi sıralama grup üstbilgileri ekler (tedarikçi tarafından sıralama sırasında bir ekran görüntüsü gösterilmektedir Şekil 5 bakın). Ancak, herhangi bir alan türü (örneğin, bir CheckBoxField veya TemplateField) tarafından sıralarken sıralama grup üstbilgileri saklanıyorsa (bkz. Şekil 6) bulunacak değildir.


[![Sıralama arabirim sıralama grup üstbilgileri, BoundFields tarafından sıralarken içerir.](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Şekil 5**: sıralama arabirimi içeren sıralama grup üstbilgileri zaman sıralayarak BoundFields ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![Eksik olduğunda sıralama bir CheckBoxField sıralama grup üstbilgileri olan](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Şekil 6**: sıralama grup üstbilgileri olan eksik olduğunda sıralama bir CheckBoxField ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


Kod şu anda yalnızca kullandığından CheckBoxField tarafından sıralarken sıralama grup üstbilgileri eksik nedeni `TableCell` s `Text` her satır için sıralanmış sütunun değeri belirlemek için özellik. CheckBoxFields için `TableCell` s `Text` özellik boş bir dize; Bunun yerine, değer içinde bulunan bir onay kutusu Web denetimi aracılığıyla kullanılabilir `TableCell` s `Controls` koleksiyonu.

Alan türleri BoundFields dışında işlemek için kod büyütmek için ihtiyacımız nerede `currentValue` değişkeni CheckBox varlığını denetlemek için atanmış `TableCell` s `Controls` koleksiyonu. Yerine `currentValue = gvr.Cells(sortColumnIndex).Text`, bu kodu aşağıdakilerle değiştirin:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Bu kod sıralanan sütunu inceler `TableCell` herhangi bir denetim olup olmadığını belirlemek üzere geçerli satır için `Controls` koleksiyonu. Varsa ve bu ilk denetim bir onay kutusudur `currentValue` Evet veya Hayır, onay kutusu s bağlı olarak değişkeni ayarlanır `Checked` özelliği. Aksi halde değer alınırlar `TableCell` s `Text` özelliği. Bu mantık GridView bulunabilecek TemplateFields için sıralama işlemek için çoğaltılabilir.

Yukarıdaki kod eklenmesiyle sıralama grup üstbilgileri tarafından dönüştürülmesine CheckBoxField sıralarken mevcut sunulmuştur (bkz. Şekil 7).


[![Artık mevcut olduğunda sıralama bir CheckBoxField sıralama grup üstbilgileri olan](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Şekil 7**: sıralama grup üstbilgileri olan artık mevcut olduğunda sıralama bir CheckBoxField ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> Ürünleri ile varsa `NULL` veritabanı değerlerini `CategoryID`, `SupplierID`, veya `UnitPrice` alanları, bu değerler olarak görünür GridView boş dizeler ilebuürünleriçinayırıcısatırsmetinanlamıvarsayılan`NULL`değerleri gibi kategori okumak: (diğer bir deyişle, orada s kategori sonra ad: kategorisiyle ister: Meşrubat). Burada görüntülenen bir değeri isterseniz BoundFields ya da ayarlayabilirsiniz [ `NullDisplayText` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) görüntülenmesini istediğiniz metin veya bir koşullu ifade atarken işleme yönteminde ekleyebilirsiniz `currentValue` ayırıcı için Satır s `Text` özelliği.


## <a name="summary"></a>Özet

GridView sıralama arabirimini özelleştirmek için birçok yerleşik seçenekleri içermez. Bununla birlikte, alt düzey kodunun bir bit, s daha özelleştirilmiş bir arabirim oluşturmak için GridView s denetim hiyerarşisi ince ayar mümkündür. Bu öğreticide daha kolay farklı gruplar ve bu grupları sınırlar tanımlayan bir sıralanabilir GridView için bir sıralama Grup ayırıcı satır ekleme gördük. Özelleştirilmiş sıralama arabirimleri ek örnekler için kullanıma [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [sıralama birkaç ASP.NET 2.0 GridView ipuçları ve püf noktaları](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) blog girişi.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](sorting-custom-paged-data-vb.md)
