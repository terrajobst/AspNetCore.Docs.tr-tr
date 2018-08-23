---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: DataList veya Repeater denetiminde (C#) verileri sıralama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, verileri DataList veya Repeater oluşturmak nasıl yanı sıra destek DataList ve Repeater'ı sıralama ekleme inceleyeceğiz...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 05fbc51d5341a4d3d634cbbc05c0e66a827b0394
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752546"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>DataList veya Repeater denetiminde (C#) verileri sıralama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) veya [PDF olarak indirin](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> DataList veya Repeater verisini disk belleğine alınan ve sıralanmış oluşturmak nasıl yanı sıra destek DataList ve Repeater'ı sıralama ekleme, bu öğreticide inceleyeceğiz.


## <a name="introduction"></a>Giriş

İçinde [önceki öğreticide](paging-report-data-in-a-datalist-or-repeater-control-cs.md) biz bir DataList için sayfalama desteği ekleme incelenir. Yeni bir yöntemde oluşturduğumuz `ProductsBLL` sınıfı (`GetProductsAsPagedDataSource`) döndürülen bir `PagedDataSource` nesne. DataList veya Repeater ne zaman bir DataList veya Repeater bağlı, istenen sayfa veri yalnızca görüntüleyebilir. Bu teknik, ne dahili olarak tarafından FormView GridView ve DetailsView denetimlerini yerleşik varsayılan sayfalama işlevleri sağlamak için kullanılan benzer.

Disk belleği destek vermenin yanı sıra GridView sıralama desteği kullanıma hazır de içerir. DataList veya Repeater ne yerleşik sıralama işlevselliği sağlar. Ancak, sıralama özellikleri ile kod biraz eklenebilir. DataList veya Repeater verisini disk belleğine alınan ve sıralanmış oluşturmak nasıl yanı sıra destek DataList ve Repeater'ı sıralama ekleme, bu öğreticide inceleyeceğiz.

## <a name="a-review-of-sorting"></a>Sıralama bir gözden geçirme

İçinde gördüğümüz gibi [sayfalama ve sıralama rapor verileri](../paging-and-sorting/paging-and-sorting-report-data-cs.md) öğreticide GridView denetiminde sıralama desteği kullanıma hazır sağlar. Her GridView alanının bir ilişkili olabilir `SortExpression`, verileri sıralamak üzere veri alanını gösterir. Zaman GridView s `AllowSorting` özelliği `true`, sahip her GridView alan bir `SortExpression` özellik değerine sahip bir LinkButton işlenen üstbilgisi. Bir kullanıcı belirli bir GridView alan s başlığa tıkladığında, bir geri gönderme gerçekleşir ve veriler s tıklandı alanı göre sıralanır `SortExpression`.

GridView denetiminde sahip bir `SortExpression` depolayan özelliği de `SortExpression` GridView alanın veri değerlerine göre sıralanır. Ayrıca, bir `SortDirection` özelliği, veriler artan veya azalan (iki kez art arda, sıralama düzeni belirli bir GridView s alanı üstbilgi bağlantı açılıp bir kullanıcı tıklama değilse), yeniden sırılanacığını olup olmadığını gösterir.

GridView kendi veri kaynak denetimine bağlı olduğunda kapalı uygulamalı kendi `SortExpression` ve `SortDirection` özellikleri veri kaynağı denetimi. Veri kaynağı denetimini verileri alır ve ardından sağlanan göre sıralar `SortExpression` ve `SortDirection` özellikleri. Verileri sıralama sonra veri kaynağı denetimini GridView'a döndürür.

Bu işlev DataList veya Repeater denetimleri ile çoğaltmak için biz gerekir:

- Bir sıralama arabirimi oluşturma
- Sıralamanın yapılacağı veri alan ve artan veya azalan düzende sıralamak etkinleştirilip etkinleştirilmeyeceğini unutmayın
- Verileri belirli veri alana göre sıralamak için ObjectDataSource isteyin

Biz bu üç adım 3 ve 4 görevleri üstesinden. Hem sayfalama ve sıralama DataList veya Repeater desteği ekleme, inceleyeceğiz.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>2. adım: Repeater'da ürünleri görüntüleme

Repeater denetimiyle ürünleri listeleyerek Başlat s sıralama ile ilgili işlevleri uygulama hakkında endişe önce sağlar. Başlangıç açarak `Sorting.aspx` sayfasını `PagingSortingDataListRepeater` klasör. Repeater denetimiyle web ayar sayfasında, eklemek, `ID` özelliğini `SortableProducts`. Adlı yeni bir ObjectDataSource Repeater s akıllı etiketten oluşturma `ProductsDataSource` ve bu verileri almak için yapılandırma `ProductsBLL` s sınıfı `GetProducts()` yöntemi. (Hiçbiri) INSERT, UPDATE ve DELETE sekmeleri aşağı açılan listelerden seçeneğini seçin.


[![Bir ObjectDataSource oluşturun ve bunu GetProductsAsPagedDataSource() yöntemi kullanmak üzere yapılandırın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Şekil 1**: bir ObjectDataSource oluşturmak ve kullanmak için yapılandırma `GetProductsAsPagedDataSource()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Aşağı açılan listeler, ekleme, güncelleştirme ayarlayın ve sekme (hiçbiri) silme](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Şekil 2**: aşağı açılan listeler ayarlayın, ekleme, güncelleştirme ve silme (hiçbiri) için sekmeler ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


Farklı DataList'i ile Visual Studio otomatik olarak oluşturmaz bir `ItemTemplate` bir veri kaynağına bağlama sonra Repeater denetiminde için. Biz bu Ayrıca, eklemelisiniz `ItemTemplate` s DataList içinde bulunan Şablonları Düzenle seçeneğini Repeater denetim s akıllı etiket eksik olarak bildirimli olarak. Let s kullanın aynı `ItemTemplate` önceki öğreticide, görüntülenen s ürün adı, tedarikçi ve kategorisi.

Ekledikten sonra `ItemTemplate`, yineleyici ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Şekil 3, bir tarayıcıdan görüntülendiğinde bu sayfada görüntülenir.


[![Her bir ürün adı, üretici ve kategoriye s görüntülenir](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Şekil 3**: her ürün s adı, tedarikçi ve kategori görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>3. adım: verileri sıralamak için ObjectDataSource söyleyen

Yineleyicideki görüntülenen verileri sıralamak için biz veri sıralanmalıdır sıralama ifadesi ObjectDataSource bildirmeniz gerekir. ObjectDataSource verilerini almadan önce ilk harekete kendi [ `Selecting` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), bize bir sıralama ifadesi belirtmek bir fırsat sağlar. `Selecting` Olay işleyicisinin türü bir nesne geçirilen [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), adlı bir özellik olan [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) türü [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Sınıfı verilerle ilgili istekleri için veri kaynak denetimi veri bir tüketiciden geçirmek için tasarlanmıştır ve içeren bir [ `SortExpression` özelliği](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Sıralama bilgileri ObjectDataSource için ASP.NET sayfasından geçirmek için bir olay işleyicisi oluşturma `Selecting` olay ve aşağıdaki kodu kullanın:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

*SortExpression* verileri (örneğin, ProductName) göre sıralamak için veri alanı adını değer atanmalıdır. Sıralama yönü ile ilgili bir özellik yok, verileri azalan düzende sıralamak isterseniz, bu nedenle DESC dize eklemek için *sortExpression* değeri (örneğin, ProductName DESC).

Devam etmek ve denemek için bazı farklı sabit kodlanmış değerler *sortExpression* ve sonuçları bir tarayıcıda test edin. ProductName DESC olarak kullanırken, Şekil 4'te gösterildiği gibi *sortExpression*, ürün adında ters alfabetik düzende sıralanır.


[![Ürün adında ters alfabetik olarak sıralanır.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Şekil 4**: ürünleri ters alfabetik düzende kendi adına göre sıralanır ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>4. adım: sıralama arabirimi oluşturmak ve yönünü ve sıralama ifadesi hatırlama

GridView desteği sıralama kapatma her sıralanabilir alan s üst bilgi metni bir LinkButton dönüştürür, verileri tıklandığında, buna göre sıralar. Sıralama bir arabirim için burada verilerini düzgünce sütunlarında düzenlendiğini GridView mantıklıdır. DataList ve Repeater denetimleri, ancak farklı bir sıralama arabirimi gereklidir. Veri sıralanabilir alanları sağlayan bir açılır listede (aksine, bir kılavuz veri), veri listesi için ortak bir sıralama arabirimi var. Bu öğretici için böyle bir arabirim uygulamak s olanak tanır.

Yukarıdaki DropDownList Web denetim ekleme `SortableProducts` Yineleyici ve kümesi kendi `ID` özelliğini `SortBy`. Özellikler penceresinden, üç noktaya tıklayın `Items` özelliği ListItem Koleksiyonu Düzenleyicisi yukarı taşıyın. Ekleme `ListItem` verileri göre sıralamak için s `ProductName`, `CategoryName`, ve `SupplierName` alanları. Ayrıca bir `ListItem` ürünleri adında ters alfabetik düzende sıralamak.

`ListItem` `Text` Özellikleri (örneğin, adı), herhangi bir değere ayarlanabilir ancak `Value` özellikleri ayarlanmış olması gerekir (örneğin, ProductName) veri alanının adı. Sonuçlar, azalan düzende sıralamak için dize DESC ProductName DESC gibi veri alanı adını ekleyin.


![Sıralanabilir veri alanların her biri için bir ListItem Ekle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Şekil 5**: ekleme bir `ListItem` sıralanabilir veri alanların her biri için


Son olarak, bir düğme Web denetimi DropDownList sağında ekleyin. Ayarlama, `ID` için `RefreshRepeater` ve kendi `Text` yenileme özelliği.

Oluşturduktan sonra `ListItem` s ve Yenile düğmesini ekleme DropDownList ve düğme s Tanımlayıcı Sözdizimi görünmelidir aşağıdakine benzer:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Tam sıralama DropDownList ile sonraki ObjectDataSource s güncelleştirmek ihtiyacımız `Selecting` BT'nin seçili kullanması için olay işleyicisi `SortBy``ListItem` s `Value` özelliği bir sabit kodlanmış bir sıralama ifadesi değil.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

Sayfa ilk ziyaret edildiğinde bu noktada ürünleri tarafından başlangıçta sıralanacağını `ProductName` haliyle veri alanı s `SortBy` `ListItem` varsayılan olarak seçili (bkz. Şekil 6). Bir farklı seçenek kategorisi gibi sıralama ve yenileme tıklayarak seçerek bir geri göndermeye neden olur ve Şekil 7 gösterildiği gibi veri kategori adına göre yeniden sıralayın.


[![Başlangıçta kendi adına göre sıralanmış olan ürünler](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Şekil 6**: olan ürünler başlangıçta kendi adına göre sıralanır ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Kategoriye göre artık sıralanmış olan ürünler](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Şekil 7**: olan ürünler artık sıralanmış kategoriye göre ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Yenile düğmesine tıklanması, yineleyici s görünüm durumu devre dışı bırakıldı çünkü otomatik olarak böylece kendi veri kaynağına her geri gönderme üzerinde yeniden bağlamak bir yineleyici neden yeniden sıralanmış veriler neden olur. Önceden açılan sıralama değiştirme etkin, yineleyici s görünüm durumu bırakılırsa t kazanılan listesi sıralama düzeni herhangi bir etkisi sahip. Bu sorunu gidermek için yenile düğmesine s için bir olay işleyicisi oluşturma `Click` olay ve yeniden bağlamasını Yineleyicinin kendi veri kaynağına (s Repeater çağırarak `DataBind()` yöntemi).


## <a name="remembering-the-sort-expression-and-direction"></a>Yönünü ve sıralama ifadesi hatırlama

Sıralanabilir DataList veya Repeater burada olmayan sıralama ilgili Geri göndermeler oluşabilir, sayfada oluştururken, sıralama ifadesi ve yönü Geri göndermeler arasında anımsanacağını s zorunluluktur. Örneğin, Yineleyicinin bir Sil düğmesini her ürünle birlikte dahil etmek için bu öğreticideki güncelleştirdik olduğunu düşünün. Kullanıcı Sil düğmesine tıkladığında d seçili ürün silin ve ardından Repeater verileri yeniden bağlamanız için bazı kodlar çalıştırıyoruz. Sıralama ayrıntıları geri gönderme arasında kalıcı yapılmaz, ekranda görüntülenen verileri özgün sıralama düzeni olarak döner.

Bu öğreticide, DropDownList örtük olarak sıralama ifadesi ve yönü, Görünüm durumunda bizim için kaydeder. Farklı bir sıralama arabirim bir sağlanan çeşitli sıralama seçeneklerini örneğin LinkButtons birlikte kullanıyorsanız, d halletmeniz sıralama düzenini Geri göndermeler arasında hatırlamak ihtiyacımız var. Bu sorgu dizesi veya diğer durum Kalıcılık yöntemi aracılığıyla sıralama parametresini dahil ederek sıralama parametreleri sayfası s görünüm durumuna depolayarak sağlayabilirsiniz.

Bu öğreticide ileride örnekleri sıralama Ayrıntıları sayfası s görünüm durumu kalıcı hale getirmek nasıl keşfedin.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>5. adım: Varsayılan disk belleği kullanan bir DataList destek sıralama ekleme

İçinde [önceki öğretici](paging-report-data-in-a-datalist-or-repeater-control-cs.md) size nasıl bir DataList'i ile varsayılan sayfalama uygulama incelenir. S Sayfalanmış verileri sıralama özelliği eklemek için bu önceki örneği genişletmek olanak tanır. Başlangıç açarak `SortingWithDefaultPaging.aspx` ve `Paging.aspx` içinde sayfa `PagingSortingDataListRepeater` klasör. Gelen `Paging.aspx` sayfasında, sayfa s bildirim temelli biçimlendirme görüntülemek için kaynak düğmesine tıklayın. Seçili metni kopyalayın (bkz. Şekil 8) ve bildirim temelli biçimlerini yapıştırın `SortingWithDefaultPaging.aspx` arasında `<asp:Content>` etiketler.


[![Bildirim temelli işaretlemede çoğaltmak &lt;asp: Content&gt; Paging.aspx SortingWithDefaultPaging.aspx için etiketler](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Şekil 8**: bildirim temelli işaretlemede çoğaltmak `<asp:Content>` gelen etiketleri `Paging.aspx` için `SortingWithDefaultPaging.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Bildirim temelli biçimlendirme kopyaladıktan sonra özellikleri ve yöntemleri kopyalayın `Paging.aspx` sayfasında s arka plan kod sınıfı için arka plan kod sınıfa `SortingWithDefaultPaging.aspx`. Ardından, görüntülemek için bir dakikanızı ayırarak `SortingWithDefaultPaging.aspx` sayfasını bir tarayıcıda. Aynı işlevselliğe ve görünüm olarak göstermelidir `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Sayfalama ve sıralama yöntemi varsayılan içerecek şekilde ProductsBLL geliştirme

Önceki öğreticide oluşturduğumuz bir `GetProductsAsPagedDataSource(pageIndex, pageSize)` yönteminde `ProductsBLL` sınıfı döndüren bir `PagedDataSource` nesne. Bu `PagedDataSource` nesne ile doldurulmuş *tüm* ürünlerinin (BLL s aracılığıyla `GetProducts()` yöntemi), ancak zaman yalnızca belirtilen karşılık gelen kayıtları için DataList bağlı *PageIndex* ve *pageSize* giriş parametrelerini görüntülendi.

Bu öğreticide daha önce sıralama desteği ObjectDataSource s sıralama ifadesi belirterek ekledik `Selecting` olay işleyicisi. Bu işlemin çalıştığı da ObjectDataSource, gibi sıralanabilir bir nesne döndürüldüğünde `ProductsDataTable` tarafından döndürülen `GetProducts()` yöntemi. Ancak, `PagedDataSource` tarafından döndürülen nesne `GetProductsAsPagedDataSource` yöntemi kendi iç veri kaynağı sıralamayı desteklemiyor. Bunun yerine, döndürülen sonuçları sıralamak ihtiyacımız `GetProducts()` yöntemi *önce* biz bunu koymak `PagedDataSource`.

Bunu gerçekleştirmek için yeni bir yöntemde oluşturma `ProductsBLL` sınıfı `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Sıralanacak `ProductsDataTable` tarafından döndürülen `GetProducts()` yöntemini belirtin `Sort` varsayılan özelliği `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

`GetProductsSortedAsPagedDataSource` Yöntemi yalnızca biraz farklıdır gelen `GetProductsAsPagedDataSource` önceki öğreticide oluşturulan yöntemi. Özellikle, `GetProductsSortedAsPagedDataSource` ek giriş parametresi kabul eden `sortExpression` ve bu değeri atar `Sort` özelliği `ProductDataTable` s `DefaultView`. Kodu daha sonra birkaç satırlık `PagedDataSource` s veri kaynağı nesnesinin atandığı `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>GetProductsSortedAsPagedDataSource yöntemini çağırarak ve SortExpression giriş parametresinin değerini belirtme

İle `GetProductsSortedAsPagedDataSource` tam yöntemi, sonraki adım ise bu parametre için değer sağlamanız için. ObjectDataSource içinde `SortingWithDefaultPaging.aspx` çağırmak için şu anda yapılandırılmış `GetProductsAsPagedDataSource` yöntemi ve iki giriş parametresi, iki aracılığıyla geçişlerinde `QueryStringParameters`, içinde belirtilir `SelectParameters` koleksiyonu. Bu iki `QueryStringParameters` belirtmek için kaynak `GetProductsAsPagedDataSource` metodu s *PageIndex* ve *pageSize* parametreleri gelen sorgu dizesi alanlar `pageIndex` ve `pageSize`.

ObjectDataSource s güncelleştirme `SelectMethod` olan yeni çağırır özelliğini `GetProductsSortedAsPagedDataSource` yöntemi. Ardından, yeni bir ekleme `QueryStringParameter` böylece *sortExpression* giriş parametresi sorgu dizesi alanından erişilen `sortExpression`. Ayarlama `QueryStringParameter` s `DefaultValue` ProductName.

Bu değişikliklerden sonra ObjectDataSource s bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

Bu noktada, `SortingWithDefaultPaging.aspx` sayfa, sonuçlarını sıralama alfabetik olarak ürün adına göre (bkz. Şekil 9). Varsayılan olarak, bir ProductName değeri olarak geçirilen nedeni `GetProductsSortedAsPagedDataSource` metodu s *sortExpression* parametresi.


[![Varsayılan olarak, sonuçlar ProductName göre sıralanır.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Şekil 9**: varsayılan olarak, sonuçları tarafından sıralanır `ProductName` ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


El ile eklerseniz bir `sortExpression` gibi sorgu dizesi alanı `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` sonuçlar sıralanır tarafından belirtilen `sortExpression`. Ancak, bu `sortExpression` verilerin başka bir sayfaya taşıma sırasında parametre sorgu dizesinde değil dahil. Aslında, İleri'yi veya son sayfada tıklayarak izin ver düğmeleri bize geri alır `Paging.aspx`! Ayrıca, orada s şu anda hiçbir sıralama arabirim. Bir kullanıcı Sayfalanmış verileri sıralama düzenini değiştirebilirsiniz yalnızca doğrudan sorgu dizesini işleyerek yoludur.

## <a name="creating-the-sorting-interface"></a>Sıralama arabirimi oluşturma

Biz öncelikle güncelleştirmeniz gerekiyor `RedirectUser` kullanıcıya gönderilecek yöntemi `SortingWithDefaultPaging.aspx` (yerine `Paging.aspx`) ve dahil edilecek `sortExpression` sorgu dizesi değeri. Biz de bir salt okunur sayfa adlı düzeyinde eklemeniz gerekir `SortExpression` özelliği. Bu özellik, benzer `PageIndex` ve `PageSize` özellikleri önceki öğreticide oluşturulan değeri döndürür `sortExpression` varsa, sorgu dizesi alanı ve varsayılan değer (ProductName) Aksi takdirde.

Şu anda `RedirectUser` yöntemi yalnızca tek bir giriş parametresi görüntülemek için sayfanın dizini kabul eder. Ancak biz sorgu dizesinde belirtilen hangi s dışındaki bir sıralama ifadesi kullanarak verilerin belirli bir sayfada kullanıcı yönlendirmek istediğinizde zamanlar olabilir. Bir dakika içinde bir dizi veri belirtilen sütuna göre sıralama düğmesini Web denetimleri içeren bu sayfa için sıralama arabirimi oluşturacağız. Bu düğmelerden birine tıklandığında uygun sıralama ifadesi değerini geçirir kullanıcı yeniden yönlendirme istiyoruz. Bu işlevselliği sağlayacak şekilde iki sürümünü oluşturma `RedirectUser` yöntemi. Birinci, ikinci sayfa dizini ve sıralama ifadesi kabul ederken görüntülenecek yalnızca sayfa dizini kabul etmelidir.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Bu öğreticide ilk örnekte, bir sıralama arabirimi bir DropDownList kullanarak oluşturduk. Bu örnekte, let s DataList birini sıralama için yerleştirilmiş üç düğme Web denetimleri kullanın `ProductName`, diğeri için `CategoryName`, diğeri `SupplierName`. Ayarı üç düğme Web denetimleri ekleme, `ID` ve `Text` özellikleri uygun şekilde:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Ardından, oluşturun bir `Click` her olay işleyicisi. Olay işleyicileri çağırmalıdır `RedirectUser` kullanıcı uygun sıralama ifadesi kullanarak ilk sayfaya döndüren yöntemi.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Sayfa ilk ziyaret edildiğinde, veriler ürün adına göre alfabetik olarak sıralanır (Şekil 9'a geri bakın). Veri ikinci sayfasına ilerleyin ve sıralama kategori düğme tarafından ardından İleri düğmesine tıklayın. Bu bize veri, kategori adına göre sıralanmış ilk sayfasına döndürür (bkz. Şekil 10). Benzer şekilde, tedarikçi düğmesi göre sıralama verileri başlayarak verilerin ilk sayfadan tedarikçi göre sıralar. Sıralama seçeneği aracılığıyla veriler havuzda gibi hatırlanır. Şekil 11, kategoriye göre sıralama ve ardından veri 13 sayfasına ilerledikten sonra sayfada gösterilir.


[![Ürün kategorisine göre sıralanır.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Şekil 10**: ürün kategorisine göre sıralanır ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![Hatırlanan, disk belleği aracılığıyla veri sıralama ifadesi olan](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Şekil 11**: sıralama ifadesi olan anımsanacak sayfalama aracılığıyla verilerinin ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>6. adım: Bir yineleyici kayıtları arasında özel disk belleği

DataList örnek 5 sayfa verimsiz varsayılan sayfalama tekniği kullanarak, veriler aracılığıyla adımda incelenir. Yeterince büyük miktarda veriyi disk belleği, özel disk belleği kullanılması zorunludur. Geri [verimli bir şekilde sayfalama aracılığıyla büyük miktarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) ve [özel disk belleğine alınan veri sıralama](../paging-and-sorting/sorting-custom-paged-data-cs.md) öğreticiler, biz incelenmesi için BLL varsayılan ve özel disk belleği ve oluşturulan yöntemler arasındaki farkları özel disk belleği ve özel Sayfalanmış verileri sıralama yararlanarak. Özellikle, bu iki önceki öğreticilerdeki aşağıdaki üç yöntemi ekledik `ProductsBLL` sınıfı:

- `GetProductsPaged(startRowIndex, maximumRows)` belirli bir alt başlayan kayıtları döndürür *startRowIndex* ve aşmadan *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` kayıtları belirtilen göre sıralanmış belirli bir alt kümeyi döndürür *sortExpression* giriş parametresi.
- `TotalNumberOfProducts()` kayıtlarının toplam sayısını sağlar `Products` veritabanı tablosu.

Bu yöntemler kullanarak bir DataList veya Repeater denetiminde verileri sıralama ve etkili bir şekilde sayfasında kullanılabilir. Bunu açıklamak üzere; ile özel sayfalama desteğini Repeater denetimiyle oluşturarak başlayın s sağlar; sıralama özellikleri daha sonra ekleyeceğiz.

Açık `SortingWithCustomPaging.aspx` sayfasını `PagingSortingDataListRepeater` klasör ve bir yineleyici sayfasına ekleyin, `ID` özelliğini `Products`. Adlı yeni bir ObjectDataSource Repeater s akıllı etiketten oluşturma `ProductsDataSource`. Kendi verileri seçmek için yapılandırma `ProductsBLL` s sınıfı `GetProductsPaged` yöntemi.


[![ObjectDataSource s ProductsBLL sınıfı GetProductsPaged yöntemi kullanmak üzere yapılandırma](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` s sınıfı `GetProductsPaged` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Güncelleştirme, ekleme, açılan listeler ayarlayın, sekmeleri (hiçbiri) SİLİN ve ardından İleri düğmesine tıklayın. Veri Kaynağı Yapılandırma Sihirbazı'nı şimdi kaynakları için ister `GetProductsPaged` metodu s *startRowIndex* ve *maximumRows* giriş parametreleri. Çünkü, bu giriş parametreleri yok sayılır. Bunun yerine, *startRowIndex* ve *maximumRows* değerleri geçirilir aracılığıyla `Arguments` ObjectDataSource s özelliğinde `Selecting` nasıl belirlemiş olduğu gibi olay işleyicisi *sortExpression* de Bu öğretici s ilk Tanıtımı. Bu nedenle, parametre kaynağıyla None Ayarlama Sihirbazı'nda açılan listeler bırakın.


[![Parametre kaynakları kümesi yok olarak bırakın.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Şekil 13**: parametre kümesi kaynakları yok olarak bırakın ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Yapmak *değil* ObjectDataSource s ayarlamak `EnablePaging` özelliğini `true`. Bu otomatik olarak kendi içerecek şekilde ObjectDataSource neden *startRowIndex* ve *maximumRows* parametreleri `SelectMethod` s mevcut parametre listesi. `EnablePaging` Özelliği, bu denetimler belirli davranış, s ObjectDataSource beklediğiniz olduğundan bağlama özel veri FormView GridView veya DetailsView denetimi için disk belleğine alınan gerektiğinde kullanışlıdır yalnızca kullanılabilir `EnablePaging` özelliği `true`. Biz DataList ve Repeater sayfalama desteğini el ile eklemek zorunda olduğundan, bu özellik kümesine bırakın `false` (varsayılan) olarak size gerekli işlevleri doğrudan bizim ASP.NET sayfası içinde hazırlama.


Son olarak, s yineleyici tanımlamak `ItemTemplate` böylece s ürün adı, kategori ve tedarikçi gösterilir. Yineleyici ve ObjectDataSource s Tanımlayıcı Sözdizimi bu değişikliklerden sonra aşağıdakine benzer görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve kayıt döndürülür not için bir dakikanızı ayırın. Bunun nedeni, size henüz belirtmek için ve *startRowIndex* ve *maximumRows* parametre değerlerini; bu nedenle, değeri 0 olarak her ikisi için geçirilen. Bu değerleri belirtmek için bir olay işleyicisi ObjectDataSource s için oluşturma `Selecting` olay ve bu parametrelerin değerlerini programlı olarak sabit kodlanmış değerleri 0 ile 5, sırasıyla ayarla:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Bu değişiklik, bir tarayıcıdan görüntülendiğinde sayfada, ilk beş ürün gösterilir.


[![İlk beş kayıt görüntülenir](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Şekil 14**: ilk beş kayıt görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Şekil 14'te listelenen ürünler, çünkü ürün adına göre sıralanacak meydana `GetProductsPaged` verimli özel disk belleği sorgu gerçekleştirir saklı yordam sonuçlarına göre sıralar `ProductName`.


Sayfalar arasında adım kullanıcı için başlangıç satır dizini ve en fazla satır izlemek ve bu değerleri Geri göndermeler arasında gerekiyor. Varsayılan disk belleği örneğinde bu değerleri kalıcı hale getirmek için sorgu dizesi alanlar kullanılan; Bu Tanıtım için bu sayfayı s görünüm durumu bilgileri kalıcı s olanak tanır. Aşağıdaki iki özelliği oluşturun:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Ardından, kullandığı seçme olay işleyicisinde kodu güncelleştirmeniz `StartRowIndex` ve `MaximumRows` 0 ve 5 sabit kodlanmış değerler yerine özellikleri:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

Bu noktada sayfamızda, yalnızca ilk beş kaydı yine de gösterilir. Ancak, şu özelliklere sahip bir yerde biz yeniden disk belleği arabirimimizi oluşturmak için hazır.

## <a name="adding-the-paging-interface"></a>Disk belleği arabirim ekleme

Let s kullanımı aynı ilk, önceki son sayfalama yanında, arabirim, hangi veri sayfasını görüntüleyen denetim görüntülenebilir etiketi Web dahil olmak üzere varsayılan sayfalama örnek ve kaç toplam sayfa mevcut kullanılır. Dört düğme Web denetimleri ve yineleyici altına etiketi ekleyin.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Ardından, oluşturma `Click` dört düğme için olay işleyicileri. Aşağıdaki düğmelerden birine tıklandığında güncelleştirmek ihtiyacımız `StartRowIndex` ve yineleyici verileri yeniden bağlayın. Kod ilk, önceki ve sonraki düğmeleri için yeteri kadar basittir, ancak son düğme için nasıl veri son sayfası için başlangıç satır dizini belirleriz? Bu dizin yanı sıra İleri ve son düğmeleri etkinleştirilmesi gerekip gerekmediğini aracılığıyla toplamda kaç kaydın havuzda bilmek ihtiyacımız belirlemek mümkün hesaplamak için. Biz çağırarak belirleyebilirsiniz `ProductsBLL` s sınıfı `TotalNumberOfProducts()` yöntemi. Let s oluşturma adlı bir salt okunur, sayfa düzeyi özellik `TotalRowCount` sonuçları döndüren `TotalNumberOfProducts()` yöntemi:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Bu özellik artık son sayfa s başlangıç satır dizini belirleyebiliriz. Özellikle, s tamsayı sonucu, `TotalRowCount` eksi 1 bölü `MaximumRows`, tarafından çarpılan `MaximumRows`. Artık yazabiliriz `Click` dört sayfalama arabirimi düğme için olay işleyicileri:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Son olarak, veri ve sonraki ve son düğmeleri'nın ilk sayfasında son sayfayı görüntülerken görüntülerken disk belleği arabiriminde ilk ve önceki düğmeleri devre dışı bırakmak ihtiyacımız var. Bunu gerçekleştirmek için aşağıdaki kodu ekleyin ObjectDataSource s `Selecting` olay işleyicisi:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Bunlar ekledikten sonra `Click` olay işleyicileri ve kodu etkinleştirmek veya devre dışı geçerli başlangıç satır dizinini temel alarak sayfalama arabirimi öğeleri için test sayfası tarayıcıda. İlk sayfa ilk ziyaret edildiğinde Şekil 15 gösterir ve önceki düğmeleri olacak şekilde devre dışı bırakıldı. İleri'ye tıklama gösterir, verilerin ikinci sayfasında son tıklayarak son sayfayı görüntülerken (Şekil 16. ve 17 bakın). Verilerin son sayfayı görüntülerken İleri ve son düğmeleri devre dışı bırakıldı.


[![Önceki ve son düğmeler, ilk sayfa ürünleri görüntülerken devre dışı bırakıldı](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Şekil 15**: önceki ve son düğmeleri devre dışı bırakıldı, ilk sayfa ürünleri görüntülerken ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![Dispalyed olan, ikinci sayfa ürünler](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Şekil 16**: İkinci sayfa olan olan ürünler Dispalyed ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Tıklayarak son görüntüler verinin son sayfa](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Şekil 17**: son tıklayarak, son sayfasında veri görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>7. adım: Özel desteğiyle sıralama dahil olmak üzere Repeater disk belleği

Özel disk belleği uygulanmıştır, yeniden sıralama dahil etmek için hazır destekliyoruz. `ProductsBLL` s sınıfı `GetProductsPagedAndSorted` yöntemi aynı olan *startRowIndex* ve *maximumRows* giriş parametreleri olarak `GetProductsPaged`, ancak bir ek izin veren  *sortExpression* giriş parametresi. Kullanılacak `GetProductsPagedAndSorted` yönteminden `SortingWithCustomPaging.aspx`, aşağıdaki adımları gerçekleştirmeniz gerekir:

1. ObjectDataSource s değiştirme `SelectMethod` özelliğinden `GetProductsPaged` için `GetProductsPagedAndSorted`.
2. Ekleme bir *sortExpression* `Parameter` ObjectDataSource s nesnesine `SelectParameters` koleksiyonu.
3. Bir özel, sayfa düzeyi oluşturma `SortExpression` değerini s sayfanın görünüm durumu aracılığıyla Geri göndermeler arasında kalıcı bir özelliği.
4. ObjectDataSource s güncelleştirme `Selecting` ObjectDataSource s atamak için olay işleyicisi *sortExpression* parametre değeri, sayfa düzeyi `SortExpression` özelliği.
5. Sıralama arabirimi oluşturun.

Başlangıç ObjectDataSource s güncelleştirerek `SelectMethod` özelliği ve ekleyerek bir *sortExpression* `Parameter`. Emin olun *sortExpression* `Parameter` s `Type` özelliği `String`. Bu ilk iki görevleri tamamladıktan sonra bildirim temelli biçimlendirme ObjectDataSource s aşağıdaki gibi görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Ardından, sayfa düzeyi ihtiyacımız `SortExpression` özellik değeri durumunu görüntülemek için serileştirilir. Herhangi bir sıralama ifadesi değer ayarlarsanız ProductName varsayılan olarak kullanın:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

ObjectDataSource çağırır önce `GetProductsPagedAndSorted` yöntemi ayarlamak için ihtiyacımız *sortExpression* `Parameter` değerine `SortExpression` özelliği. İçinde `Selecting` olay işleyicisi, aşağıdaki kod satırını ekleyin:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Kalan tek şey sıralama arabirim uygulamak için. Son örnekte yaptığımız gibi sonuçları sıralamak kullanıcının olanak tanıyan üç düğme Web denetimleri kullanarak, ürün adı, kategori veya sağlayıcı tarafından uygulanan sıralama arabirimine sahip s olanak tanır.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Oluşturma `Click` bu üç düğme denetimleri için olay işleyicileri. Olay işleyicisi, sıfırlama `StartRowIndex` 0'a ayarlanmış `SortExpression` uygun değeri ve yeniden bağlamasını Repeater verileri:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Tüm var. Bu s için İşte bu kadar! Bir dizi özel sayfalama ve sıralama uygulanan alma adımlarını kopyalanırken adımları varsayılan sayfalama gerekenler için çok benzer. Kategoriye göre sıralanmış veri son sayfayı görüntülerken, Şekil 18 ürünlerini gösterir.


[![Son sayfa verileri, kategoriye göre Sorted görüntülenir](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Şekil 18**: son sayfa, verileri, kategoriye göre Sorted görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> Üretici, sıralama ifadesi kullanılan sağlayıcı tarafından sıralarken önceki örneklerde. Ancak, özel sayfalama uygulama için biz CompanyName kullanmanız gerekir. Bunun nedeni, özel disk belleği uygulamak için sorumlu saklı yordamı `GetProductsPagedAndSorted` sıralama ifadesine geçirir `ROW_NUMBER()` anahtar sözcüğü, `ROW_NUMBER()` anahtar sözcüğü, bir diğer ad yerine gerçek sütun adı gerektirir. Bu nedenle, biz kullanmalısınız `CompanyName` (sütun adı `Suppliers` tablo) kullanılan diğer adı yerine `SELECT` sorgu (`SupplierName`) sıralama ifadesi için.


## <a name="summary"></a>Özet

Ne DataList veya Repeater sıralama yerleşik destek sağlar, ancak bu işlevselliğin bir bit kod ve özel bir sıralama arabirim eklenebilir. Aracılığıyla sıralama, ancak değil sayfalama uygularken, sıralama ifadesi belirtilebilir `DataSourceSelectArguments` nesnesi geçirildi ObjectDataSource s ile `Select` yöntemi. Bu `DataSourceSelectArguments` s nesnesi `SortExpression` özelliği ObjectDataSource s'te atanabilir `Selecting` olay işleyicisi.

DataList veya Repeater sayfalama desteği sağlayan sıralama yetenekleri eklemek için bir sıralama ifadesi kabul eden bir yönteme içerecek şekilde iş mantığı katmanı özelleştirmek için en kolay yaklaşım olduğu. Bu bilgiler daha sonra bir parametre ObjectDataSource s üzerinden geçirilebilir `SelectParameters`.

Bu öğreticide, sayfalama ve sıralama DataList ve Repeater denetimleri ile bizim İnceleme tamamlar. İleri ve son öğreticimize öğesi başına temelinde, kullanıcı tarafından başlatılan özel işlevsellik sağlamak için DataList ve Repeater s şablonlara düğmesi Web denetimleri ekleme inceleyeceksiniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme David Suru oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [İleri](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
